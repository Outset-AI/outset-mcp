# Outset MCP

Connect AI agents to [Outset](https://outset.ai) — the AI-moderated research platform — via the [Model Context Protocol](https://modelcontextprotocol.io).

The Outset MCP server is **remote**: there is nothing to install or run. Point your MCP client at the endpoint below and log in with your Outset account.

```
https://api.outset.ai/mcp/
```

- **Transport:** Streamable HTTP (stateless JSON-RPC)
- **Auth:** OAuth 2.1 with PKCE + Dynamic Client Registration — compliant clients connect with zero pre-registration
- **Full developer guide:** [Outset MCP — Integration Guide for Developers](https://outset-ai.notion.site/outset-mcp-integration-guide-for-developers) (OAuth flow, scopes, DCR details, testing)

## Connect from your client

### Claude (claude.ai / Desktop)

**Settings → Connectors → Add custom connector**, name it *Outset*, paste `https://api.outset.ai/mcp/`, then **Connect** to go through the OAuth consent flow. (Custom connectors require a Pro, Max, Team, or Enterprise plan.)

### Claude Code

```bash
claude mcp add --transport http outset https://api.outset.ai/mcp/
```

Or install as a plugin from this repo:

```bash
claude plugin marketplace add Outset-AI/outset-mcp
claude plugin install outset@outset-mcp
```

### Cursor

[![Add to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/install-mcp?name=outset&config=eyJ1cmwiOiJodHRwczovL2FwaS5vdXRzZXQuYWkvbWNwLyJ9)

Or **Settings → MCP → Add Server** and paste the URL — Cursor handles registration and OAuth automatically.

### ChatGPT

Enable **Developer Mode** under **Settings → Apps & Connectors → Advanced settings**, then **Create** a connector with `https://api.outset.ai/mcp/` as the MCP server URL and OAuth as the auth method.

### VS Code

```bash
code --add-mcp '{"name":"outset","type":"http","url":"https://api.outset.ai/mcp/"}'
```

### Gemini CLI

```bash
gemini extensions install https://github.com/Outset-AI/outset-mcp
```

### Codex

Add to `~/.codex/config.toml` (or `.codex/config.toml` in your project):

```toml
[mcp_servers.outset]
url = "https://api.outset.ai/mcp/"
```

Then authenticate with:

```bash
codex mcp login outset
```

### Anything else

Any MCP client that supports remote servers (Streamable HTTP + OAuth) works. For stdio-only clients, bridge with [`mcp-remote`](https://github.com/geelen/mcp-remote):

```bash
npx -y mcp-remote https://api.outset.ai/mcp/
```

To explore the tool list interactively:

```bash
npx @modelcontextprotocol/inspector https://api.outset.ai/mcp/
```

## What your agent can do

- **Create and edit studies** — projects, sections, questions, screeners, display logic, quotas, concepts
- **Configure and launch recruitment** — sample size, reward, demographic filters (launching may incur fees and is gated behind the separate `studies:launch` scope)
- **Query results** — reports, answers, topline insights, category summaries, highlight clips
- **Look up org context** — workspaces, projects, organization info

The live `tools/list` response is the source of truth — new tools ship regularly. Scopes and org roles are enforced server-side; see the [integration guide](https://outset-ai.notion.site/outset-mcp-integration-guide-for-developers) for the full scope table.

## What's in this repo

Manifests for agent directories and marketplaces — the server itself is not here (it's remote):

| File | Purpose |
|---|---|
| `.claude-plugin/plugin.json` + `.mcp.json` | Claude Code plugin wrapping the remote server |
| `.claude-plugin/marketplace.json` | Makes this repo installable as a Claude Code plugin marketplace |
| `.cursor-plugin/marketplace.json` | Same, for Cursor's plugin system |
| `gemini-extension.json` | Gemini CLI extension (`gemini extensions install <repo url>`) |
| `server.json` | Manifest for the [official MCP registry](https://registry.modelcontextprotocol.io) |
