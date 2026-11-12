# Validation Results

> **Evidence notice:** This file summarizes research records and reported test outcomes. Primary worker captures, complete harness code, local JSON matrices, downloaded production assets, and the tested patch diff were not supplied with these drafts. They are needed for independent reproduction.

## Local reproduction

The pinned Brython fork was reportedly built in two separate local test environments with lockdown enabled. Both reproduced the permitted module route to the worker global and access through the preloaded module registry.

A separate compiled code route succeeded in one environment but failed in the other. That discrepancy is unresolved. A candidate patched runtime was reported to block known routes while retaining ordinary Python behavior, but neither the full patch nor the regression matrix is included here.

## Reported production IDE checks

| ID | Result | Limit |
|---|---|---|
| T1 | Worker global reached, `callable_fetch=True` | No authenticated request demonstrated |
| T2 | Synthetic text marker reached `printToTextArea` | Not a privileged handler |
| T3 | Public favicon returned HTTP `200` after a corrected retry | First invocation failed before a request was sent |
| T4 | Inert `data:` script executed through `importScripts` | Worker execution only |
| T5 | Synthetic IndexedDB record written, read, and removed | Cache API and existing data not tested |
| T6 | `WebSocket` constructor reachable, local connection refused | No successful external connection |
| T7 | No direct `localStorage` access, `document` was a stub | Does not rule out indirect page mediated operations |
| T8 | CORS enabled public S3 asset returned HTTP `200` | Standard permitted CORS behavior |

The production checks are described in the research records as bounded tests in the `/ide` runner. Without the original logs, they remain **reported observations**, not independently verified measurements.

## Unverified downstream impact

There is no reported live demonstration of teacher context execution, protected backend API access, cross user data exposure, persistent grade changes, or privileged page message handling.

Local mock application tests explored potential page message and grading behavior. A mock server accepting client scores does not prove that CMU's actual backend accepts them. The print popup HTML behavior was researched separately and should not be conflated with the Brython lockdown issue.

## Evidence still needed

1. Original local test harnesses and their outputs
2. Paired regression matrices for the original and proposed patched runtime
3. The exact tested patch diff
4. Full hashes and source copies for relevant production assets
5. Sanitized browser captures for T1 through T8, including both T3 attempts
6. Records of the authorization scope for production tests

No missing result, log, screenshot, hash, or patch is represented as verified in this report.
