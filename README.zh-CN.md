# Baton

[English](README.md) · **简体中文**

**Baton — 智能体互联开源框架**

把 AI agent 跑成相互隔离、持久存在的 runtime node —— 在你手边这台笔记本上，或在你
自己的服务器上。然后把这些节点连成属于你自己的网络。

- **安全隔离沙箱** —— 一个节点一个运行时：自己的文件系统、依赖、身份。
- **可组合技术栈** —— 换 harness，工作空间不动。
- **会积累的工作** —— 一次运行的产出可以变成设置的一部分。
- **能协同的 agent** —— 按名字找到、交接工作、送回结果。
- **网络之间互联** —— 网内私有，想开放时再开放。

> **每个 agent 都有自己的节点，每个节点都在你的网络里。**

> **Pre-alpha。v1.0 之前不承诺任何兼容性。**
> 命令名、flag、文件格式、磁盘布局都可能在没有废弃期的情况下改变。pre-1.0 各版本
> 之间没有升级路径。

---

## 这个仓库是什么

这里是 **`baton`** 的公开主页 —— Baton 平台的命令行入口。目前，它承载的只有本项目
的公开文档与许可信息。

**CLI 源码还不在这里。** 它会在剩余的发布闸口清完之后，从上游仓库发布出来：商标
签署、全历史凭据扫描、第三方安全审查。这个仓库先存在，是为了让许可证、商标政策、
安全联系方式，以及一份对产品的诚实描述，**在代码公开之前**就已经公开、可被引用。

比起端出一份描述你根本拿不到的软件的 README，我们宁愿先讲清楚今天什么是真的。

---

## Baton 解决了什么？

五层，而顺序本身就是论证：每一层只有在它上面那层成立之后才有意义。agent 得先有地方
待着，它的资产才谈得上可携带；资产可携带了，工作才谈得上在里面积累；有了积累，共享
才值钱。

### 1 · 建一次 → 到处跑 → 7×24 一直跑

一个 agent 装了 Node 24，下一个要 Node 22，第三个要 Python 3.10，第四个要 3.12 ——
而且没人知道到底改了什么。然后笔记本一合，或者活得挪到服务器上，一切重来一遍。

> 一个工作空间只承载一个 agent 运行时：自己的文件系统、依赖树和身份。`baton attach`
> 把你送进它真正的终端，默认只读；它由 supervisor 托管，所以合上笔记本、断掉隧道，
> 它都还活着。**能搬走的单位是这个工作空间 —— 底下那台机器只是一个可替换的 provider。**
>
> `Workspace` · `Template` · `Snapshot` · `Restore` · `Migration` · `Local & Cloud Node`

### 2 · 一处切换 Harness / Model / Skills / 配置

你的 instructions、Skills、MCP 配置、工作约定，全都长在 Claude Code 里——或者长在
Codex 里、长在某个 repo 里。换一个 harness 就要重建一遍，因为这些资产属于工具，
不属于你。

> 工作空间是稳定的那一层，harness 只是一个驱动。配置、资源、密钥和文件待在工作空间
> 里；Claude Code、Codex、OpenClaw 是驱动它的三种方式，由一份 spec 文件声明，而不是
> 编译进去的。**Baton 搬运并为它们做版本，但从不打开它们。**
>
> `Harness Adapter` · `Runtime Config` · `Model` · `Skills` · `Resources`

### 3 · 会自我改进的工作空间

隔离是目的，也是代价：你教会某个 agent 的约定、另一个写下的文档、第三个攒起来的
context，全都被困在它们各自产生的地方。每次任务都很聪明，工作空间却从来没变聪明。

> 一次运行的产出落在工作空间里，而不是落在聊天记录里 —— 从那里它可以被保留、被版本化，
> 被并入这个工作空间自己的配置和资源。判断哪一次产出值得留下，是 agent 和你的判断；
> Baton 的职责是让它沉淀的那个地方，比这次运行、这台机器和这个 harness 活得更久。
>
> `Output` → `Evaluate` → `Promote` → `Config` / `Skill` / `Knowledge` / `Workflow`

