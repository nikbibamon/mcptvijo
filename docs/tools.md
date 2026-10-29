# Tool reference

Ten tools in total: seven always available, three only with `TVIJO_LOCAL_REPORTS=1`.

Every tool answers with JSON text plus structured content where the value is tabular.
Nothing here accepts a filesystem path, a shell command or a free-form URL from the model.

## Always available

| Tool | Cost | Annotations | Input | Output |
| --- | --- | --- | --- | --- |
| `tvijo_ask` | **paid, capped** | — | `prompt` (1–20,000 chars, required), `max_usd` (positive, required), optional `title` (≤120), `model` (default `coding`), `mode` (default `single`), `max_tokens` (default `2048`) | `task_id`, `answer`, `spent_usd`, `state`, `recovered` |
| `tvijo_review` | **paid, capped** | — | supplied pre-merge diff plus the same cap fields | review result, `task_id`, `spent_usd` |
| `tvijo_account` | free | read-only | — | account, balance, limits |
| `tvijo_task_status` | free | read-only | `task_id` | task state and result |
| `tvijo_cancel_task` | free | destructive | `task_id` | cancellation result |
| `tvijo_login_start` | free | — | — | verification URL, user code, interval |
| `tvijo_login_poll` | free | — | optional `device_code` | `pending` / `signed in as …` / `denied` |

`tvijo_cancel_task` stops work; money already spent may still be charged. It is annotated
destructive so annotation-aware clients prompt before calling it.

## Opt-in local evidence (`TVIJO_LOCAL_REPORTS=1`)

Read-only, bounded to 64 KiB, refused when older than 15 minutes, strict schemas, no
symlinks, descriptor-relative traversal on Linux. They exist so an agent can quote a
snapshot instead of guessing.

| Tool | Returns |
| --- | --- |
| `tvijo_local_repositories` | `as_of`, repository ids, heads, `worktree: clean/dirty/unknown` |
| `tvijo_local_test_results` | `as_of`, per-suite `PASS/FAIL/NOT_RUN` with passed/failed/skipped/not_run totals |
| `tvijo_local_execution_budget` | `as_of`, `accounting: known/unknown/frozen`, USD limits and residues |

`tvijo_local_execution_budget` is **evidence only**. It never grants permission to spend and
never raises a cap. `accounting: unknown` carries `null` values by design — an unknown
amount is never rendered as zero.

## Spending semantics

1. `max_usd` must be supplied per call; there is no ambient default that spends without it.
2. Effective per-call cap = `min(max_usd, TVIJO_MAX_USD_CAP)`, ceiling 5 USD. Above the cap →
   rejected before the network request.
3. Task creation uses a fresh idempotency key. A paid request is never retried automatically.
4. Ambiguous outcome → one read-only lookup by idempotency key / task status. Recovered
   answers may carry `answer: null` or `spent_usd: null`; both mean *unknown*.
5. Several calls in one session can each spend up to the cap. The cap is per call, not a
   session budget. Account limits still apply.

## Failure codes you will actually see

| Code | Meaning |
| --- | --- |
| `authorization_pending` | Device login not approved yet; poll no faster than the returned interval. |
| `authorization_denied` | The person rejected the device code. |
| `request_expired` | Device authorization or the pending login expired; start again. |
| `invalid_local_snapshot` / `invalid_local_report_root` | Snapshot missing, stale, malformed, or the root is not an absolute non-symlink directory. |

## What the server will not do

- Return a token to the model, in any tool result.
- Log a token or a secret-bearing error payload.
- Read arbitrary files, run commands, or accept a path argument.
- Retry a paid call on its own.
