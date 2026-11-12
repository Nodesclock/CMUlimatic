# CMUlimatic Technical Report

> **Evidence notice:** This report summarizes source inspection, local reproductions, and reported tests in the production IDE. Original browser captures, complete test harnesses, full asset hashes, and the candidate patch diff are not included. The reported outcomes require primary record verification before public release.

## Summary

CMU CS Academy runs student Python in a JavaScript Web Worker using a modified Brython runtime. Its `$B.lockdown` setting limits selected imports and evaluation paths. The reported weakness is in native JavaScript property resolution. Permitted Python objects can expose JavaScript prototype properties even while lockdown is enabled.

That access can lead to the Web Worker global object and APIs outside the intended restricted Python interface. The issue crosses an **application level interpreter boundary**, not the browser or operating system sandbox. It is not remote code execution on CMU servers.

## Affected software

The reviewed source is [CMU's Brython fork](https://github.com/cmu-cs-academy/brython) at commit `e516a010adde1bfdbc3f1775610f93026d3f0568`. The reported production interpreter asset is `static/js/cmu-graphics.f1e1dd70.js` in the `/ide` code runner.

Two frontend assets, `main.13958e42.js` and `main.e2956417.js`, also appear in the research records. They are application frontend bundles, not the interpreter asset. A frontend filename change alone is not evidence that the interpreter weakness was fixed. Neither the first affected release nor the current deployed status has been established. See [Affected Versions](../research/affected-versions.md).

## Root cause

The source review found that lockdown checks operate at selected import and evaluation entry points while native backed object lookup can still resolve underlying JavaScript properties. A permitted Python module can therefore expose prototype linked JavaScript functionality and lead to the worker's `globalThis`.

Other reported paths involve `sys.modules`, dotted imports, `importlib`, and the interpreter's `__BRYTHON__` reference. An alternative compiled code route produced conflicting results between local test environments and remains unresolved. These paths are treated as related behavior rather than automatically as separate CVEs.

The source locations and mechanism are described in [Root Cause](../research/root-cause.md).

## Validation

Separate local test environments reportedly reproduced the primary module based escape against the pinned source revision with lockdown enabled. A controlled production IDE test reportedly reproduced worker global access. Additional checks examined ordinary Web Worker capabilities and limitations.

The results cover worker messaging, requests for public resources, script loading, synthetic IndexedDB access, WebSocket constructor access, and the absence of direct main page storage or DOM access. Original test artifacts have not yet been attached. [Validation Results](../research/validation-results.md) distinguishes observations from claims that were not tested.

## Security impact

The evidence supports an interpreter restriction bypass inside a browser Web Worker. It does not show access to protected backend records, teacher account compromise, authentication token theft, stored grade modification, or browser level privilege escalation.

The effect on trusted page handlers and grading authority is unresolved. Those questions matter more to severity than the availability of standard worker APIs. No final CVSS score is assigned. See [Impact Assessment](impact-assessment.md).

## Remediation considerations

1. Enforce lockdown restrictions when native JavaScript properties are resolved, not only when import or evaluation syntax is checked.
2. Restrict access to interpreter internals and preloaded browser bridge modules without breaking normal Python module behavior.
3. Treat every worker message as untrusted. Allow only expected message types and validate their contents before trusted page handlers act on them.
4. Evaluate stronger execution isolation, such as a dedicated origin or an appropriately sandboxed frame that creates its own worker. A `blob:` worker in the main page does not provide an independent origin.
5. Ensure recorded grades or progress affecting other users are authorized and verified by the server.

The proposed A–G interpreter changes were reported to pass local regression tests. The actual tested diff and regression output were not provided, so these remain **candidate mitigations**, not a verified complete fix. See [Patch Notes](../patches/README.md).

## Research boundaries

The evidence includes local harness experiments, reported production IDE checks, and separate earlier backend probes. These activities should remain distinct in any private vendor submission. No cross user compromise or privileged backend operation is claimed.
