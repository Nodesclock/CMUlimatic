# Affected Versions and Environments

## Source examined

The research focused on [CMU's Brython fork](https://github.com/cmu-cs-academy/brython) at commit `e516a010adde1bfdbc3f1775610f93026d3f0568`. Local environments reportedly reproduced the module based escape against that revision with `$B.lockdown` enabled.

The evidence does **not** establish the first affected commit or a complete range of vulnerable releases.

## Reported production assets

| Asset | Role | Evidence status |
|---|---|---|
| `static/js/cmu-graphics.f1e1dd70.js` | Embedded interpreter and student code runtime | Reported present in the tested IDE. Full asset and hash not included |
| `main.13958e42.js` | Earlier observed application frontend | Source of reported page handler analysis |
| `main.e2956417.js` | Later observed application frontend | Not independently audited for all page handler changes |

The change between the two `main.*.js` names does not establish that the interpreter was patched. A reported production test reached the worker global in the `/ide` code runner, but the original browser capture is still needed for independent verification.

## Outside the verified scope

The research does not establish that ordinary `/course` navigation, the desktop graphics package, other CMU web properties, or every installation of upstream Brython is vulnerable.

The status of any deployment after the reported test has not been independently checked. Avoid applying these findings to versions or products that have not been examined.
