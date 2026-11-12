# Root Cause

## Intended boundary

The CMU CS Academy Brython fork uses `$B.lockdown` to restrict JavaScript interfaces available to student Python. In the reported `/ide` execution path, Python runs in a browser Web Worker and should not reach the worker's underlying JavaScript global object through normal permitted Python operations.

## Native property access

The reviewed revision is [`e516a010adde1bfdbc3f1775610f93026d3f0568`](https://github.com/cmu-cs-academy/brython/commit/e516a010adde1bfdbc3f1775610f93026d3f0568).

The research identified native backed object lookup paths where a Python attribute resolves to a property on an underlying JavaScript object or its prototype chain. The interpreter's lockdown checks did not consistently apply to those lookups.

A permitted Python module therefore reportedly provides a route from ordinary attribute access to the JavaScript `Function` constructor and then to the worker global scope. The issue is that the interpreter restriction, not JavaScript's standard constructor behavior, is the security boundary being bypassed.

Relevant source locations include:

| Source | Mechanism to review |
|---|---|
| `www/src/py_builtin_functions.js` | `$B.$getattr`, native backed attributes, and builtins evaluation |
| `www/src/js_objects.js` | `JSObj.__getattribute__` and raw JavaScript property access |
| `www/src/py_import.js` | Import filters, package names, and module loading |
| `www/src/builtin_modules.js` | `sys.modules` and interpreter object exposure |

Source references should be checked against the pinned commit rather than assumed to have the same line numbers in other revisions or minified bundles.

## Related routes

The research also describes module registry access through `sys.modules`, dotted package imports, `importlib`, and the `__BRYTHON__` interpreter reference. These are additional routes to the same worker environment.

A proposed `compile()` code object route behaved differently across local reproductions. Its portability remains unresolved and should not be presented as universally confirmed.

## Mitigation limits

An attribute denylist can block known routes but may miss future interoperability paths. Restricting interpreter references and import behavior is useful defense in depth. Stronger isolation should remove privileged application resources from the untrusted execution environment and validate worker to page messages.

## Scope

This finding concerns an application level Python interpreter boundary. It does not establish Chromium sandbox escape, operating system privilege escalation, backend command execution, or account takeover.
