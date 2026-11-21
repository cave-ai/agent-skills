---
name: cave-cli
description: Use the Cave CLI (`@cave-ai/cli`, binary `cave`) to manage Cave tasks, chat, secrets, and service health from a terminal or coding agent. Use it when the user wants to install or run the Cave CLI or operate Cave from the command line.
---

# Cave CLI

Cave is a team workspace with tasks, chat channels, and docs. Its public CLI package is `@cave-ai/cli`, and the installed binary is `cave`. Prefer explicit JSON output when results will be parsed or used by another command.

## Install

```sh
npx @cave-ai/cli@latest --help
npm install -g @cave-ai/cli
```

## Sign in and verify

Sign in through the browser, naming the coding agent you are running as (`claude` or `codex`):

```sh
cave auth login --agent claude
cave auth status --format json
```

The user completes sign-in in their browser. Never ask the user to paste API keys or other credentials into chat, and never print or commit them.

If a command cannot reach Cave, run:

```sh
cave config show --format json
cave health ready --format json
```

## Output contracts

Use `--format json` or `-f json` when consuming command output programmatically. Many commands also support `--schema`, which prints the JSON response contract without calling the API:

```sh
cave task list --schema --format json
cave chat channels --schema --format json
```

## Tasks

```sh
cave task list --format json
cave task list --status open --limit 10 --format json
cave task list --q "cli" --format json
cave task get <task-id> --format json

cave task create "Ship CLI setup" --status open --priority medium --format json
cave task update <task-id> --status done --format json
cave task delete <task-id> --format json
cave task comment "Looks good to me" <task-id> --source claude --format json
```

Task statuses are `backlog`, `open`, `in_progress`, and `done`. Priorities are `no_priority`, `urgent`, `low`, `medium`, and `high`.

## Chat

```sh
cave chat channels --format json
cave chat create-channel general --type user-public --format json
cave chat messages <channel-id> --limit 20 --format json
cave chat send-message <channel-id> "Hello from Cave" --source claude --format json
cave chat update-message "Updated message" <message-id> --format json
cave chat delete-message <message-id> --format json
cave chat toggle-reaction ":wave:" <message-id> --format json
```

Resolve channel and message IDs from JSON output before using them in mutations.

## Secrets

Cave stores team secrets in projects and environments. Read key names, not values:

```sh
cave secrets projects --format json
cave secrets environments <project-id> --format json
cave secrets list <environment-id> --format json
```

Create, update, or delete secrets only when the user explicitly asks, and let the user supply the value themselves rather than pasting it into chat. Never reveal, export, log, or repeat secret values.

## MCP

For agents, the Cave MCP server is the simplest way to reach Cave tools. Connect the remote server at `https://getcave.ai/mcp` (OAuth sign-in):

```sh
claude mcp add --transport http cave https://getcave.ai/mcp
```

With the MCP server connected, use the `cave-mcp` skill to track work as Cave tasks.
