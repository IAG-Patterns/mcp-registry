# MCP Registry (GitHub Pages)

A static MCP (Model Context Protocol) registry hosted on GitHub Pages, compatible with the [v0.1 MCP Registry specification](https://modelcontextprotocol.io/registry/registry-aggregators).

## Registered Servers

| Server | Type | Transport | Source |
|--------|------|-----------|--------|
| **Playwright MCP** | Local (npm) | stdio | `@playwright/mcp` via npx |
| **Figma MCP Server** | Remote | streamable-http | `https://mcp.figma.com/mcp` |
| **MCP Atlassian** | Local (pypi) | stdio | `mcp-atlassian` via uvx |
| **Memory Server** | Local (npm) | stdio | `@modelcontextprotocol/server-memory` via npx |
| **Sequential Thinking** | Local (npm) | stdio | `@modelcontextprotocol/server-sequential-thinking` via npx |
| **GitHub MCP Server** | Remote | streamable-http | `https://api.githubcopilot.com/mcp/` |

## How It Works

This repository serves a static MCP registry by mapping the v0.1 REST API endpoints to a file/directory structure that GitHub Pages can serve:

```
GET /v0.1/servers                                          → v0.1/servers/index.html
GET /v0.1/servers/{namespace}/{name}/versions/latest       → v0.1/servers/{namespace}/{name}/versions/latest
GET /v0.1/servers/{namespace}/{name}/versions/{version}    → v0.1/servers/{namespace}/{name}/versions/{version}
```

Version files are extensionless JSON files. The `index.html` in `v0.1/servers/` contains the full server list as JSON (served when requesting `/v0.1/servers` via directory index redirect).

## Setup

1. Push this repo to GitHub
2. Go to **Settings > Pages**
3. Set **Source** to "Deploy from a branch"
4. Select your branch (e.g. `main`) and root `/`
5. Save — your registry will be live at `https://<username>.github.io/<repo-name>`

## Using the Registry

Paste your GitHub Pages URL into the **MCP registry URL** field in your GitHub Copilot organization/enterprise settings:

```
https://<username>.github.io/<repo-name>
```

## Known Limitations

- **Content-Type**: The `/v0.1/servers` list endpoint is served via `index.html`, so its Content-Type is `text/html` rather than `application/json`. Most HTTP clients parse the body correctly regardless.
- **CORS**: GitHub Pages sets `Access-Control-Allow-Origin: *` by default, which works for most IDE clients. If you need custom CORS headers, place a Cloudflare Worker or similar proxy in front.

## Adding a New Server

1. Create the directory structure: `v0.1/servers/{namespace}/{server-name}/versions/`
2. Add a `latest` file (extensionless) containing the server JSON
3. Add a version-specific file (e.g., `1.0.0`) with the same content
4. Update `v0.1/servers/index.html` to include the new server in the list
5. Commit and push
