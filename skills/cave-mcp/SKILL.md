---
name: cave-mcp
description: Track all agent work in Cave through the Cave MCP server (cave_* tools). Use this skill whenever the Cave MCP tools are available and you are asked to do any piece of work — code changes, investigations, fixes, research, ops tasks — so the work is created as a Cave task and kept up to date through task comments and Cave chat. Also use it when the user asks to read or update Cave tasks, channels, messages, or docs.
---

# Cave MCP Skill

Cave is a team workspace with tasks, chat channels, and docs. When the Cave MCP server is connected, its tools are named `cave_*` (for example `cave_create_task`; some clients prefix them, such as `mcp__Cave_AI__cave_create_task`).

**Rule: every piece of work is a Cave task.** Before you start, make sure a task exists for it. While you work, keep the task and the team's chat updated. When you finish, close the task with a summary. The team should be able to follow your work in Cave without reading your session.

## What counts as work

Track it in Cave when the request asks you to produce or change something: write or change code, fix a bug, investigate an issue, run a migration or deploy, write a doc, do research that ends in a recommendation.

Do not create a task for a quick question you answer directly (explain this function, what does this error mean) or for reading Cave itself. If unsure, create the task — an extra task is cheaper than invisible work.

## Workflow

### 1. Find or create the task

1. If the user gave a task ID or link, call `cave_get_task` and use that task.
2. Otherwise search for an existing task first so you don't duplicate one: `cave_list_tasks` with `q` set to a few keywords from the request (check `open`, `in_progress`, and `backlog`).
3. If nothing matches, call `cave_create_task`:
   - `title`: short imperative summary, e.g. `Fix login redirect loop on mobile`.
   - `description`: the request in your own words, the goal, and what "done" means. Include links (repo, branch, PR, issue) when you have them.
   - `status`: `in_progress` if you are starting now, `open` or `backlog` if you are only filing it.
   - `priority`: what the user stated; otherwise `no_priority`. Use `urgent` only when the user says so.
   - `assigneeUserId` / `dueAt`: only when the user gives them.
4. Tell the user the task ID you are working under.

If you picked up an existing task that is not `in_progress`, set it with `cave_update_task`.

### 2. Announce the work in chat

Post one short message in the relevant channel with `cave_send_message`:

- Use the channel the user named, or the channel the request came from.
- Otherwise call `cave_list_channels` and pick the channel that clearly matches the project or team. If none clearly matches, skip chat and use task comments only — do not guess or create a channel unless asked.
- Start a thread for the work: post the kickoff message, then pass its message ID as `threadParentMessageId` on your next message; after that, reply with `threadId`. Keep all later updates for this task in that thread rather than the main channel.

Kickoff format:

```md
Starting: <task title> (task <task-id>)
Plan: <one or two lines>
```

Record the channel ID and thread ID in a task comment so later sessions can continue in the same thread.

### 3. Update as you go

Comment on the task with `cave_comment_task` (Markdown) at meaningful milestones, not on every tool call:

- Plan settled or changed.
- A significant finding (root cause found, approach rejected and why).
- Something shipped: commit, branch, PR, deploy — with links.
- Blocked: what you need and from whom. Also post this in the chat thread, since it needs a person.

Keep each comment a few lines: what happened, what's next. Mirror only important milestones (blocked, PR ready, done) to the chat thread; the task comments hold the detail.

Update task fields with `cave_update_task` when they change — refine the `title` or `description` if the scope changes, and adjust `priority` if the user says so. Only the fields you pass are changed.

### 4. Finish

1. Post a final task comment: what was done, links to PRs/commits/docs, how it was verified, and any follow-ups.
2. Set `status` to `done` with `cave_update_task` — only when the work is actually complete. If you stop partway, leave it `in_progress` (or `open` if nobody is on it) and say in the comment what remains.
3. Post a one-line wrap-up in the chat thread: `Done: <title> — <link to PR or result>`.
4. File follow-ups you discovered as new tasks (`backlog` or `open`), and link them in the final comment.

### Multiple pieces of work

If one request contains several independent pieces of work, create one task per piece. Small steps of a single piece belong in comments, not separate tasks.

## Reading Cave

- Tasks: `cave_list_tasks` (filter by `status`, `assigneeUserId`, `q`; page with `cursor`), `cave_get_task`.
- Chat: `cave_list_channels` to get a channel ID, then `cave_list_messages` (pass `threadId` to read a thread). Read the thread before posting into it so you don't repeat what others said.
- Docs: `cave_list_docs` for doc and folder IDs, `cave_get_doc` to read as Markdown, `cave_create_doc` / `cave_update_doc` to write. Put long write-ups (design notes, investigation reports) in a doc and link it from the task instead of pasting them into comments.

Statuses: `backlog`, `open`, `in_progress`, `done`. Priorities: `no_priority`, `low`, `medium`, `high`, `urgent`.

Write tools return an intent ID once the change is applied. If Cave rejects a write, it fails with a reason — fix the input and retry once, or tell the user.

## Etiquette

- Task comments and chat messages are visible to the team. Write them for teammates: plain, short, no internal reasoning dumps.
- Never put secrets, tokens, or credentials in tasks, comments, messages, or docs. Secret tools (`cave_list_secret_*`, `cave_create_secret`, `cave_update_secret`) can set values but never read them back; refer to secrets by key name only.
- Don't delete tasks, messages, docs, or secrets unless the user explicitly asks.
- Don't spam: one kickoff, milestone updates, one wrap-up per task.
