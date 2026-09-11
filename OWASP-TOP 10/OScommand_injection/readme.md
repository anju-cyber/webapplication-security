# OS Command Injection

This folder documents an **OS Command Injection** vulnerability identified during authorized security testing.

The vulnerability affects the `email` parameter of the feedback submission endpoint. User-controlled input can influence server-side operating-system command execution.

The vulnerability was also tested as a **blind command injection** scenario and validated through an out-of-band DNS interaction using Burp Collaborator.

## Finding Overview

| Field                | Details                             |
| -------------------- | ----------------------------------- |
| Vulnerability        | OS Command Injection                |
| Severity             | High                                |
| Affected Endpoint    | `POST /feedback/submit`             |
| Affected Parameter   | `email`                             |
| Testing Tool         | Burp Suite Professional             |
| Validation Technique | Burp Collaborator / Out-of-Band DNS |
| Status               | Confirmed                           |

## Attack Flow

```text
Feedback Form
      ↓
User-Controlled Email Parameter
      ↓
Capture Request
      ↓
Burp Repeater
      ↓
Command Injection Testing
      ↓
Blind Command Execution
      ↓
Burp Collaborator
      ↓
Out-of-Band DNS Interaction
      ↓
Command Execution Confirmed
```

## Key Security Observation

The application allows user-controlled input to reach an operating-system command execution context without sufficient separation between application data and executable commands.

The blind command execution was validated through an out-of-band DNS interaction.

## Evidence

Detailed reproduction steps and technical evidence are available in [`report.md`](./report.md).

### Screenshots

* `01-feedback-form.png` — Feedback form containing the user-controlled email field.
* `02-original-request.png` — Original feedback submission request.
* `03-command-injection-payload.png` — Modified request containing the command-injection payload.
* `04-blind-command-injection.png` — Blind command-injection request/response behavior.
* `05-burp-collaborator-interaction.png` — Burp Collaborator DNS interaction confirming out-of-band activity.

## Recommended Remediation

* Avoid shell-based command execution where possible.
* Use safe APIs and parameterized process execution.
* Validate user input server-side.
* Separate data from executable commands.
* Apply least-privilege execution.
* Use appropriate application isolation and monitoring.

## Classification

**OS Command Injection**

## Lab Disclaimer

This finding was identified during authorized security testing for educational and defensive security purposes.
