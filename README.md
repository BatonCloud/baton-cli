# Baton

**English** · [简体中文](README.zh-CN.md)

**Baton — Open-Source Agent & Network Runtime Framework**

Run AI agents as isolated, persistent runtime nodes — on the laptop you already
have, or on a server you own. Then connect those nodes into a network of your own.

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

## What Baton solves

Baton runs AI agents — Claude Code, Codex, OpenClaw and others — as **isolated,
persistent runtime nodes**. Three problems, in the order you hit them:

- **Agents fighting over one machine.** Each gets its own node: its own filesystem, dependency tree and identity.
- **Agents that cannot reach each other.** Nodes join a network of yours, and an agent is addressed by name rather than by host.
- **Collaboration that costs you access.** Networks open to each other under a domain, without handing over a key.

The first is built today. The other two are accepted decisions with no code
behind them yet, and [what is built](https://dev.baton.wiki/status/) says so row
by row.

### Four agents on one machine, and their dependencies fight

One installed Node 24, one needs Node 22 — PATH, pip and the interpreters all
fight, and nobody knows what changed.

> **One isolated node per agent.**
>
> Own filesystem, own dependency tree, own identity. `baton attach` walks you
> into its real terminal. It outlives the lid closing and the dropped tunnel.
>
> `Claude Code · Codex · OpenClaw · Hermes` — a runtime is declared by a spec
> file, not compiled in. The first three are the ones exercised today.

### The agent that works is on the wrong machine

Two separate installs that share nothing — not a name, not a message, not a
file — so making them work together means copying things by hand.

> **One network, not eight installs.**
>
> Nodes find each other, share what they are allowed to, and pass messages.
> `sales@your-network` addresses the *agent* — never the machine it sits on — so
> a route survives the agent moving.

### The people you want to work with are outside your network

Working across two teams means handing over an API key, a shared server, or a
copy of your workspace — each gives away more than the task needed.

> **Publish the network.**
>
> Outside agents join under a domain, and one address form covers both cases:
> whether the two agents share a laptop or two continents is a routing detail,
> not a different API. Trust is the **signed descriptor**, not the server that
> answered, and the resolver is replaceable, so nobody sits in the path by
> default.

**One of these three is built.** The first is real and you can run it today. The
network and the public network are accepted decisions with no code behind them
yet — we publish the design before the code on purpose, because it is far
cheaper to argue with a decision than with a shipped mistake.

---

## Where Baton sits

Applications call agents. Agents run inside a workspace. **Baton is what binds a
workspace to the compute underneath it.**

```
  APPLICATIONS     Cursor · Copilot · Windsurf · Perplexity · Replit · LangChain
        │
        │  Agent API ↓
        │
  AGENT NETWORK    ◆ BATON — an agent & network runtime framework
        │
        │  Provider contract ↑     ← the only arrow that points up
        │
  RUNTIME LAYER    Local machine · Third-party cloud · Managed agent nodes
        │
        │  Runs ↓
        │
  AGENT HARNESS    Claude Code · Codex · OpenClaw · Hermes
        │
        │  Inference API ↓
        │
  LLM LAYER        ChatGPT · Claude · Gemini · DeepSeek · Kimi · GLM · Qwen
```

> **One isolated node per agent. Baton creates them, watches them, holds their
> identity — and never reads a prompt.**

The compute binds *up* to Baton across the provider contract, which is why the
machine underneath can be replaced without redesigning anything above it.

Baton owns identity, workspace, the runtime process, the container instance, the
TTY, the filesystem, secrets, environment, lifecycle, health, logs — and, since
ADR-0020, the delivery of a message from one agent to another. It does not own —
and will not grow into — prompts, memory, context windows, reasoning, planning,
tool selection, model or token choices, conversations, knowledge stores, or what
any of those messages mean.

---

## Features

| | |
|---|---|
| **Declare the agent, declare the network** | A runtime is a YAML spec — image, command, session, secrets, health. Unknown fields are refused rather than quietly ignored. [Detail →](https://dev.baton.wiki/features/declare-agent-and-network) |
| **P2P and N2N, one address form** | `agent@network` resolves agent-to-agent inside a network and network-to-network across domains. `ssh`, `scp` and port forwarding keep working. [Detail →](https://dev.baton.wiki/features/p2p-and-n2n) |
| **Decentralised, and yours to route** | Nodes dial *out* over one outbound connection and listen on nothing. No node has to be reachable, and no SSH key is handed over. [Detail →](https://dev.baton.wiki/features/decentralised) |
| **Managed is an addition, never a subtraction** | Nothing in the tree exists in order to be switched off — no edition build tags, no entitlement checks, no license gates — and CI fails the build if any appear. [Detail →](https://dev.baton.wiki/features/managed-is-additive) |

---

## Quick start

Installing the CLI, founding a network and creating a workspace are three
different acts, and Baton keeps them apart on purpose.

**The package is not on npm yet**, so step 1 is the one you cannot run today.

### 1 · Install the CLI

```bash
npm i -g @batoncloud/baton-cli   # not published yet — see below
baton version                    # only the CLI arrived: no node, no network,
                                 # no container. Nothing has been created.
```

### 2 · Found a network on this machine

```bash
baton init             # this machine becomes a Baton node — writes one local
                       # file, registers nowhere, starts zero containers

baton setup master     # ...and additionally takes the master role: founds the
                       # network, issues the internal CA, hands you an operator
                       # certificate. This is the moment the network exists.

baton create agent codex01 --runtime ./codex.yaml
                       # an agent, its workspace, and the container behind it.
                       # --secret NAME=/path names a host file, never a value.
```

`baton join <url>` — registering this node into a network founded elsewhere — is
the fourth step in the design and is **not implemented**.

### 3 · Walk in, or drive it

```bash
# for humans — the panel
baton web              # mints a 60-second handoff from your operator cert and
                       # opens a browser session, read-only by construction

baton attach codex01   # the real terminal, exactly as it looks locally.
                       # --takeover to type, under a lease, audited both ends.

# for automation — the CLI is the API
baton status  --output json    # every command speaks JSON on request
baton node list --output json  # and exits with a documented code, not a guess
```

The full operator panel that replaces the read-only console is decided
(ADR-0029) and being built. What ships today is the viewer.

**Requirements**, when the package does ship:

| | |
|---|---|
| Node.js | `>=20 <25` |
| Platform | Linux, macOS |
| Container engine | Docker |

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
| **Agent network** | `join` · agent-to-agent delivery · federation | **Decided, not built** |

Also true, and worth knowing before you form an opinion:

- There is exactly **one runtime driver** — Docker — and it is not abstracted
  behind an interface yet. Runtime independence is a stated principle, not a
  description of the code.
- **Two agents cannot talk to each other today.** The node channel carries
  operator streams only, and ADR-0027 says so in the protocol rather than
  leaving you to discover it.
- **Do not run this in production.**

---

## What Baton is not

- **Not an agent framework.** LangGraph and CrewAI decide what an agent thinks
  about. Baton decides where it runs. They compose; they do not compete.
- **Not a sandbox, and not a competitor to one.** Sandboxes isolate a code
  execution. Baton manages a long-lived runtime that has an identity, a
  workspace that outlives any single task, and a terminal you can walk into.
- **Not "Kubernetes for AI agents."** Kubernetes schedules fungible, replaceable
  workloads. An agent is not fungible — it is the same agent after it moves.

More of these, answered at length, in the
[FAQ](https://dev.baton.wiki/#faq).

---

## Documentation

Full documentation lives at **<https://dev.baton.wiki>**, in English and
Simplified Chinese.

That site is a **development preview** and tracks the code, which means it will
sometimes describe things ahead of a release. It is the same source of truth we
use, not a marketing surface.

| | |
|---|---|
| [Start](https://dev.baton.wiki/start/) | Eight ordered steps, from what you need installed to tearing it back down. |
| [What is built](https://dev.baton.wiki/status/) | The capability table, with a source for every row — including the rows that say no. |
| [The decisions](https://dev.baton.wiki/decisions/) | Every decision record, written before the code landed, each stating what it costs. |

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
