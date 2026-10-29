# Offer ladder

Three offers, in the order a stranger can accept them. Each one has a written acceptance
check — a deliverable either passes it or is reported as failed.

## 1. Registry readiness audit — free

**You get:** a checklist over your registry entry, tool annotations, auth shape and failure
behaviour. Every line is `PASS` / `FAIL` / `NOT_RUN` plus the command or URL that produced it.

**We get:** the right to say "we audited this entry" and, if you agree, a one-line quote.

**Acceptance check:** the report names at least one real defect, or explicitly states that
none was found and which items could not be verified. A report that says everything is fine
without evidence is a failed deliverable.

**Why free:** the gate is not knowledge, it is attention. This is the cheapest honest way to
be useful to people who are stuck.

## 2. Evidence-first review of one tool group — paid, fixed price

**Scope:** one group of related tools (for example: all write tools, or the auth tools).

**You get:**
- description quality: could an agent pick the right tool from the descriptions alone;
- schema strictness and what the model can sneak past it;
- error semantics, including what "unknown" looks like;
- destructive-action annotations and confirmation flow;
- spending behaviour, if the tool spends money;
- the review's own cost, `spent_usd`, next to the findings.

**Acceptance check:** findings cite tool name and input; unverifiable items are `NOT_RUN`;
the cost is reported or explicitly `unknown`. A review without its own cost is rejected.

**Caps:** every call carries `max_usd`; the hard ceiling is 5 USD per call. This is published
so no one has to trust a promise about the bill.

## 3. Citable technical note — paid, editorial gate

**You get:** a public page about your server built to be quoted by assistants.

**Structure:** answer in the first paragraph; one claim per paragraph; source and `as of`
date next to every number; a "⚠️ when this does not work" section; FAQ; JSON-LD; `llms.txt`.

**Acceptance check:** our deterministic surface check passes on the published page (the page
renders, the numbers match the cited source, the FAQ questions appear as headings). If it
fails, it is not published and the fee is not taken for a failed check.

**What it is not:** a promise of citation, a promise of ranking, or a paid placement label
hidden in the text.

## What is never for sale

- A `github.com/mcp` listing or any claim about GitHub's curation.
- Deletion or softening of a failing finding in an audit.
- A review of code we did not run.
- Publication of a number without a source and a date.

## How a stranger moves through the ladder

```
saw a page  →  ran the free audit  →  asked for one paid review  →  asked for a note
   (learn/)      (issue in repo)        (fixed scope, capped)        (editorial gate)
```

There is no step that requires a call, a calendar link or a "discovery session". If a person
wants one, they ask; we do not manufacture urgency.

## Pricing

Not published as a list. One fixed price per deliverable, quoted after the free audit, in
writing, with the acceptance check attached. Price changes are recorded in the private
commercial notes, not negotiated per customer.
