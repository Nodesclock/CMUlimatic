# Affected Versions and Environments

## Source examined

I examined [CMU's Brython fork](https://github.com/cmu-cs-academy/brython) at commit `e516a010adde1bfdbc3f1775610f93026d3f0568`. Separate local test environments reportedly reproduced the primary module-based escape against that revision with `$B.lockdown` enabled.

I have not established the first affected commit or a complete range of vulnerable releases.

## Reported production assets

I separated the interpreter asset from the frontend assets referenced in the research records:

| Asset | Role | Evidence status |
|---|---|---|
| `static/js/cmu-graphics.f1e1dd70.js` | Embedded interpreter and student code runtime | Reported present in the tested IDE. Full asset and hash not attached |
| `main.13958e42.js` | Earlier observed application frontend | Used in reported page-handler analysis |
| `main.e2956417.js` | Later observed application frontend | Not independently audited for all page-handler changes |

A change between the two `main.*.js` filenames does not by itself mean the interpreter was patched. The test records report worker-global access in the `/ide` code runner, but I do not have the original browser capture attached for independent verification.

## Outside the verified scope

I have not established that ordinary `/course` navigation, the standalone desktop graphics package, other CMU properties, or every upstream Brython installation is affected.

I also have not verified the state of deployments after the reported tests. This report should not be read as a claim about versions or products I have not examined.
