# Baton

**English** · [简体中文](README.zh-CN.md)

**Baton — Open-Source Agent & Network Runtime Framework**

Turn AI agents into manageable runtime nodes.

> **Pre-alpha. No compatibility is promised before v1.0.**
> Command names, flags, file formats, and on-disk layout can change without a
> deprecation period. There is no upgrade path between pre-1.0 versions.

---

## What this repository is

This is the public home of **`baton`**, the command line entry point to the
Baton platform — and, for now, it carries the project's public documentation and
licensing only.

**The CLI source is not here yet.** It is published from the upstream
repository once the remaining publication gates are cleared: trademark sign-off,
a full-history credential scan, and a third-party security review. This
repository exists so that the license, the trademark policy, the security
contact, and an honest description of the product are public and citable
*before* the code is.

We would rather tell you what is true today than ship a README that describes
software you cannot obtain.

---

## What Baton is

Baton runs and manages agent runtimes — Claude Code, Codex, OpenClaw and others
— as **isolated, persistent runtime nodes**: on your own machine, or on a server
you own.

> **Baton manages the runtime, never the intelligence.**

That is a boundary, not a slogan. Baton owns identity, workspace, the runtime
process, the container instance, the TTY, the filesystem, secrets, environment,
lifecycle, health, and logs. It does not own — and will not grow into — prompts,
memory, context windows, reasoning, planning, tool selection, model or token
choices, conversations, knowledge stores, or routing tasks between agents.

### The problem it solves: runtime drift

```
Monday      pip install something
Tuesday     apt install something else
Wednesday   rm -rf a directory that seemed unused
Thursday    Claude Code: "something is broken"
```

Nobody knows what changed. The environment drifts a little at a time and there
is no way back. Meanwhile the second agent needs Node 22 and the first one
installed Node 24, in the same home directory, on the same machine.

Baton's answer has two halves. **Isolation** — one instance per agent runtime,
with its own filesystem, workspace, dependency tree, identity, and a terminal a
human can walk into. **Reversibility** — snapshot, rollback, templates.

The first half is built. **The second half is not: see the status table below.**

---

## Status — what exists today

Baton is pre-alpha and the project's own documentation is deliberately blunt
about the gap between the design and the code.

| Area | Verbs | Status |
|---|---|---|
| Lifecycle | `create` · `start` · `stop` · `restart` · `destroy` | Implemented |
| Observation | `status` · `logs` · `sessions` · `events` | Implemented |
| Human access | `attach` (`--takeover`) · `shell` · `console` | Implemented |
| Cluster | `node` · `token` · `capability` · `call` · `failover` | Implemented |
| Operations | `doctor` · `uninstall` · `web` · `version` | Implemented |
| **Portability** | `snapshot` · `restore` · `migrate` · `export` · `backup` | **Do not exist** |

Also true, and worth knowing before you form an opinion:

- There is exactly **one runtime driver** — Docker — and it is not abstracted
  behind an interface yet. Runtime independence is a stated principle, not a
  description of the code.
- `baton join` — joining a network founded on another machine — is **not
  implemented**.
- **Do not run this in production.**

---

## Installation

The published package will be:

```
@batoncloud/baton-cli      →  binary: baton
```

**It is not on npm yet.** There is no `npm install`, no release binary, and no
install script that works today. We are not printing a command here that would
fail for you; when the package is published, the command will appear in this
section and nowhere else.

**Requirements**, when it does ship:

| | |
|---|---|
| Node.js | `>=20 <25` |
| Platform | Linux, macOS |
| Container engine | Docker |

---

## How it will work

Installing the CLI is not the same thing as creating a network, and neither is
the same thing as creating a workspace. Baton keeps those three separate on
purpose.

```
install              only the CLI — no node, no network, no container
  ↓
baton init           this machine becomes a Baton Node
                     roles: [agent], network: null, zero containers started
  ↓
baton setup master   this node additionally takes the master role:
                     founds a network, issues the CA, starts the panel
  ↓
baton create agent <name>
                     an Agent, its Workspace, and the container behind it
```

`baton join <url>` — registering this node into a network founded elsewhere —
is the fourth step in the design and is **not implemented**.

### Attach is the feature

Human-in-the-loop here means a terminal, not a web IDE.

```
attach → TTY → terminal → detach
```

`baton attach <name>` shows you Claude Code, or Codex, or bash, exactly as it
looks locally. It is read-only by default; `--takeover` acquires an exclusive
lease and audits both edges of the handover.

---

## What Baton is not

- **Not an agent framework.** LangGraph and CrewAI decide what an agent thinks
  about. Baton decides where it runs. They compose; they do not compete.
- **Not a sandbox, and not a competitor to one.** Sandboxes isolate a code
  execution. Baton manages a long-lived runtime that has an identity, a
  workspace that outlives any single task, and a terminal you can walk into.
- **Not "Kubernetes for AI agents."** Kubernetes schedules fungible, replaceable
  workloads. An agent is not fungible — it is the same agent after it moves.

---

## Documentation

Full documentation lives at **<https://dev.baton.wiki>**, in English and
Simplified Chinese.

That site is a **development preview** and tracks the code, which means it will
sometimes describe things ahead of a release. It is the same source of truth we
use, not a marketing surface.

---

## License and trademarks

**Apache-2.0, in full.** See [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE).

There is no code in the Baton tree that exists in order to be switched off — no
edition build tags, no entitlement checks, no license gates — and CI fails the
build if any of them appear. Extension happens through exported interfaces, not
through a paid unlock.

**The code is open; the marks are not.** "MailLoop" and "BATON" and the
associated logos are trademarks of MailLoop. The license grants you rights to
the code; it does not grant rights to the marks. See
[`TRADEMARKS.md`](TRADEMARKS.md) for what you may and may not do with them.

---

## Contributing

Contributions to the documentation and translations in this repository are
welcome now. See [`CONTRIBUTING.md`](CONTRIBUTING.md) — commits are signed off
under the [DCO](https://developercertificate.org/) (`git commit -s`); there is no
CLA.

Adding a language: see [`TRANSLATIONS.md`](TRANSLATIONS.md).

Community expectations: [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md).

**Security issues do not go in the issue tracker.** See
[`SECURITY.md`](SECURITY.md).
