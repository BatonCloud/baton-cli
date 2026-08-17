# Security Policy

## Reporting a vulnerability

**Do not open a public issue for security problems.**

Email **security@mailloop.dev** with:

- affected component (`control-api`, `agent`, `cli`, `admin-web`) and version
- reproduction steps or proof of concept
- impact you believe it has

We acknowledge within **3 business days** and aim to ship a fix for
Critical/High within **14 days**, Medium within **60 days**.

If you prefer encrypted mail, request our PGP key in a first message
containing no details.

## Supported versions

Until 1.0, only the latest minor release receives security fixes.

## What we consider in scope

- authentication, enrollment, and certificate handling
- privilege escalation across node / admin / agent boundaries
- secret exposure in logs, API responses, support bundles, or images
- container escape from the shipped images
- capability invocation bypassing the registry or policy layer

## Out of scope

- vulnerabilities requiring an already-root attacker on the same host
- misconfiguration explicitly warned about in `baton doctor`
- denial of service from unbounded local resource use in a POC/dev deployment

## Our commitments

- Severe issues may be met with revoked release signatures, blocked
  versions, emergency CA rotation, and a forced-upgrade advisory.
- **License terms are not a security control.** We do not use obfuscation,
  hidden interfaces, or backdoors to enforce anything.

## A note on this repository

This repository currently carries documentation and licensing only; the CLI
source is not published here yet. Reports about the shipped product are still
welcome at the address above — the policy covers the product, not this file
tree.
