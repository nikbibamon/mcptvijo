# Security policy

## Reporting

Open a private security advisory on this repository, or write to the address published on
<https://dsh.tvijo.com>. Please do not file a public issue for a vulnerability.

Include: affected tool, input that reproduces it, expected vs actual behaviour, and whether
any money could be spent by the issue.

## Scope

In scope:

- the `tvijo-dshrem-mcp` package and the MCP server it starts;
- the hosted API it calls;
- token handling and the device-login flow;
- spending caps and the paths that decide whether a call is rejected before the network.

Out of scope:

- the content of answers produced by a model;
- third-party MCP clients;
- anything that requires a stolen token plus full account access to demonstrate.

## Design commitments

1. **Tokens never travel through the model.** No tool result contains a token; errors are
   scrubbed of secrets.
2. **Caps fail closed.** A missing, non-numeric or non-positive cap rejects the call before any
   network request. Values above the ceiling are clamped, not negotiated.
3. **No automatic retry of a paid call.** Ambiguous outcomes are resolved by a read-only
   lookup.
4. **Unknown is not zero.** `spent_usd: null` means unknown and is never rendered as `0`.
5. **No arbitrary filesystem or shell access.** Local evidence tools accept no path, read
   fixed filenames under an absolute root, refuse symlinks, cap size at 64 KiB and refuse
   snapshots older than 15 minutes.
6. **Destructive tools are annotated.** Clients that honour `destructiveHint` will prompt.

## What we will publish

A fix, a version bump and a short note in the release journal. If we cannot fix it, we say so
and disable the affected tool behind an environment flag rather than leaving it reachable.
