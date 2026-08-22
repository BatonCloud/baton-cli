# Baton

[English](README.md) · **简体中文**

**Baton — Open-Source Agent & Network Runtime Framework**

把 AI agent 变成可管理的 runtime node。

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

## 问题是什么

```
周一    pip install 装了点什么
周二    apt install 装了点别的
周三    rm -rf 删了一个看起来没用的目录
周四    Claude Code：「有东西坏了」
```

没人知道到底改了什么。环境是一次一点地漂移过去的，而且回不来。与此同时，同一台
机器上第二个 agent 要 Node 22，第一个装的却是 Node 24；第三个要 Python 3.10，第四个
要 3.12 —— PATH、pip、npm 和各个解释器于是互相打架。

接下来是一批没有工具能回答的问题：*agent A 在哪个终端里？笔记本合上以后它还活着吗？
我怎么把它搬到服务器上？* 还有最晚出现、也最难受的那一个 ——
**能干活的那个 agent 在这台机器上，而需要它的那个 agent 在另一台机器上。**

---

## 你能用它搭出三样东西

### 1 · 一个完全由你掌控的 agent 沙箱 &nbsp;·&nbsp; **已实现**

在你自己的笔记本上，或你自己的服务器上。一个节点只承载一个 agent 运行时 ——
自己的文件系统、workspace、依赖树和身份 —— 所以那个要 Python 3.12 的 agent，根本
够不到装了 3.10 的那一个。

`baton attach` 把你送进它真正的终端，默认只读。节点被 supervisor 托管，比连接活得
更久：笔记本合上、隧道断掉，活还在继续。

```
Claude Code · Codex · OpenClaw · Hermes
```

一个运行时是由 spec 文件声明的，不是编译进去的。前三个是今天真正跑过的；接入另一个
是写一份声明，不是改一次代码。

### 2 · 一个属于你自己的 agent 网络 &nbsp;·&nbsp; **已裁定，未实现**

笔记本上的两个节点和服务器上的六个节点，不再是八份互不相干的安装，而是一个网络：
它们在本地彼此发现、按允许的范围共享资源、互相发消息。

每个 agent 带着自己的路由身份，所以 `sales@your-network` 寻址的是**这个 agent**，
而永远不是它此刻恰好待着的那台机器 —— 人也用手上现成的 `ssh` 就能进去。

已经完整裁定，**但背后还没有代码**：身份与收件箱（ADR-0020）、远程 attach 需要的
中继（ADR-0027）、以及地址本身（ADR-0028）。

### 3 · 一个可以公开出去的网络 &nbsp;·&nbsp; **已裁定，未实现**

把网络挂在一个域名下公开，外部的 agent 就能加入进来协作。一种地址形式覆盖两种情况：
两个 agent 是共用一台笔记本、还是隔着两个大洲，只是路由细节，不是另一套 API。

被信任的是**签名后的 descriptor**，不是回答这次查询的那台服务器；解析器可替换 ——
所以默认情况下没有任何一方（包括我们自己）站在通路中间（ADR-0031）。

> **这三样里，今天只有一样是建成的。** 沙箱是真的，你今天就能跑起来。网络与公开
> 网络是已经接受、但背后还没有代码的决策。我们刻意先公开设计再公开代码，因为跟一个
> 决策争论，远比跟一个已经发出去的错误争论便宜 —— 但上面任何一条都不该被读成
> 「现在就有」。逐行的对照表在 <https://dev.baton.wiki/zh/status/>。

### 四句话讲完全部特性

| | |
|---|---|
| **声明你的 agent，也声明你的网络** | 一个运行时就是一份 YAML spec —— 镜像、命令、会话、密钥、健康检查。未知字段会被直接拒绝，而不是悄悄忽略。[spec →](https://dev.baton.wiki/zh/reference/runtime-spec) |
| **P2P 与 N2N，同一种地址** | `agent@network` 在网络内部解析成 agent 到 agent，跨域时解析成网络到网络。`ssh`、`scp` 和端口转发照常能用。[ADR-0028 →](https://dev.baton.wiki/decisions/0028-ssh-addressing) |
| **完全去中心化，流量由你自己走** | 节点只**向外**拨出一条连接，自己不监听任何端口。没有哪个节点必须可达，也不用交出 SSH key。[节点跑在哪 →](https://dev.baton.wiki/zh/concepts/where-nodes-run) |
| **Managed 只做加法，从不做减法** | 代码树里没有任何东西是为了「被关掉」而存在的 —— 没有版本编译标记、没有授权校验、没有 license 开关，一旦出现，CI 直接让构建失败。[ADR-0003 →](https://dev.baton.wiki/decisions/0003-edition-boundary) |

---

## Baton 在哪一层

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
  Agent 层         Claude Code · Codex · OpenClaw · Hermes
        │
        │  Inference API ↓
        │
  LLM 层           ChatGPT · Claude · Gemini · DeepSeek · Kimi · GLM · Qwen
```

> **Baton 管的是运行时，从不管智能本身。**

这句话是一条边界，不是一句口号：它决定了哪些功能建在这里，哪些属于别人的产品。
应用调用 agent，agent 跑在 workspace 里。**Baton 是把 workspace 绑到底下算力上的
那一层** —— 而算力是通过 provider contract **向上**绑到 Baton 的，这正是底下那台
机器可以换掉、上面却不用重新设计的原因。

Baton 拥有身份、workspace、运行时进程、容器实例、TTY、文件系统、密钥、环境、生命
周期、健康、日志 —— 以及从 ADR-0020 起，一条消息从一个 agent 送到另一个 agent
这件事本身。

它不拥有、也不会长成 —— prompt、记忆、上下文窗口、推理、规划、工具选择、模型与
token 的选择、对话、知识库，也不拥有那些消息到底是什么意思。

---

## 怎么做？三步

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
baton web              # 用你的 operator 证书换一张 60 秒的一次性凭据，打开
                       # 浏览器会话。构造上就是只读的：今天的控制台压根
                       # 没有任何会改状态的 API。

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

## 参与贡献

本仓库的文档与翻译，现在就欢迎贡献。见 [`CONTRIBUTING.md`](CONTRIBUTING.md) ——
提交按 [DCO](https://developercertificate.org/) 签署（`git commit -s`），没有 CLA。

新增一种语言：见 [`TRANSLATIONS.md`](TRANSLATIONS.md)。

社区行为准则：[`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md)。

**安全问题不要提到 issue 里。** 见 [`SECURITY.md`](SECURITY.md)。
