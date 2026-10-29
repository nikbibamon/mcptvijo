# Tvijo MCP — public product, documentation and marketing hub

Public home of the **Tvijo DSHREM MCP server**: a Model Context Protocol server that lets
an assistant ask a question or review a pre-merge diff **with an explicit spending limit**,
and read or cancel its own tasks.

- npm: [`tvijo-dshrem-mcp`](https://www.npmjs.com/package/tvijo-dshrem-mcp)
- Open MCP Registry: [`com.tvijo/dshrem`](https://registry.modelcontextprotocol.io/v0.1/servers/com.tvijo%2Fdshrem/versions/latest)
- Product: <https://dsh.tvijo.com>
- Source of truth for the code: [`nikbibamon/tvijocode/mcp`](https://github.com/nikbibamon/tvijocode/tree/main/mcp)

This repository is the **public packaging, documentation and marketing layer**. It does not
hold a second copy of the server code: `tvijocode/mcp` is the single source and the npm
package is built from it. A copy here would be a second version that drifts — and two
places to patch a spending path.

## Status — observed 2026-10-29 (UTC)

| Channel | State | Evidence |
| --- | --- | --- |
| npm `tvijo-dshrem-mcp` | `0.2.0` published | `https://registry.npmjs.org/tvijo-dshrem-mcp/latest` |
| Open MCP Registry `com.tvijo/dshrem` | `0.2.0`, `active`, `isLatest: true` | `curl https://registry.modelcontextprotocol.io/v0.1/servers/com.tvijo%2Fdshrem/versions/latest` |
| github.com/mcp and the VS Code MCP gallery | **not listed** — `count: 0` | `curl "https://api.mcp.github.com/v0.1/servers?search=dshrem"` |
| Onboarding request | posted in [discussion #1257](https://github.com/github/github-mcp-server/discussions/1257#discussioncomment-19245495); acceptance **unconfirmed** | request id recorded in the Tvijo release journal |

Adding a server to `github.com/mcp` is a **manual curation step performed by GitHub**
(confirmed by a GitHub collaborator in that thread, May 2026). Publishing to the open
registry is a prerequisite, not a guarantee, and there is no published ETA or criteria.
We do not promise a listing date to anyone, including ourselves.

The gallery is discovery only. The server works today through manual configuration.

## Install

Node.js 20 or newer, and a [Tvijo account](https://dsh.tvijo.com).

```sh
npx -y tvijo-dshrem-mcp login     # device sign-in, token stored 0600
npx -y tvijo-dshrem-mcp --version
```

VS Code — `.vscode/mcp.json`:

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

Details: [docs/quickstart.md](docs/quickstart.md) · Cursor and terminal clients are covered there too.

## Tools

Seven tools are always available; three more appear only when local evidence snapshots are
switched on explicitly.

| Tool | Kind | Purpose |
| --- | --- | --- |
| `tvijo_ask` | paid, capped | Answer a prompt while spending at most an explicit `max_usd`. |
| `tvijo_review` | paid, capped | Review a supplied pre-merge diff. |
| `tvijo_account` | read-only | Account, balance and limits. |
| `tvijo_task_status` | read-only | Read a task by `task_id`. |
| `tvijo_cancel_task` | destructive | Cancel a task; money already spent may still be charged. |
| `tvijo_login_start` / `tvijo_login_poll` | auth | Device sign-in without exposing a token to the model. |
| `tvijo_local_repositories` | read-only, opt-in | Bounded snapshot of authorized repositories. |
| `tvijo_local_test_results` | read-only, opt-in | Bounded test-result summary snapshot. |
| `tvijo_local_execution_budget` | read-only, opt-in | Budget snapshot — **evidence only, never permission to spend**. |

Full schemas and failure behaviour: [docs/tools.md](docs/tools.md).

## Spending safety

- `tvijo_ask` and `tvijo_review` spend real money up to `max_usd`, which **must** be supplied per call.
- `TVIJO_MAX_USD_CAP` defaults to 1 USD per call, hard ceiling 5 USD; values above 5 are clamped.
- A request above the effective cap is rejected **before** any network call.
- Paid requests are never retried automatically. An ambiguous outcome is resolved by a
  read-only lookup, and `spent_usd: null` means "unknown", never "zero".
- The server never logs tokens and never returns one to the model.

## Repository map

```
docs/         quickstart, tool reference, registry status, partner programme
marketing/    ICP, offer ladder, outreach rules, AEO/GEO plan
learn/        teaching page: how MCP discovery, tool design and citation actually work
```

## For maintainers of other MCP servers

[docs/registry-status.md](docs/registry-status.md) records what the two registries do and do
not do, with dated evidence. [docs/partners.md](docs/partners.md) describes what we can and
cannot help with — including the part we cannot promise (a GitHub listing).

## License

MIT for this repository. The hosted Tvijo service is proprietary.
