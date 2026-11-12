# Impact Assessment

**Status:** Provisional. The evidence supports a reported interpreter isolation bypass. Its effect on privileged application operations remains unverified.

## Observed capabilities

The reported production checks T1 through T8 found that restricted student Python could reach the Web Worker global scope and use standard worker features.

| Area | Reported result | Important limit |
|---|---|---|
| Worker global | `global_reachable callable_fetch=True` | No authenticated backend access shown |
| Page messaging | Synthetic output reached `printToTextArea` | No privileged page action shown |
| Public requests | Public static resources returned HTTP `200` | No protected data was requested |
| Script loading | An inert `data:` script ran with `importScripts` | Execution remained in the worker |
| IndexedDB | Synthetic record created, read, and deleted | No existing user data was accessed |
| WebSocket | Constructor was available | No connection to a remote service succeeded |
| Storage and DOM | Page `localStorage` absent, `document` was a stub | Indirect page actions remain unverified |
| Cross origin requests | Public CORS enabled S3 asset returned `200` | No CORS bypass was shown |

These demonstrate capabilities reachable after the interpreter restriction was bypassed. Several are ordinary Web Worker features rather than separate vulnerabilities.

## Not demonstrated

No test supplied here shows teacher or student account takeover, unauthorized access to records, theft of the page's authentication token, persistent grade changes, backend code execution, or a browser or OS sandbox escape.

The frontend reportedly attaches its API token from the main page's `localStorage`. The escaped worker could not read that storage directly. This limits use of the normal authenticated API flow but does not conclusively rule out every indirect page mediated action.

The client also appears to send submitted code to a backend autograding endpoint. That is consistent with server controlled grading but does not by itself prove every stored result is independently calculated on the server.

## Severity

**CVSS: Not assigned.** A score should reflect demonstrated effects on confidentiality, integrity, or availability rather than an assumed teacher takeover or grade forgery scenario.

## Questions for CMU

1. Can a trusted page handler perform privileged actions based on messages emitted by a student code worker?
2. In which workflows, if any, does student supplied code execute in another user's browser context?
3. Are stored grades and progress updates independently checked by the server?
4. Do any application endpoints use ambient authentication accessible to worker network requests?

These are vendor review questions, not confirmed weaknesses.
