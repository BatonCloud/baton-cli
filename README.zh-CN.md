# Baton

[English](README.md) · **简体中文**

**Baton — Open-Source Agent & Network Runtime Framework**

把 AI agent 变成可管理的 runtime node。

> **Pre-alpha。v1.0 之前不承诺兼容。**
> 命令名、参数、文件格式、磁盘布局都可能在没有弃用期的情况下改变。pre-1.0 版本之间没有升级路径。

---

## 这个仓库是什么

这里是 **`baton`** 的公开主页 —— Baton 平台的命令行入口 —— 但目前它只承载项目的公开文档与许可信息。

**CLI 源码还不在这里。** 它会在剩余的公开门禁清完之后从上游仓库发布：商标签字、全历史凭据扫描、
第三方安全评审。这个仓库先行存在，是为了让许可证、商标政策、安全联系方式，以及一份诚实的产品说明
在代码公开**之前**就是公开且可引用的。

我们宁愿告诉你今天的事实，也不愿给出一份描述着你拿不到的软件的 README。

---

## Baton 是什么

Baton 把 agent 运行时 —— Claude Code、Codex、OpenClaw 以及其它 —— 作为**相互隔离、持久存在的
runtime node** 来运行和管理：在你自己的机器上，或在你自己的服务器上。

> **Baton 管理运行时，永远不管理智能。**

这是一条边界，不是一句 slogan。Baton 拥有身份、workspace、运行时进程、容器实例、TTY、文件系统、
密钥、环境、生命周期、健康状态与日志。它不拥有、也不会长成 —— prompt、记忆、上下文窗口、推理、
规划、工具选择、模型与 token 的取舍、会话、知识库，或 agent 之间的任务路由。

### 它解决的问题：运行时漂移

```
周一    pip install 点什么
周二    apt install 点别的
周三    rm -rf 一个看起来没用的目录
周四    Claude Code：「有东西坏了」
```

没人知道改了什么。环境一次一点地漂移，回不去。与此同时，第二个 agent 要 Node 22，而第一个装了
Node 24 —— 同一个 home 目录，同一台机器。

Baton 的答案有两半。**隔离** —— 每个 agent 运行时一个实例，各有自己的文件系统、workspace、
依赖树、身份，以及一个人可以走进去的终端。**可逆** —— 快照、回滚、模板。

第一半已经建成。**第二半没有：见下面的现状表。**

---

## 现状 —— 今天到底有什么

Baton 处在 pre-alpha，项目自己的文档对「设计」与「代码」之间的差距一向直说。

| 领域 | 动词 | 状态 |
|---|---|---|
| 生命周期 | `create` · `start` · `stop` · `restart` · `destroy` | 已实现 |
| 观察 | `status` · `logs` · `sessions` · `events` | 已实现 |
| 人类接入 | `attach`（`--takeover`）· `shell` · `console` | 已实现 |
| 集群 | `node` · `token` · `capability` · `call` · `failover` | 已实现 |
| 运维 | `doctor` · `uninstall` · `web` · `version` | 已实现 |
| **可移植性** | `snapshot` · `restore` · `migrate` · `export` · `backup` | **不存在** |

还有几件同样为真、值得在形成判断之前知道的事：

- 今天只有**一个运行时 driver** —— Docker —— 而且还没有被抽象到接口后面。运行时无关是一条既定
  原则，不是对代码现状的描述。
- `baton join` —— 加入在另一台机器上成立的 network —— **未实现**。
- **不要在生产环境运行。**

---

## 安装

将要发布的包是：

```
@batoncloud/baton-cli      →  可执行文件：baton
```

**它还没有上 npm。** 今天没有可用的 `npm install`、没有发布的二进制、也没有安装脚本。我们不会在这里
印一条会对你报错的命令；等包发布了，命令会出现在这一节，也只会出现在这一节。

发布之后的**前置要求**：

| | |
|---|---|
| Node.js | `>=20 <25` |
| 平台 | Linux、macOS |
| 容器引擎 | Docker |

---

## 它将怎么工作

装 CLI 不等于成立 network，成立 network 也不等于创建 workspace。Baton 刻意把这三件事分开。

```
install              只有 CLI —— 没有 node、没有 network、没有容器
  ↓
baton init           这台机器成为一个 Baton Node
                     roles: [agent]，network: null，不起任何容器
  ↓
baton setup master   这个 node 额外承担 master：
                     成立 network、签发 CA、起 panel
  ↓
baton create agent <name>
                     一个 Agent、它的 Workspace，以及背后的容器
```

`baton join <url>` —— 把这个 node 注册进别处成立的 network —— 是设计里的第四步，**未实现**。

### Attach 才是那个功能

这里说的 human-in-the-loop，指的是一个终端，不是一个网页 IDE。

```
attach → TTY → 终端 → detach
```

`baton attach <name>` 打开的就是 Claude Code、或 Codex、或 bash，和它在本地的样子一模一样。默认只读；
`--takeover` 取得一份排他租约，并对交接的两端都留审计。

---

## Baton 不是什么

- **不是 agent 框架。** LangGraph 和 CrewAI 决定 agent 想什么，Baton 决定它在哪里跑。它们是组合
  关系，不是竞争关系。
- **不是 sandbox，也不是 sandbox 的竞品。** sandbox 隔离的是一次代码执行；Baton 管理的是一个长期
  存在、有身份、workspace 比任何单个任务活得更久、而且你可以走进去的运行时。
- **不是「agent 版 Kubernetes」。** Kubernetes 调度的是可替换、可互换的负载。agent 不可互换 ——
  它搬走之后还是同一个 agent。

---

## 文档

完整文档在 **<https://dev.baton.wiki>**，中英双语。

该站点是**开发环境预览**，跟着代码走，所以它有时会描述尚未发布的东西。它就是我们自己用的那份事实
来源，不是一个营销页面。

---

## 许可与商标

**完整的 Apache-2.0。** 见 [`LICENSE`](LICENSE) 与 [`NOTICE`](NOTICE)。

Baton 代码树里没有任何一行代码是为了「可以被关掉」而存在的 —— 没有版本编译标记、没有授权校验、
没有许可证门禁 —— 一旦出现，CI 会让构建失败。扩展通过导出的接口发生，不通过付费解锁。

**代码是开放的，商标不是。** "MailLoop" 与 "BATON" 及相关标识是 MailLoop 的商标。许可证授予你对
**代码**的权利，不授予你对**标识**的权利。可以做什么、不可以做什么，见 [`TRADEMARKS.md`](TRADEMARKS.md)。

---

## 参与贡献

本仓库的文档与翻译现在就欢迎贡献。见 [`CONTRIBUTING.md`](CONTRIBUTING.md) —— 提交按
[DCO](https://developercertificate.org/) 签署（`git commit -s`），不走 CLA。

新增语言：见 [`TRANSLATIONS.md`](TRANSLATIONS.md)。

社区行为准则：[`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md)。

**安全问题不要走 issue tracker。** 见 [`SECURITY.md`](SECURITY.md)。
