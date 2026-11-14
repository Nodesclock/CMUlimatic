# CMUlimatic

**A Brython lockdown bypass in the CMU CS Academy Python runner.**

CMUlimatic is my independent research project examining an interpreter isolation weakness in [CMU CS Academy](https://academy.cs.cmu.edu). Its web IDE uses a modified [Brython](https://github.com/cmu-cs-academy/brython) runtime to execute student Python in a JavaScript Web Worker. My investigation found a path from permitted Python objects to JavaScript features that the `$B.lockdown` restrictions are intended to hide.

## Overview

| Field | Details |
|---|---|
| Affected component | [CMU CS Academy Brython fork](https://github.com/cmu-cs-academy/brython) |
| Observed environment | CS Academy `/ide` student code runner |
| Vulnerability | Application-level interpreter isolation bypass |
| CWE | CWE-693, provisional |
| CVE | Not assigned |
| Severity | Under assessment |
| Status | Research documented, impact still under review |

## Findings

I examined the source and compared local reproductions with reported tests in the production IDE. The results indicate that restricted Python can reach the Web Worker global object. Additional checks reported access to standard worker features, including public network requests, page output messaging, script loading, and synthetic IndexedDB operations.

**These results do not demonstrate** account takeover, access to other students' records, stolen authentication tokens, persistent grade changes, or server-side code execution. The browser's own security sandbox was not bypassed.

I've documented the underlying mechanism in [Root Cause](research/root-cause.md). [Validation Results](research/validation-results.md) separates reported observations from evidence that is still missing, while [Impact Assessment](advisory/impact-assessment.md) describes the limits of the findings.

## Repository

The [Technical Report](advisory/technical-report.md) brings together the analysis and potential fixes. [Affected Versions](research/affected-versions.md) identifies the examined revision and observed production assets. [Patch Notes](patches/README.md) explains why I have not included an unverified patch diff.

## Disclosure and credits

I'm keeping complete reproduction material out of this repository while preparing a coordinated disclosure with Carnegie Mellon University. This is independent research and is not affiliated with or endorsed by CMU.

**Researcher and repository author:** [Nodesclock](https://github.com/Nodesclock)

I used AI-assisted tools for parts of the analysis and documentation. Where the original logs or test artifacts are unavailable, I identify results as reported rather than independently verified.

## License

My original project material is available under the [MIT License](LICENSE). Third-party source code retains its own license.
