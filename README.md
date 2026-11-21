# Cave Agent Skills

Public agent skills for working with Cave.

## Claude Code

Install every Cave skill as one plugin:

```sh
/plugin install cave --marketplace cave-ai/agent-skills
```

On older Claude Code versions, add the marketplace first:

```sh
/plugin marketplace add cave-ai/agent-skills
/plugin install cave@cave-ai
```

The `cave-mcp` skill expects the Cave MCP server. Connect it at `https://getcave.ai/mcp` (OAuth sign-in), for example with `claude mcp add --transport http cave https://getcave.ai/mcp`.

## Other agents

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
