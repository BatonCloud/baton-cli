# Baton

**English** · [简体中文](README.zh-CN.md)

**Baton — Open-Source Agent & Network Runtime Framework**

Run AI agents as isolated, persistent runtime nodes — on the laptop you already
have, or on a server you own. Then connect those nodes into a network of your own.

- **Isolated sandbox** — one agent runtime per node: its own filesystem, dependencies, identity.
- **Composable stack** — swap the harness, keep the workspace.
- **Work that accumulates** — a run's output can become part of the setup.
- **Agents that coordinate** — reached by name, handed work, sending results back.
- **Networks that interconnect** — private inside, open when you choose.

> **Every agent its own node. Every node your network.**

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

## What does Baton solve?

Five layers, and the order is the argument: each only means something once the
one above it exists. An agent needs somewhere to live before its assets can be
portable; assets have to be portable before work can accumulate in them;
accumulation has to exist before sharing it is worth anything.

### 1 · Build once → Run anywhere → Keep it running 24/7

One agent installed Node 24, the next needs Node 22, a third wants Python 3.10
against a fourth's 3.12 — and nobody knows what changed. Then the laptop closes,
or the work has to move to a server, and it all starts again.

> A workspace carries exactly one agent runtime: its own filesystem, dependency
> tree and identity. `baton attach` walks you into its real terminal, read-only
> by default, and it is supervised, so it outlives the lid closing and the
> dropped tunnel. The workspace is the unit that travels — **the machine under
> it is a provider you can replace.**
>
> `Workspace` · `Template` · `Snapshot` · `Restore` · `Migration` · `Local & Cloud Node`

### 2 · Switch harness, model, skills and config in one place

Your instructions, skills, MCP setup and working conventions live inside Claude
Code — or inside Codex, or inside a repo. Switch harness and you rebuild all of
it, because the assets belong to the tool rather than to you.

> The workspace is the stable layer and the harness is a driver. Config,
> resources, secrets and files sit in the workspace; Claude Code, Codex and
> OpenClaw are three ways to drive it, declared by a spec file rather than
> compiled in. **Baton moves and versions these things; it never opens them.**
>
> `Harness Adapter` · `Runtime Config` · `Model` · `Skills` · `Resources`

### 3 · A workspace that improves itself

Isolation is the point and also the cost: a convention you taught one agent, a
document another wrote and the context a third built up are all stranded where
they were made. Every task starts clever and the workspace never gets smarter.

> A run's output lands in the workspace, not in a chat log, and from there it
> can be kept, versioned and promoted into the workspace's own configuration and
> resources. Deciding which output was good enough to keep is the agent's
> judgement and yours; Baton's job is that the place it accumulates in survives
> the run, the machine and the harness.
>
> `Output` → `Evaluate` → `Promote` → `Config` / `Skill` / `Knowledge` / `Workflow`

### 4 · Communicate & Coordinate

The agent that works is on this laptop; the one that needs it is on the server.
Two installs that share nothing. And with five agents you cannot see which is
working, which is idle, and which is waiting on you.

> Nodes discover each other and pass work. `sales@your-network` addresses the
> *agent* — never the machine it sits on. A message carries a request, its
> context, the resources it refers to and the output it produced: a work
> protocol, not a chat. **Baton moves the envelope and never decides what
> happens next.**
>
> `Identity` · `State` · `Inbox` · `Request` · `Result` · `Shared Resources`

### 5 · Experience that compounds across the network

Working across two teams means handing over an API key, a shared server, or a
copy of your workspace — each gives away more than the task needed. So every
organisation's agents research the same things and learn the same lessons.

> One address form covers both cases. Trust is the **signed descriptor**, not
> the server that answered, and the resolver is replaceable, so nobody sits in
> the path by default. Once a skill or a document can be addressed and trusted
> across a boundary, the experience one agent accumulated stops being trapped in
> the workspace that produced it.
>
> `Discovery` · `Sharing` · `Provenance` · `Federation` · `Network Resources`

**One of these five is built.** The sandbox is real and you can run it today.
The rest are accepted decisions with no code behind them yet — we publish the
design before the code on purpose, because it is far cheaper to argue with a
decision than with a shipped mistake. The row-by-row table is under
[Status](#status--what-exists-today).

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
  HARNESS LAYER    Claude Code · Codex · OpenClaw · Hermes
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

## What Baton does not provide

Four things Baton will not grow into. Each is a seam rather than a gap — it is
where a partner plugs in, and it is the reason the layers above and below this
one can be somebody else's product.

| Not ours | Whose it is |
|---|---|
| **The agent's thinking** — prompts, memory, the context window, reasoning, planning, the task graph | The harness and the framework: Claude Code, Codex, OpenClaw, LangGraph, CrewAI |
| **The model and the inference** — no model is called here, no token is spent here, no routing is done here | Whichever provider your harness is pointed at |
| **The isolation and the compute** — no sandbox, no hypervisor, no scheduler of our own | Docker, microVMs, E2B, and any cloud you already pay for |
| **The meaning of a skill or a document** — no skill format, no parser, no index. Baton moves the file and never opens it | MCP servers, skill registries, and the tools your agents already use |

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

## Community and contributing

Baton is Apache-2.0, developed in the open, and contributed to under the
[DCO](https://developercertificate.org/) — `git commit -s`, and no CLA.

| | |
|---|---|
| **Repository** | [BatonCloud/baton-cli](https://github.com/BatonCloud/baton-cli) — documentation and licensing today; the CLI source once the publication gates clear. |
| **Argue with a decision** | [Issues](https://github.com/BatonCloud/baton-cli/issues). Every decision record states what it costs. If one is wrong, that is the conversation to have. |
| **Chat, and who is using it** | Not yet. A discussion room and an adopters list go here when they exist — this project does not print social proof it does not have. |

Contributions to the documentation and translations in this repository are
welcome now. See [`CONTRIBUTING.md`](CONTRIBUTING.md) — commits are signed off
under the [DCO](https://developercertificate.org/) (`git commit -s`); there is no
CLA.

Adding a language: see [`TRANSLATIONS.md`](TRANSLATIONS.md).

Community expectations: [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md).

**Security issues do not go in the issue tracker.** See
[`SECURITY.md`](SECURITY.md).
