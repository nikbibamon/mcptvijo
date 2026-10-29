# Market position and ICP

## What we sell

An agent that can be given a task **and a hard spend limit**, and that reports what it
actually spent. Not "an AI assistant". The differentiator is the accounting: explicit cap per
call, fail-closed rejection above the cap, no automatic retry of a paid request, and
`spent_usd: null` when the amount is unknown instead of a comfortable zero.

Second line: **evidence-first review** — a pre-merge diff review that returns findings and
its own cost in the same response.

## Who buys

### A. MCP server maintainers (supply side)

The people in
[github/github-mcp-server#1257](https://github.com/github/github-mcp-server/discussions/1257):
solo developers and small SaaS teams who shipped a server and are stuck in curation.

- **Pain:** invisible in the gallery; self-doubt about the entry; no feedback loop.
- **Buys:** audit (free), review of tool descriptions and safety (paid), a citable page (paid).
- **Does not buy:** anything priced like an agency retainer. Most are pre-revenue.
- **Conversion path:** free audit → one small paid review → page.

### B. Teams putting agents into CI (demand side)

Engineering teams that already run coding agents and need a second opinion on a diff without
an open-ended bill.

- **Pain:** "who approved that spend", "how much did this review cost", "why did it retry".
- **Buys:** capped review, per-task accounting, evidence snapshot of tests and budget.
- **Trigger:** a spend incident, or a security review that asks for per-call limits.

### C. Corporate buyers of private MCP capability

Organisations that want agent access to internal systems but cannot send credentials or PII
into an unaccounted loop. Handled through the separate private track, not from this repository.

## Positioning statement

> For teams that put an agent in a money path, Tvijo is the MCP server that refuses to spend
> more than the number you passed — and tells you what it spent, or says it does not know.

## What we deliberately do not claim

- Listed in the VS Code gallery — we are not, and we do not promise it to anyone.
- A specific number of users, installs or revenue — not measured publicly, so not printed.
- Model names and vendor rankings in marketing copy. Method visible, vendor names not.
- Guaranteed citations in assistants.

## Channels, in order of cost per qualified conversation

1. Direct, in the thread and in issues where the pain is already stated in public.
2. Our own public pages, written to be cited (see [aeo-plan.md](aeo-plan.md)).
3. The registry entry itself — the description is read by humans and agents.
4. Nothing else until the first three produce a measured number.

## Funnel

| Stage | Signal | Artifact |
| --- | --- | --- |
| Seen | page opened, registry entry read | `learn/`, entry description |
| Understood | copied a config block | quickstart |
| Trusted | ran the audit, saw PASS/FAIL/NOT_RUN | audit checklist |
| Paid | first capped call | review with `spent_usd` |
| Repeat | asks for the next tool group | second review |

Each stage needs one measurement before the next stage gets budget. A stage with no
instrument is marked **not measured**, never estimated.

## Honest constraints

- The supply side (segment A) is numerous but poor; it is a **distribution** channel more
  than a revenue channel. One satisfied maintainer mentions the cap in public — that is the
  return, not the invoice.
- The demand side (segment B) is where money is, but the trigger is rare and specific.
- Corporate work (segment C) has the longest cycle and the highest verification cost; it
  cannot be scaled by posting.
