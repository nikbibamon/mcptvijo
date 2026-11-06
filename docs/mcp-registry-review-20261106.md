# GitHub MCP Registry review — `com.tvijo/dshrem` (2026-11-06)

Status: GitHub reviewed the onboarding request on 2026-11-06 (ticket `170697`) and asked for
three changes. All three are made in the source repository, `nikbibamon/tvijocode`. The
reply that closes the review is at the bottom of this page.

## What GitHub asked

> 1. The root README describes the Tvijo Code editor build and mentions the MCP server in
>    one line. Please either add a prominent section there covering the server's
>    capabilities, setup and configuration, or move the server to its own dedicated
>    repository.
> 2. Please confirm the organisation or company that owns `com.tvijo`, and provide an
>    official support contact.
> 3. Please state near the top of the README that a paid Tvijo account is required.

## What changed

PR [#13](https://github.com/nikbibamon/tvijocode/pull/13) in `nikbibamon/tvijocode`, merged
to `main` (`807ee4b`). Only `README.md` and `mcp/README.md` changed; the server stays in
`mcp/` as the single source of the npm package, so the "dedicated repository" branch of
request 1 was not needed.

| Request | Where it is addressed |
| --- | --- |
| 1. Prominent server section | Root `README.md`: new "Tvijo DSHREM MCP server" section before "Download" — capabilities table (paid, read-only, free wiki/catalog reads, auth, opt-in local evidence), setup (`npx` login, stdio server, VS Code `mcp.json`, Claude Code), configuration table (`TVIJO_TOKEN`, `TVIJO_MAX_USD_CAP`, `TVIJO_API_BASE`), spending safety. |
| 2. Owner and support contact | "Ownership and support" in both READMEs: `com.tvijo` is the DNS-verified registry namespace of `tvijo.com`, owned and operated by Tvijo (Nik Bibamon, [@nikbibamon](https://github.com/nikbibamon)); support via repository issues, <https://t.me/bibamon> (the channel published on tvijo.com) and <https://dsh.tvijo.com/feedback>. |
| 3. Paid account near the top | Block quote directly under the title of the root README, and a bold paragraph at the top of `mcp/README.md`: paid tools `tvijo_ask` and `tvijo_review` spend from a funded Tvijo account; read-only tools spend nothing. |

## Verification (2026-11-06, UTC)

```sh
curl -s https://raw.githubusercontent.com/nikbibamon/tvijocode/main/README.md | head -20
curl -s https://raw.githubusercontent.com/nikbibamon/tvijocode/main/mcp/README.md | head -16
cd mcp && npm ci && npm test     # 113 passed, 0 failed
```

Observed results: both `main` READMEs carry the paid-account notice, the root README carries
the server section, the package tests pass (113/113), and the added links return 200 or 303.

## Reply to paste above the ticket line

```text
Hi GitHub team,

Thanks for the review. All three points are addressed in nikbibamon/tvijocode
(main, merge commit 807ee4b; PR #13).

1. Root README. It now has a dedicated "Tvijo DSHREM MCP server" section before
   "Download", covering capabilities (paid tools, read-only account/task tools,
   free public wiki and catalog reads, device sign-in, opt-in local evidence),
   setup (device login, stdio server, VS Code mcp.json, Claude Code, with
   Codex and Cursor in mcp/README.md), configuration (TVIJO_TOKEN,
   TVIJO_MAX_USD_CAP, TVIJO_API_BASE) and the spending-safety guarantees. The
   server stays in mcp/ in that repository because it is the single source of
   the published npm package; a second repository would be a copy that drifts.

2. Ownership and support. com.tvijo is the DNS-verified MCP Registry namespace
   of the tvijo.com domain. The domain, the hosted Tvijo service and the
   source repository are owned and operated by Tvijo, the product brand of
   Nik Bibamon (https://github.com/nikbibamon), who is the accountable
   maintainer. Official support contact: GitHub issues on
   https://github.com/nikbibamon/tvijocode/issues, the public support channel
   https://t.me/bibamon listed on tvijo.com, and the feedback form
   https://dsh.tvijo.com/feedback. This is now stated in an "Ownership and
   support" section of both README files.

3. Paid account. Both READMEs now state near the top that a paid (funded)
   Tvijo account is required to use the paid tools tvijo_ask and tvijo_review,
   which spend real money up to the explicit per-call max_usd (default cap
   1 USD, hard ceiling 5 USD, checked before any network call). Read-only
   tools and the public wiki/catalog reads need a signed-in account and spend
   nothing.

Checks: cd mcp && npm ci && npm test passes 113/113, and the links added to
the READMEs resolve.

Please let us know if anything else is needed for onboarding.

Thanks,
Nik
Tvijo, https://tvijo.com
```
