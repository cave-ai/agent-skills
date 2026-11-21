---
name: cave-cli
description: Use the public Cave CLI to work with Cave tasks, chat, secrets, configuration, auth, and health checks. Use this skill when the user wants to install or run `@cave-ai/cli`, configure `cave`, manage Cave tasks or chat from a terminal, export Cave secrets, or inspect Cave service readiness.
---

# Cave CLI Skill

Use this skill when operating Cave from the command line.

Cave is a real-time messaging platform. Its public CLI package is `@cave-ai/cli`, and the installed binary is `cave`.

## Install

Run without installing globally:

```sh
npx @cave-ai/cli@latest --help
```

Install globally:

```sh
npm install -g @cave-ai/cli
cave --help
```

Update a global install:

```sh
npm update -g @cave-ai/cli
```

## Configure

For production Cave, point the CLI at the API and store an API key:

```sh
cave config set-api-url https://api.getcave.ai
cave auth set YOUR_API_KEY
cave auth status
```

For one shell session, use environment variables instead:

```sh
export CAVE_API_URL=https://api.getcave.ai
export CAVE_API_KEY=YOUR_API_KEY
cave health ready
```

The persisted config file is:

```txt
~/.config/cave/config.json
```

Environment variables override the config file.

## Output

Most read commands support machine-readable output:

```sh
cave task list --format json
cave chat channels --format json
cave secrets projects --format json
```

Use JSON output when another tool or script needs to parse results. Use pretty output for human inspection.

Many commands also support `--schema` to print the response contract instead of calling the API.

## Auth And Config

```sh
cave auth set YOUR_API_KEY
cave auth status
cave auth clear

cave config show
cave config set-api-url https://api.getcave.ai
```

Never print or commit API keys. Prefer environment variables for short-lived automation and the config file for local interactive use.

## Health

Check service readiness:

```sh
cave health ready
```

Run this after changing API URL or credentials to confirm the CLI can reach Cave.

## Tasks

List and inspect tasks:

```sh
cave task list
cave task list --status open --limit 10
cave task list --q "cli" --format json
cave task get <task-id>
```

Create, update, and delete tasks:

```sh
cave task create "Ship CLI README" --status open --priority medium
cave task update <task-id> --status done
cave task delete <task-id>
```

Useful aliases:

```sh
cave task ls
cave task rm <task-id>
```

Task statuses are `backlog`, `open`, `in_progress`, and `done`. Task priorities are `no_priority`, `urgent`, `low`, `medium`, and `high`.

## Chat

List and create channels:

```sh
cave chat channels
cave chat create-channel general --type public
```

Read and send messages:

```sh
cave chat messages <channel-id>
cave chat send-message <channel-id> "Hello from the CLI"
cave chat toggle-reaction <message-id> ":wave:"
```

Use `--format json` when you need channel IDs or message IDs for follow-up commands.

## Secrets

The `secrets` command also has the alias `secret`.

Manage projects and environments:

```sh
cave secrets projects
cave secrets create-project app --description "Application secrets"
cave secrets environments <project-id>
```

Manage individual secrets:

```sh
cave secrets list <environment-id>
cave secrets create <environment-id> API_KEY <value>
cave secrets update <secret-id> --value <new-value>
cave secrets reveal <secret-id>
cave secrets delete <secret-id>
```

Useful aliases:

```sh
cave secret ls <environment-id>
cave secret rm <secret-id>
```

Export secrets for local tooling:

```sh
cave secrets export --environment-id <environment-id> --format env
cave secrets export --project-id <project-id> --format env --output .env.local
```

Use `--project-id` to export the project's default environment. Pass either `--environment-id` or `--project-id`, not both.

Treat revealed and exported secret values as sensitive. Do not paste them into chat, commit them, or include them in logs.

## Troubleshooting

If a command cannot reach Cave, check the API URL first:

```sh
cave config show
cave health ready
```

If authentication fails, verify credentials without exposing the key:

```sh
cave auth status
```

If scripting a command, prefer `--format json` and parse IDs from the response instead of relying on pretty output.