### 4 · 通讯与协同

能干活的那个 agent 在这台笔记本上，需要它的那个在服务器上，两份安装什么都不共享。
而当你真的有五个 agent 时，你看不到谁在干活、谁闲着、谁在等你。

> 节点彼此发现、互相交接工作。`sales@your-network` 寻址的是**这个 agent**，而不是它
> 此刻待着的那台机器。一条消息带着请求、它的上下文、它引用的资源，以及它产出的结果 ——
> 那是一套工作协议，不是聊天。**Baton 搬信封，从不决定接下来发生什么。**
>
> `Identity` · `State` · `Inbox` · `Request` · `Result` · `Shared Resources`

### 5 · 经验在网络里复利

跨团队协作意味着交出一把 API key、架一台共享服务器，或者复制一份 workspace 到别人
账号里 —— 每一种交出去的都比需要的多。于是每个组织的 agent 都在从零研究同样的东西、
踩同样的坑。

> 一种地址形式覆盖两种情况。被信任的是**签名后的 descriptor**，不是回答这次查询的
> 那台服务器；解析器可替换，默认没有任何人站在通路中间。当一个 skill 或一份文档可以
> 跨边界被寻址、被信任，一个 agent 积累下来的经验，就不再被困在产生它的那个工作空间里。
>
> `Discovery` · `Sharing` · `Provenance` · `Federation` · `Network Resources`

