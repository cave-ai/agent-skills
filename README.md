# Cave Agent Skills

Public agent skills for working with Cave.

## Skills

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
