# CMUlimatic Technical Report

> **Evidence notice:** This report draws on source inspection, local reproduction records, and reported production IDE tests. I have not included the original browser captures, complete harnesses, full asset hashes, or the candidate patch diff. Those artifacts are still needed to independently verify the reported results.

## Summary:

In this report, I examine the `$B.lockdown` restrictions in the modified Brython runtime used by CMU CS Academy. Student Python executes in a JavaScript Web Worker. Lockdown limits selected imports and evaluation paths, but the source review identified native JavaScript property resolution that can expose prototype properties on otherwise permitted Python objects.

That access can reach the worker's global object and APIs outside the intended restricted Python interface. This is an **application-level interpreter isolation bypass**, not an escape from the browser or operating system sandbox. It does not establish code execution on CMU servers.

## Affected software:

I focused on [CMU's Brython fork](https://github.com/cmu-cs-academy/brython) at commit `e516a010adde1bfdbc3f1775610f93026d3f0568`. The reported production interpreter asset is `static/js/cmu-graphics.f1e1dd70.js` in the `/ide` code runner.

The research records also identify two application frontend bundles, `main.13958e42.js` and `main.e2956417.js`. Their names alone cannot establish whether the interpreter issue was fixed. I have not established the first affected release or the status of the current deployment. See [Affected Versions](../research/affected-versions.md).

## Root cause:

My review points to a gap between lockdown's import and evaluation checks and the lower-level attribute resolution logic. Native-backed objects can expose underlying JavaScript properties, allowing a permitted Python module to provide a path to the worker's `globalThis`.

Related paths reported in the research involve `sys.modules`, dotted imports, `importlib`, and the interpreter's `__BRYTHON__` reference. A proposed compiled-code route behaved differently across local environments, so I am leaving that route unresolved rather than treating it as confirmed everywhere.

I describe the source locations and mechanism in [Root Cause](../research/root-cause.md).

## Validation:

Two separate local environments reportedly reproduced the primary module-based escape against the examined Brython revision with lockdown enabled. A controlled production IDE test also reportedly reached the worker global object. I have not independently verified the original browser transcripts.

The recorded follow-up checks cover worker messaging, public resource requests, inert script loading, synthetic IndexedDB access, WebSocket constructor availability, and the absence of direct access to the main page's storage and DOM. See [Validation Results](../research/validation-results.md) for the full breakdown and the missing evidence.

## Security impact:

I consider the interpreter restriction bypass the core finding. The material available to me does not show access to protected backend records, teacher account compromise, stolen authentication tokens, persistent grade modification, or browser-level privilege escalation.

The behavior of privileged page message handlers and the backend's grading authority remains unverified. I have therefore not assigned a numerical CVSS score. See [Impact Assessment](impact-assessment.md).

## Remediation considerations:

Based on the reported mechanism, I recommend that CMU:

1. Apply lockdown restrictions during native JavaScript property resolution, not only during import and evaluation checks.
2. Restrict interpreter internals and preloaded browser bridge modules without breaking normal Python behavior.
3. Validate worker message types and payloads before they reach trusted page handlers.
4. Consider a dedicated security origin or an appropriately sandboxed frame that creates its own worker. A `blob:` worker created on the main page does not provide a separate origin.
5. Ensure stored grades and progress updates are independently authorized and verified on the server.

The research records describe candidate interpreter changes labeled A through G. They reportedly passed local regression tests, but I have not received the exact tested diff or regression output. I am treating them as **candidate mitigations**, not a verified production fix. See [Patch Notes](../patches/README.md).

## Research boundaries:

I've kept local harness experiments, reported production IDE checks, and earlier backend probes separate in this assessment. I do not claim cross-user compromise or privileged backend access.
