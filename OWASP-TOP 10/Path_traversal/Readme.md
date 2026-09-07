# Path Traversal — Encoded Traversal Filter Bypass

This folder documents a **Path Traversal** vulnerability identified during an authorized PortSwigger Web Security Academy laboratory exercise.

The application attempts to restrict path traversal through input filtering, but the validation can be bypassed using encoded and double-encoded traversal sequences.

## Finding Overview

| Field             | Details                              |
| ----------------- | ------------------------------------ |
| Vulnerability     | Path Traversal                       |
| Attack Type       | Encoded / Double-Encoded Traversal   |
| Severity          | High                                 |
| Affected Endpoint | `GET /image?filename=29.jpg`         |
| Testing Tool      | Burp Suite Professional              |
| Environment       | PortSwigger Web Security Academy Lab |
| Status            | Confirmed                            |

## Attack Flow

```text
Image Request
      ↓
Identify filename Parameter
      ↓
Test Direct Traversal
      ↓
Direct Payload Blocked
      ↓
Encode Traversal Sequence
      ↓
Filter Bypass
      ↓
Server Resolves Manipulated Path
      ↓
Unauthorized File Access
```

## Key Security Observation

The application relies on input filtering to prevent path traversal but does not adequately canonicalize and validate the final filesystem path.

Encoding and double encoding can therefore bypass the filtering logic.

## Evidence

Detailed reproduction steps and technical evidence are available in [`report.md`](./report.md).

### Screenshots

* `01-image-request.png` — Normal image request using the `filename` parameter.
* `02-path-traversal-attempt.png` — Direct traversal attempt blocked by the application.
* `03-encoded-path-traversal.png` — Encoded/double-encoded traversal used to bypass filtering.
* `04-sensitive-file-response.png` — Result of accessing a file outside the intended directory.

## Recommended Remediation

* Canonicalize paths before validation.
* Ensure the resolved path remains inside the permitted directory.
* Prefer server-side file identifiers over user-controlled filesystem paths.
* Apply validation after decoding and normalization.
* Use least-privilege filesystem permissions.
* Avoid blacklist-only security controls.

## Classification

**Path Traversal / Directory Traversal**

## Lab Disclaimer

This finding was identified in an authorized PortSwigger Web Security Academy laboratory environment for educational and defensive security testing.
