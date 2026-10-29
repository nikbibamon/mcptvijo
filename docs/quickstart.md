# Quickstart

Requirements: Node.js 20 or newer, and a Tvijo account at <https://dsh.tvijo.com>.

The package is ESM JavaScript with no build step; `npx` runs it directly.

## 1. Sign in

```sh
npx -y tvijo-dshrem-mcp login
```

Open the verification URL printed in the terminal and approve the shown user code. The
token is stored at `~/.config/tvijo/mcp-token.json` (directory `0700`, file `0600`, includes
its expiry and API base). To revoke:

```sh
npx -y tvijo-dshrem-mcp logout
```

Logout revokes remotely and always removes the local file, reporting a failure if remote
revocation did not succeed.

## 2. Connect a client

### VS Code

`.vscode/mcp.json` — VS Code asks for the token through a password input:

```json
{
  "inputs": [
    { "type": "promptString", "id": "tvijo-token", "description": "Tvijo access token", "password": true }
  ],
  "servers": {
    "tvijo": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "tvijo-dshrem-mcp"],
      "env": { "TVIJO_TOKEN": "${input:tvijo-token}", "TVIJO_MAX_USD_CAP": "1" }
    }
  }
}
```

Prefer device login? Drop `inputs` and `TVIJO_TOKEN` and run the login command under the
same operating-system user as the MCP server.

### Cursor

`~/.cursor/mcp.json`, then run the device login command:

```json
{
  "mcpServers": {
    "tvijo": {
      "command": "npx",
      "args": ["-y", "tvijo-dshrem-mcp"],
      "env": { "TVIJO_MAX_USD_CAP": "1" }
    }
  }
}
```

Restart or reconnect the client after changing environment variables.

### Terminal clients

```sh
YOUR_MCP_CLIENT mcp add tvijo -- npx -y tvijo-dshrem-mcp
```

With no arguments, `npx -y tvijo-dshrem-mcp` starts the stdio server.

## 3. Configuration

| Variable | Meaning |
| --- | --- |
| `TVIJO_TOKEN` | Optional access token; overrides device-login storage. |
| `TVIJO_MAX_USD_CAP` | Per-call maximum in USD; default `1`, hard ceiling `5`. Values above 5 are clamped; invalid or non-positive values reject the call. |
| `TVIJO_API_BASE` | Optional API base; default `https://dsh.tvijo.com/api/dshrem/v1`. HTTPS required except for localhost in tests. A saved token is used only with its original API base. |
| `TVIJO_LOCAL_REPORTS` | `1` enables three read-only local evidence snapshot tools. Off by default. |
| `TVIJO_LOCAL_REPORT_ROOT` | Absolute directory holding the snapshots. No symlinks, no traversal, no paths accepted from the model. |

## 4. First call

```json
{ "prompt": "Explain how to make this function easier to test.", "max_usd": 0.25 }
```

`tvijo_ask` returns `task_id`, `answer`, `spent_usd`, `state`, `recovered`.

## Troubleshooting

| Symptom | Cause |
| --- | --- |
| `rejected before any network request` | `max_usd` above the effective cap, or a missing/non-positive cap. |
| `spent_usd: null` | Outcome not known — **not** evidence that nothing was spent. Inspect the task before asking again. |
| Signed out after a while | The saved token expired; run login again. |
| Server not in the gallery | Expected — the GitHub registry is curated manually. See [registry-status.md](registry-status.md). |
