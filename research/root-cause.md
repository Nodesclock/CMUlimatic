# Root Cause

## Intended boundary:

I examined how CMU CS Academy's Brython fork uses `$B.lockdown` to restrict JavaScript interfaces exposed to student Python. In the reported `/ide` execution path, Python runs inside a browser Web Worker. Permitted Python operations should not provide an unintended route to the underlying JavaScript global object.

## Native property access:

I focused on source revision [`e516a010adde1bfdbc3f1775610f93026d3f0568`](https://github.com/cmu-cs-academy/brython/commit/e516a010adde1bfdbc3f1775610f93026d3f0568).

My source review identified native-backed attribute lookup paths where a Python attribute can resolve to a property on an underlying JavaScript object or its prototype chain. The reported lockdown checks do not consistently apply to those lookups.

As a result, a permitted Python module reportedly exposes a route from ordinary attribute access to the JavaScript `Function` constructor and then to the worker's global scope. The security issue is the interpreter's failure to enforce its intended boundary, not JavaScript's normal constructor behavior.

I examined these source locations:

| Source | Mechanism |
|---|---|
| `www/src/py_builtin_functions.js` | `$B.$getattr`, native-backed attributes, and builtins evaluation |
| `www/src/js_objects.js` | `JSObj.__getattribute__` and raw JavaScript property access |
| `www/src/py_import.js` | Import restrictions and module loading |
| `www/src/builtin_modules.js` | `sys.modules` and interpreter references |

These references apply to the pinned source revision. I have not assumed the same line numbers or implementation in other versions.

## Related routes:

The research records also identify paths involving `sys.modules`, dotted imports, `importlib`, and the `__BRYTHON__` interpreter reference. I treat these as related routes into the same worker environment.

A proposed `compile()` code-object route produced different results across local test environments. I am leaving its portability unresolved rather than claiming that it works in every build.

## Mitigation limits:

In my assessment, an attribute denylist may block the known routes but could miss future interoperability paths. Restricting exposed interpreter references and imports can help, but stronger isolation would keep privileged application resources outside the untrusted worker and validate worker-to-page messages.

## Scope:

I am reporting an **application-level Python interpreter boundary** issue. The evidence does not establish a Chromium sandbox escape, operating system privilege escalation, backend command execution, or account takeover.
