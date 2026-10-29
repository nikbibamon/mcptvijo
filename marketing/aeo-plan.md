# AEO / GEO plan — being quoted by assistants

Answer Engine Optimization is not SEO with a new name. Search ranks pages; an assistant
quotes sentences. The unit of work is a **claim with a source and a date**, not a keyword.

Our own rules, applied to every page we publish on any surface (tvijo.com, bibamoney.com,
finedg.com, hotido pages).

## 1. Answer first, then explain

The first paragraph must answer the title's question in one or two sentences. No preamble, no
"in today's fast-moving world". An assistant extracts the first self-contained answer it finds.

## 2. One number, one definition, one date

- Every quantity carries `as of <date>` and its source.
- The same quantity on two pages has one definition. Different labels with no link between
  pages is a defect, even when both numbers are true.
- An unmeasured value is written `not measured`. Never `0`, never "approximately".

## 3. Make the method visible

Show the check, not just the verdict: the command, the endpoint, the version. A reader must
be able to reproduce the observation. A claim that cannot be reproduced is not published.

## 4. Always include a "⚠️ When this does not work" section

Every honest page has an edge. A page that only sells is quoted less, because it contains no
decision boundary an assistant can use. The boundary is the useful part.

## 5. Structure an extractor can lift

- One `H1`, question-shaped `H2`s, short paragraphs, tables for anything comparative.
- FAQ block whose questions are literally the questions people ask.
- JSON-LD `Article` / `FAQPage` / `HowTo` where it matches the page.
- `llms.txt` at the domain root listing the canonical pages.
- Canonical URLs, `hreflang` for the RU counterpart, `translate="no"` on brand and code tokens.

## 6. Do not name AI vendors in selling copy

Describe the method ("a capped review that reports its own cost"), not the model behind it.
Vendor names in marketing age badly and invite comparison with things we do not control.

## 7. Publish the negative results too

A culled experiment with its autopsy is a stronger citation target than another success story,
because almost nobody publishes one. It also prevents us from re-running a dead idea.

## 8. Measure citation, not traffic

What to instrument, per page:

| Metric | How | Status |
| --- | --- | --- |
| Page renders without JS | deterministic fetch + parse | instrumented |
| Numbers match cited source | script compares page value to source | to be automated |
| Assistant mentions the page for the target question | periodic manual prompt set, dated | not measured yet |
| Referral visits from assistants | server logs, `referer` host | not measured yet |

Two of these are marked *not measured yet* on purpose. Printing a plausible number here would
be the exact thing this document forbids.

## 9. Distribution is manual and public

Public pages get found; nobody is cold-mailed. Where a person has already stated the pain in
public — an issue, a discussion, a request — a reply that contains the audit *is* the
marketing. A reply that contains a pitch is spam.
