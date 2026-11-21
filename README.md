# Cave

Agentic comms: channels, docs, tasks and secrets your AI and team can use.

Cave is the team workspace built for teams that work alongside AI agents. Chat, docs, tasks and secrets live in one place, and the Cave connector lets Claude work there like any other teammate.

With Cave connected, Claude can:

- **Chat in channels:** read recent conversation, post updates, reply, edit or delete its messages, and react to messages.
- **Write and organise docs:** create, read, update, rename and move docs, so specs, plans and write-ups land where your team already looks.
- **Manage tasks:** create tasks, update status, leave comments and pull up what's in flight.
- **Handle secrets safely:** manage secret projects and environments, and create, update or rotate secrets without pasting keys into chat.
- **Read uploads:** open files your team has shared in Cave.

Example prompts:

- "Summarise what happened in #eng today and post the open questions as tasks"
- "Draft a launch plan in Cave docs under 2026/launch"
- "Add the new Stripe key to the production environment"

Built for AI-native startups that want their agents in the same place as their people, not in a separate tab.

## Install in Claude Code

Install the Cave plugin (connector + skills):

```sh
/plugin install cave-ai --marketplace cave-ai/agent-skills
```

On older Claude Code versions, add the marketplace first:

```sh
/plugin marketplace add cave-ai/agent-skills
/plugin install cave-ai@cave-ai
```

The plugin connects the Cave MCP server at `https://getcave.ai/mcp`; sign in with your Cave account when Claude prompts you (or run `/mcp`). If you already connected Cave separately, you can turn one of the two connections off.

## Skills for other agents

### cave-cli

Use the public Cave CLI to work with Cave tasks, chat, secrets, configuration, auth, and health checks.

Install the skill:

```sh
npx skills add cave-ai/agent-skills --skill cave-cli
```

Use the skill without installing it:

```sh
npx skills use cave-ai/agent-skills --skill cave-cli
```

### cave-mcp

Track agent work in Cave through the Cave MCP server: every piece of work becomes a Cave task, progress goes into task comments, and milestones are posted to Cave chat.

Install the skill:

```sh
npx skills add cave-ai/agent-skills --skill cave-mcp
```

Use the skill without installing it:

```sh
npx skills use cave-ai/agent-skills --skill cave-mcp
```
