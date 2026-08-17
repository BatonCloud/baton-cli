# Contributing

Thank you for looking. This document describes what can be contributed **here,
today** — which is narrower than the project as a whole, and we would rather say
so than let you write a patch that has nowhere to land.

## What this repository accepts right now

This repository carries the public documentation and licensing for
`@batoncloud/baton-cli`. The CLI source is not published here yet; it arrives
once the remaining publication gates are cleared (trademark sign-off, a
full-history credential scan, and a third-party security review).

So, accepted now:

- **Corrections.** Anything in this repository that is wrong, stale, or
  overstates what Baton does. Accuracy reports are the most valuable thing you
  can send us — see the standard below.
- **Translations.** New languages and improvements to existing ones. See
  [`TRANSLATIONS.md`](TRANSLATIONS.md).
- **Clarity.** Wording that made you misunderstand something is a defect.

Not accepted yet, because there is nothing to attach it to:

- CLI features, bug fixes, and tests. Those follow the source when it lands.

## Ground rules

1. **Sign off every commit** — `git commit -s`. Contributions are made under the
   [Developer Certificate of Origin](https://developercertificate.org/).
   **There is no CLA**; you keep your copyright.
2. **Conventional Commits** for commit subjects: `docs: ...`, `fix(readme): ...`,
   `feat(i18n): add ja translation`.
3. **One change per pull request.** A translation and a factual correction are
   two pull requests.

## The accuracy standard

Baton's documentation has one rule above the rest: **it says what is true today,
and labels what is not built as not built.**

Concretely, a change here must not:

- describe a command, flag, or capability that does not exist yet without
  marking it as unimplemented;
- promise portability, snapshots, or migration as available — those verbs do not
  exist in the code;
- soften the pre-alpha and pre-1.0-compatibility notices.

A pull request that makes Baton sound better than it is will be declined, even
when every individual sentence is defensible. Rendering an unbuilt thing as
green is treated as a bug, not as marketing.

## Style

- **English is canonical.** Translations follow the English text; where they
  disagree, English is correct and the translation is the defect.
- Prefer the project's own terms: an **Agent** is who, a **Node** is where, a
  **Workspace** is the Node in use by an Agent. `runtime` is ambiguous on its
  own — say which sense you mean.
- Keep lines wrapped at a readable width. No trailing whitespace.

## Reporting security issues

Do not use the issue tracker. See [`SECURITY.md`](SECURITY.md).

## Code of conduct

Participation is governed by [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md).