**这五样里，今天只有一样是建成的。** 沙箱是真的，你今天就能跑起来。其余都是已经接受、
但背后还没有代码的决策 —— 我们刻意先公开设计再公开代码，因为跟一个决策争论，远比跟
一个已经发出去的错误争论便宜。逐行的对照表见[现状](#现状--今天到底有什么)。

---

## Baton 在哪一层

应用调用 agent，agent 跑在 workspace 里。**Baton 就是把 workspace 绑到底下算力上的
那一层。**

```
  应用层           Cursor · Copilot · Windsurf · Perplexity · Replit · LangChain
        │
        │  Agent API ↓
        │
  智能体网络        ◆ BATON — 智能体互联开源框架
        │
        │  Provider contract ↑     ← 全图唯一向上的箭头
        │
  运行时/容器层     本地机器 · 云平台 · 托管 agent 节点
        │
        │  Runs ↓
        │
  Harness 层       Claude Code · Codex · OpenClaw · Hermes
        │
        │  Inference API ↓
        │
  LLM 层           ChatGPT · Claude · Gemini · DeepSeek · Kimi · GLM · Qwen
```

> **每个 agent 一个隔离节点。Baton 创建它们、观察它们、持有它们的身份 ——
> 而永远不读一句 prompt。**

算力是通过 provider contract **向上**绑到 Baton 的，这正是底下那台机器可以换掉、
上面却不用重新设计的原因。

Baton 拥有身份、workspace、运行时进程、容器实例、TTY、文件系统、密钥、环境、生命
周期、健康、日志 —— 以及从 ADR-0020 起，一条消息从一个 agent 送到另一个 agent
这件事本身。它不拥有、也不会长成 —— prompt、记忆、上下文窗口、推理、规划、工具
选择、模型与 token 的选择、对话、知识库，也不拥有那些消息到底是什么意思。

---

## Baton 不提供什么

四件 Baton 不会长成的事。每一件都是接缝，不是缺口 —— 它是生态伙伴接进来的地方，
也是这一层上下两层可以是别人产品的原因。

| 不归我们 | 归谁 |
|---|---|
| **agent 的思考** —— prompt、记忆、上下文窗口、推理、规划、任务图 | harness 与框架：Claude Code、Codex、OpenClaw、LangGraph、CrewAI |
| **模型与推理** —— 这里不调模型、不花 token、不做路由 | 你的 harness 指向的那个 provider |
| **隔离与算力** —— 我们不自己造沙箱、不造虚拟机、不造调度器 | Docker、microVM、E2B，以及你已经在付费的任何云 |
| **技能与文档的含义** —— 不定义 skill 格式、不解析、不建索引。Baton 搬这个文件，从不打开它 | MCP server、skill 注册表，以及你的 agent 本来就在用的工具 |

---

## 功能特性

| | |
|---|---|
| **声明你的 agent，也声明你的网络** | 一个运行时就是一份 YAML spec —— 镜像、命令、会话、密钥、健康检查。未知字段会被直接拒绝，而不是悄悄忽略。[详情 →](https://dev.baton.wiki/zh/features/declare-agent-and-network) |
| **P2P 与 N2N，同一种地址** | `agent@network` 在网络内部解析成 agent 到 agent，跨域时解析成网络到网络。`ssh`、`scp` 和端口转发照常能用。[详情 →](https://dev.baton.wiki/zh/features/p2p-and-n2n) |
| **完全去中心化，流量由你自己走** | 节点只**向外**拨出一条连接，自己不监听任何端口。没有哪个节点必须可达，也不用交出 SSH key。[详情 →](https://dev.baton.wiki/zh/features/decentralised) |
| **Managed 只做加法，从不做减法** | 代码树里没有任何东西是为了「被关掉」而存在的 —— 没有版本编译标记、没有授权校验、没有 license 开关，一旦出现，CI 直接让构建失败。[详情 →](https://dev.baton.wiki/zh/features/managed-is-additive) |

---

## 快速开始

装 CLI、建网络、建 workspace 是三件不同的事，Baton 刻意把它们分开。

**npm 包还没有发布**，所以第 1 步是你今天唯一跑不了的一步。

### 1 · 安装 CLI

```bash
npm i -g @batoncloud/baton-cli   # 尚未发布 —— 见下文
baton version                    # 到手的只有 CLI：没有节点、没有网络、
                                 # 没有容器。什么都还没被创建。
```

### 2 · 在这台机器上建起网络

```bash
baton init             # 这台机器成为一个 Baton 节点 —— 只写一个本地文件，
                       # 不向任何地方注册，启动零个容器

baton setup master     # ……并额外承担 master 角色：建起网络、签发内部 CA、
                       # 把 operator 证书交给你。网络就是在这一刻存在的。

baton create agent codex01 --runtime ./codex.yaml
                       # 一个 agent、它的 workspace，和背后的容器。
                       # --secret NAME=/path 给的是宿主机文件路径，绝不是值。
```

把本节点注册进别处建起的网络的 `baton join <url>`，是设计中的第四步，**尚未实现**。

### 3 · 走进去，或者把它开起来

```bash
# 给人用 —— Panel
baton web              # 用你的 operator 证书换一张 60 秒的一次性凭据，
                       # 打开浏览器会话，构造上就是只读的

baton attach codex01   # 真正的终端，跟你在本地看到的一模一样。
                       # --takeover 才能打字，带租约，两端都留审计。

# 给自动化用 —— CLI 就是 API
baton status  --output json    # 每条命令都可以说 JSON
baton node list --output json  # 并且以有文档的退出码结束，而不是靠猜
```

那个用来取代只读控制台的完整 operator Panel，已经裁定（ADR-0029）并且正在建。
今天上线的是只读的那一版。

包发布之后的**环境要求**：

| | |
|---|---|
| Node.js | `>=20 <25` |
| 平台 | Linux、macOS |
| 容器引擎 | Docker |

---

## 现状 —— 今天到底有什么

Baton 处于 pre-alpha，本项目自己的文档对「设计」与「代码」之间的差距一向直说。

| 领域 | 命令 | 状态 |
|---|---|---|
| 生命周期 | `create` · `start` · `stop` · `restart` · `destroy` | 已实现 |
| 观察 | `status` · `logs` · `sessions` · `events` | 已实现 |
| 人工接入 | `attach`（`--takeover`）· `shell` · `console` | 已实现 |
| 集群 | `node` · `token` · `capability` · `call` · `failover` | 已实现 |
| 运维 | `doctor` · `uninstall` · `web` · `version` | 已实现 |
| **可移植性** | `snapshot` · `restore` · `migrate` · `export` · `backup` | **不存在** |
| **Agent 网络** | `join` · agent 间投递 · 联邦 | **已裁定，未实现** |

下面这些同样是真的，值得在你形成判断之前知道：

- 运行时驱动**只有一个** —— Docker —— 而且还没有抽象到接口后面。运行时独立性是一条
  已经写下的原则，不是对代码现状的描述。
- **两个 agent 今天不能互相说话。** 节点通道只承载 operator 流，而且 ADR-0027 把
  这一条写进了协议里，而不是留给你自己撞上去。
- **不要拿它上生产。**

---

## Baton 不是什么

- **不是 agent 框架。** LangGraph 和 CrewAI 决定一个 agent 想什么，Baton 决定它在
  哪里跑。它们是叠加关系，不是竞争关系。
- **不是沙箱，也不跟沙箱竞争。** 沙箱隔离的是一次代码执行。Baton 管的是一个长期存活
  的运行时：它有身份，有比任何单个任务活得更久的 workspace，还有一个你能走进去的终端。
- **不是「AI agent 版的 Kubernetes」。** Kubernetes 调度的是可替换的同质负载。
  agent 不是同质的 —— 它搬完家之后，还是同一个 agent。

更多这类问题，在[常见问题](https://dev.baton.wiki/zh/#faq)里有完整回答。

---

## 文档

完整文档在 **<https://dev.baton.wiki>**，中英双语。

那是一个**开发环境预览站**，跟着代码走，所以它有时会描述还没发布的东西。它是我们
自己在用的同一份事实来源，不是一个营销页面。

| | |
|---|---|
| [开始](https://dev.baton.wiki/zh/start/) | 八个有序步骤，从需要装什么，一直到把整套拆干净。 |
| [建成了什么](https://dev.baton.wiki/zh/status/) | 能力表，每一行都有出处 —— 包括那些写着「没有」的行。 |
| [那些决策](https://dev.baton.wiki/decisions/) | 每一份决策记录都写在代码落地之前，并且都写明了它的代价（目前只有英文）。 |

---

## 许可证与商标

**完整的 Apache-2.0。** 见 [`LICENSE`](LICENSE) 与 [`NOTICE`](NOTICE)。

Baton 代码树里没有任何东西是为了「被关掉」而存在的 —— 没有版本编译标记、没有授权
校验、没有 license 开关 —— 一旦出现，CI 直接让构建失败。扩展走的是导出接口，不是
付费解锁。

**代码是开放的，商标不是。**「MailLoop」「BATON」及相关标识是 MailLoop 的商标。
许可证授予你的是代码上的权利，不是标识上的权利。可以怎么用、不可以怎么用，见
[`TRADEMARKS.md`](TRADEMARKS.md)。

---

## 社区与贡献

Baton 是 Apache-2.0、公开开发，贡献按 [DCO](https://developercertificate.org/)
签署 —— `git commit -s`，没有 CLA。

| | |
|---|---|
| **仓库** | [BatonCloud/baton-cli](https://github.com/BatonCloud/baton-cli) —— 今天装的是文档与许可；发布闸口清完之后，CLI 源码随之公开。 |
| **来跟某个决策争论** | [Issues](https://github.com/BatonCloud/baton-cli/issues)。每一份决策记录都写明了它的代价。如果哪一条是错的，那才是该发生的对话。 |
| **交流群，以及谁在用** | 还没有。讨论区和采用方列表在它们真实存在之后放这里 —— 本项目不印自己没有的社会证明。 |

本仓库的文档与翻译，现在就欢迎贡献。见 [`CONTRIBUTING.md`](CONTRIBUTING.md) ——
提交按 [DCO](https://developercertificate.org/) 签署（`git commit -s`），没有 CLA。

新增一种语言：见 [`TRANSLATIONS.md`](TRANSLATIONS.md)。

社区行为准则：[`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md)。

**安全问题不要提到 issue 里。** 见 [`SECURITY.md`](SECURITY.md)。
