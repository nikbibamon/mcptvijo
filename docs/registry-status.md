# Two registries, one gate

Why a published MCP server is still invisible in the VS Code gallery — with the evidence,
not the folklore. All observations below are dated; re-run the commands before quoting them.

## The two registries

| | Open registry | GitHub registry |
| --- | --- | --- |
| URL | `registry.modelcontextprotocol.io` | `github.com/mcp` · API `api.mcp.github.com` |
| Publication | Self-service, automated | **Manual curation by GitHub** |
| Versions | Every published version | Syncs from the open registry **after** first onboarding |
| Criteria | Schema validity | Not published |
| ETA | Immediate | None given; measured in months in practice |

GitHub collaborator statement in
[discussion #1257](https://github.com/github/github-mcp-server/discussions/1257) (May 2026):

> the GitHub MCP Registry now has the ability to sync versions from the open source
> registry, but onboarding a new server is still a manual curation process today. Once a
> server has been onboarded, newly published versions from the open source registry should
> sync from there.

So the sequence is always: publish to the open registry → request onboarding → wait for a
human → afterwards versions flow by themselves. Asking again does not move the queue.

## Observed state, 2026-10-29 (UTC)

```sh
# open registry: present, active, latest
curl "https://registry.modelcontextprotocol.io/v0.1/servers/com.tvijo%2Fdshrem/versions/latest"
#   -> "version":"0.2.0", status active, isLatest true

# GitHub registry: absent
curl "https://api.mcp.github.com/v0.1/servers?search=dshrem"
#   -> {"servers":[],"metadata":{"count":0,...}}
```

This is the normal state for a server waiting in curation, not a defect in the server.

## What actually blocks a request

From the thread itself and from our own review of ~30 entries posted between 2026-10-28 and
2026-10-29:

1. **Missing `repository` field.** Two maintainers wrote in their own request that the entry
   does not carry it. A curator with no repository and no source has nothing to check.
2. **`active`/`isLatest` not true.** A superseded version at the latest pointer reads as an
   abandoned server.
3. **Closed source with no manifest.** "Hosted, source not published" is allowed, but then
   `server.json`, docs and a public status/health endpoint become the only evidence.
4. **Write and destructive tools without annotations.** Invoice, CRM and freight servers in
   that thread expose create/update/cancel tools. Without `readOnlyHint` /
   `destructiveHint` a client cannot prompt the user safely.
5. **Auth that cannot be reviewed.** OAuth 2.1 + PKCE with protected-resource metadata and
   dynamic client registration is the shape reviewers expect for a remote server; a bearer
   key pasted into config is not.
6. **No public endpoint evidence.** A `curl` that returns 401 with `WWW-Authenticate` is
   better evidence than a screenshot.

## What we tell people

- A listing has **no ETA and no published criteria**. Any vendor promising you one is
  guessing.
- Meanwhile a server is fully usable: users add it manually (`.vscode/mcp.json`,
  `mcp.json` in Cursor, `mcp add` in terminal clients).
- The cheapest real improvement is the entry itself: schema valid, `repository` set,
  `isLatest` true, annotations on every tool, bounded output, honest error codes.

## When this changes

If GitHub publishes curation criteria or an SLA, this page gets a new dated observation
section. Until then it is not updated with opinions — only with what the two APIs return.
