# copilot-utility

Support-ticket triage agent for GitHub Copilot CLI. Type `triage <TICKET-ID>` in a
Copilot CLI session launched from this folder — the agent pulls the ticket from JIRA,
searches for prior-art fixes, routes to the right repo, and proposes a fix.

## Setup

1. Install Copilot CLI (`npm i -g @github/copilot` or your org's distribution).
2. Wire the MCP servers in `mcp-config.json` (point Copilot CLI at this file, or use
   `/mcp add` inside a session — flag/command syntax varies by CLI version, check
   `copilot --help`).
3. Auth Atlassian + GitHub MCP via OAuth on first connect.
4. Fill in `ticket-routing.yaml` with your component → repo mappings as you learn them.
5. Confirm your GitHub token/app scopes cover every repo you want searched — unscoped
   repos silently drop out of search results.

## Usage

```
$ cd copilot-utility
$ copilot
> triage PROJ-1234
```

The agent reads `AGENTS.md` for the procedure and `ticket-routing.yaml` for repo
routing, then returns: similar past cases, suspected repo/location, root cause,
proposed fix, and a confidence rating.

## Files

- `AGENTS.md` — the triage procedure Copilot CLI follows.
- `ticket-routing.yaml` — component → repo routing map (fallback when prior-art
  search doesn't surface a repo).
- `mcp-config.json` — Atlassian (JIRA) + GitHub MCP server config.

## Notes

- Ticket contents flow into Copilot + Atlassian + GitHub MCP calls. Confirm your
  enterprise plan's retention policy before piping real customer tickets through this.
- Org-wide code search (step 4, unrouted) burns rate limits — keep
  `ticket-routing.yaml` filled in to avoid hitting it often.
