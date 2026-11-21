# Cave Agent Skills

Public agent skills for working with Cave.

## Claude Code

Install every Cave skill as one plugin:

```sh
/plugin install cave-ai --marketplace cave-ai/agent-skills
```

On older Claude Code versions, add the marketplace first:

```sh
/plugin marketplace add cave-ai/agent-skills
/plugin install cave-ai@cave-ai
```

The plugin connects the Cave MCP server at `https://getcave.ai/mcp`; sign in with your Cave account when Claude prompts you (or run `/mcp`). If you already connected Cave separately, you can turn one of the two connections off.

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
