# copilot-utility

Support-ticket triage agent, available two ways:
- **GitHub Copilot CLI** — type `triage <TICKET-ID>` in a Copilot CLI session launched
  from this folder.
- **VS Code** — open this folder, pick **Bug Triage** from the Copilot Chat agent
  picker, type `triage <TICKET-ID>`.

Both pull the ticket from JIRA, search for prior-art fixes, route to the right repo,
and propose a fix.

## Setup — Copilot CLI

1. Install Copilot CLI (`npm i -g @github/copilot` or your org's distribution).
2. Wire the MCP servers in `mcp-config.json` (point Copilot CLI at this file, or use
   `/mcp add` inside a session — flag/command syntax varies by CLI version, check
   `copilot --help`).
3. Auth Atlassian + GitHub MCP via OAuth on first connect.
4. Fill in `ticket-routing.yaml` with your component → repo mappings as you learn them.
5. Confirm your GitHub token/app scopes cover every repo you want searched — unscoped
   repos silently drop out of search results.

## Setup — VS Code

1. Install/update the GitHub Copilot Chat extension (needs custom-agent support).
2. Open this folder in VS Code. `.vscode/mcp.json` is auto-detected — click **Start**
   on the Atlassian and GitHub servers when prompted, then sign in via OAuth.
3. Open Copilot Chat, select **Bug Triage** from the agent/mode picker (reads
   `.github/agents/bug-triage.agent.md`).
4. Fill in `ticket-routing.yaml` with your component → repo mappings as you learn them.

## Usage

```
$ cd copilot-utility
$ copilot
> triage PROJ-1234
```

or in VS Code Copilot Chat, with **Bug Triage** selected: `triage PROJ-1234`.

The agent reads its procedure (`AGENTS.md` for CLI, `bug-triage.agent.md` for VS Code)
and `ticket-routing.yaml` for repo routing, then returns: similar past cases, suspected
repo/location, root cause, proposed fix, and a confidence rating.

## Files

- `AGENTS.md` — the triage procedure GitHub Copilot CLI follows.
- `.github/agents/bug-triage.agent.md` — the same procedure as a VS Code custom agent.
- `ticket-routing.yaml` — component → repo routing map (fallback when prior-art
  search doesn't surface a repo).
- `mcp-config.json` — Atlassian (JIRA) + GitHub MCP server config for Copilot CLI.
- `.vscode/mcp.json` — same MCP servers, VS Code format (`servers` key, not `mcpServers`).

## Notes

- Ticket contents flow into Copilot + Atlassian + GitHub MCP calls. Confirm your
  enterprise plan's retention policy before piping real customer tickets through this.
- Org-wide code search (step 4, unrouted) burns rate limits — keep
  `ticket-routing.yaml` filled in to avoid hitting it often.
