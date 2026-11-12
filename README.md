# CMUlimatic:

**A Brython Lockdown Sandbox Escape in CMU CS Academy**

CMUlimatic is an application-level sandbox escape discovered in the Brython-based Python execution environment used by [CMU CS Academy](https://academy.cs.cmu.edu).

CMUlimatic allows restricted student Python code to bypass the interpreter's lockdown protections and access JavaScript capabilities in the browser's Web Worker environment.

## Vulnerability Overview:

| Property | Details |
|---|---|
| **Affected platform** | CMU CS Academy (Web IDE) |
| **Affected component** | Brython fork maintained by CMU |
| **Vulnerability type** | Interpreter Sandbox Escape |
| **CWE** | CWE-693 (Protection Mechanism Failure) |
| **Severity** | N/A |
| **CVE** | N/A |
| **Status** | Mostly reproduced, disclosure is still pending |

## Research & Validation:

The vulnerability was identified through AI-assisted security research and reproduced against the public Brython source code.

Testing was performed, confirmed the lockdown bypass in CS Academy's production IDE, including access to worker-global JavaScript capabilities, network requests, and worker-to-page messaging.

No malicious testing was preformed.

## Responsible Disclosure:

This research is being prepared for coordinated disclosure to Carnegie Mellon University's security team.

Technical reproduction details and supporting evidence will be published following remediation and disclosure coordination.

## Credits:

**Discovered and researched by:** [Nodesclock](https://github.com/Nodesclock)

**AI models used:** Deepseek v4.1 Flash, Yandex Alice, ChatGPT 6 Astra.

## License

This project is licensed under the [MIT License](LICENSE). The license applies only to original research materials and code created for this project.

*CMUlimatic is an independent security research project and is not affiliated with, sponsored by, or endorsed by Carnegie Mellon University.*
