# For MCP server maintainers

You published a server, it is `active` and `isLatest` in the open registry, and
`api.mcp.github.com` returns zero results. You are not broken. You are in a manual curation
queue with no ETA and no published criteria — see [registry-status.md](registry-status.md).

This page states plainly what we can help with, what we cannot, and what it costs.

## What we cannot do

- **We cannot list you in `github.com/mcp` or the VS Code gallery.** That decision belongs
  to GitHub. Anyone who promises it is guessing. We will not take money for it.
- We cannot guarantee citations in ChatGPT, Perplexity, Claude or Gemini. We can make a page
  that is *citable* — structure, dated evidence, one definition per quantity — and that is
  all anyone can honestly sell.
- We will not publish invented numbers, invented adoption, or a review that did not run.

## What we do

### 1. Registry readiness audit — free, deterministic

We read your entry and check the things that actually stop a curator:

- `server.json` valid against the current schema, `name` matches the namespace you own;
- `repository` present and reachable, or a manifest that makes up for its absence;
- `version` / `isLatest` coherent with npm, PyPI or the remote endpoint;
- every tool carries `title`, `readOnlyHint`, `destructiveHint`, `openWorldHint`;
- remote servers: OAuth 2.1 + PKCE, protected-resource metadata, a documented 401 shape;
- published per-call or per-session limits, and what happens when the call fails.

Output: a checklist with PASS / FAIL / NOT_RUN per item and the exact command that produced
it. No score without evidence.

### 2. Evidence-first MCP review — paid, capped

A bounded review of one tool group: description quality (can an agent choose this tool
correctly?), schema strictness, error semantics, destructive-action handling, spending
behaviour. The review runs through Tvijo with a per-call `max_usd` you set, and it returns
its own cost — `spent_usd` — next to the findings. If the outcome is uncertain, it says
`unknown` instead of zero.

### 3. Citable technical note — paid, editorial gate

A public page about your server written to be quoted by assistants: one claim per paragraph,
source and `as of` date next to every number, a "when this does not work" section, FAQ,
JSON-LD, `llms.txt` entry. Published on our surfaces only after our own deterministic check
passes. If the page cannot be verified, it is not published — the fee is not a publication
guarantee.

## How to start

Open an issue in this repository with:

```
Server name:
Registry name:
Latest version + isLatest:
Transport (npm/PyPI/cargo stdio, or remote URL):
Auth:
Repository (or "none — hosted"):
The one sentence you want an assistant to say about you:
```

We answer with the audit, including the items we could not verify and why.

## Terms in one line

Free audit, no strings, no listing promise. Paid work only when a written scope names the
deliverable, the cap in USD, and the acceptance check. Anything we cannot verify is reported
as NOT_RUN rather than smoothed over.
