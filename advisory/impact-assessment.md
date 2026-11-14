# Impact Assessment

**Status:** Provisional. I assess CMUlimatic as a reported interpreter isolation bypass. Its effect on privileged application operations has not been established.

## Observed capabilities:

The research records describe production checks T1 through T8. They indicate that restricted student Python could reach the Web Worker global scope and use several standard worker features.

| Area | Reported result | Important limit |
|---|---|---|
| Worker global | `global_reachable callable_fetch=True` | No authenticated backend access shown |
| Page messaging | Synthetic output reached `printToTextArea` | No privileged page action shown |
| Public requests | Public static resources returned HTTP `200` | No protected data requested |
| Script loading | An inert `data:` script ran with `importScripts` | Execution remained in the worker |
| IndexedDB | Synthetic record created, read, and deleted | No existing user data accessed |
| WebSocket | Constructor was available | No remote connection established |
| Storage and DOM | Page `localStorage` absent, `document` was a stub | Indirect page actions unverified |
| Cross-origin requests | Public CORS-enabled S3 asset returned `200` | No CORS bypass shown |

I treat these as capabilities reachable after the interpreter restriction was bypassed, not as eight separate vulnerabilities. Several are ordinary Web Worker features.

## What I have not demonstrated:

The evidence does not show teacher or student account takeover, unauthorized access to records, theft of the page's authentication token, persistent grade changes, backend code execution, or an escape from the browser or OS sandbox.

The frontend reportedly gets its API token from the main page's `localStorage`. The worker could not read that storage directly. This limits the normal authenticated API route, but it does not prove every possible page-mediated action is inaccessible.

The client also appears to send submitted code to a backend autograding endpoint. That is consistent with server-side grading, although I cannot establish from the client code alone that all stored results are independently computed by the server.

## Severity:

**CVSS:** Not assigned. I do not think a score based on an assumed teacher takeover or grade forgery scenario would be justified by the current evidence.

## Questions for CMU:

1. Can a trusted page handler perform privileged actions based on messages from a student code worker?
2. Does any review workflow execute student code in another user's browser context?
3. Are stored grades and progress independently checked by the server?
4. Do any endpoints use ambient authentication accessible through worker network requests?

These are questions for vendor review, not confirmed security impacts.
