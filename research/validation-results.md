# Validation Results

> **Evidence notice:** I compiled this summary from research records and reported test outcomes. The original worker captures, complete harness code, local JSON matrices, downloaded production assets, and tested patch diff are not included here. Without them, the outcomes below are not independently reproducible from this repository alone.

## Local reproduction:

The research involved two separate local environments running the pinned Brython fork with lockdown enabled. Both reportedly reproduced the permitted-module path to the worker global and access through the preloaded module registry.

Another route using a compiled code object succeeded in one environment and failed in the other. I am leaving that difference unresolved. A proposed patched runtime also reportedly blocked known routes while preserving ordinary Python operations, but I have not included the full patch or regression matrix.

## Reported production IDE checks:

I reviewed the T1 through T8 test summaries and kept their observed outcomes separate from claims they do not support.

| ID | Reported result | Limit |
|---|---|---|
| T1 | Worker global reached, `callable_fetch=True` | No authenticated request demonstrated |
| T2 | Synthetic text marker reached `printToTextArea` | Not a privileged page handler |
| T3 | Public favicon returned HTTP `200` after a corrected retry | First invocation failed before any request |
| T4 | Inert `data:` script ran through `importScripts` | Execution remained in the worker |
| T5 | Synthetic IndexedDB record written, read, and removed | Cache API and existing data not tested |
| T6 | `WebSocket` constructor reachable, local connection refused | No successful external connection |
| T7 | No direct `localStorage` access, `document` was a stub | Indirect page actions unverified |
| T8 | Public CORS-enabled S3 asset returned HTTP `200` | Ordinary permitted CORS behavior |

The records describe bounded tests in the `/ide` runner. I have not independently checked the original logs, so I am treating these as **reported observations**.

## Unverified downstream impact:

I found no live demonstration in the supplied records of execution in a teacher's session, protected backend access, cross-user data exposure, stored grade changes, or privileged page-message handling.

Local mock-application tests explored page messaging and potential grading behavior. A mock backend accepting client scores does not establish what CMU's production backend accepts. A separate print-popup HTML issue was examined during the broader research, but I am not presenting it as part of this Brython lockdown finding.

## Evidence still needed:

1. Original local harnesses and outputs
2. Paired regression matrices for the unmodified and proposed patched runtime
3. The exact tested patch diff
4. Complete hashes and copies of relevant production assets
5. Sanitized browser captures for T1 through T8, including both T3 attempts
6. Records defining the permitted scope of production testing

I have not represented any missing log, screenshot, hash, or patch as verified evidence.
