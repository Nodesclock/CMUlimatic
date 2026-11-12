# CMUlimatic

**A Brython lockdown bypass in the CMU CS Academy Python runner.**

CMUlimatic documents an interpreter isolation weakness in [CMU CS Academy](https://academy.cs.cmu.edu). The web IDE uses a modified [Brython](https://github.com/cmu-cs-academy/brython) runtime to run student Python inside a JavaScript Web Worker. Research into its `$B.lockdown` restrictions found a route from permitted Python objects to JavaScript features that the restricted interface was intended to hide.

## Overview

| Field | Details |
|---|---|
| Affected component | [CMU CS Academy Brython fork](https://github.com/cmu-cs-academy/brython) |
| Observed environment | CS Academy `/ide` student code runner |
| Vulnerability | Application level interpreter isolation bypass |
| CWE | CWE-693, provisional |
| CVE | Not assigned |
| Severity | Under assessment |
| Status | Research documented, coordinated disclosure pending |

## Findings

Local reproductions and reported tests in the production IDE indicate that restricted Python can reach the Web Worker global object. Follow up checks observed standard worker features including public network requests, page output messaging, dynamic script loading, and synthetic IndexedDB operations.

The available results **do not demonstrate** account takeover, access to other students' records, stolen authentication tokens, persistent grade changes, or server side code execution. The browser's own security sandbox was not bypassed.

The underlying source analysis is documented in [Root Cause](research/root-cause.md). See [Validation Results](research/validation-results.md) for the test evidence and its limitations, and [Impact Assessment](advisory/impact-assessment.md) for what remains unverified.

## Repository

The [Technical Report](advisory/technical-report.md) describes the issue and potential fixes. [Affected Versions](research/affected-versions.md) separates the reviewed source from observed production assets. [Patch Notes](patches/README.md) explain why an unverified diff has not been included.

## Disclosure and credits

This is an independent research project. Technical reproduction material is being held for coordinated disclosure with Carnegie Mellon University. The project is not affiliated with or endorsed by CMU.

**Researcher and repository author:** [Nodesclock](https://github.com/Nodesclock)

AI assisted tools supported parts of the analysis and documentation. The technical claims are limited to what the available source material and reported test results support.

## License

Original project material is available under the [MIT License](LICENSE). Third party source code retains its own license.
