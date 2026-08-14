# DeepSeek Harness 指南

[English](README.md) | [简体中文](README_zh.md) | [繁體中文](README_tw.md) | [日本語](README_ja.md) | [한국어](README_ko.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md) | [Italiano](README_it.md) | [Português](README_pt.md) | [Русский](README_ru.md) | [العربية](README_ar.md) | [Bahasa Indonesia](README_id.md) | [ไทย](README_th.md) | [Tiếng Việt](README_vi.md)

📘 [阅读技术架构指南 →](GUIDE_zh.md)

> 面向 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) 理解、扩展与插件开发的社区多语言指南。

DeepSeek Harness（`dsh`）是 DeepSeek AI 开源的智能体 Harness。它最核心的设计理念是：**一切皆插件（Everything is a Plugin）**。模型适配器、工具、智能体循环、会话存储、权限、沙箱、遥测和用户界面，都可以通过配置进行组合或替换。

本项目希望把这套架构转化为易读的原理说明、可复现的插件教程、可复用的开发工作流，以及一条从“什么是 Harness”到发布生产级 `dsh` 插件的学习路径。

> [!IMPORTANT]
> 本仓库是独立的社区指南，不是 DeepSeek 官方仓库。DeepSeek Harness 目前处于开发者预览阶段，可能出现破坏兼容性的变更。具体实现请始终以[官方仓库](https://github.com/deepseek-ai/deepseek-harness)和[官方文档](https://deepseek-harness.github.io/deepseek-harness/)为准。

## 为什么需要 Harness

模型本身不会自动读取代码仓库、执行命令、调用工具、申请授权、保存会话或从失败中恢复。**Harness** 提供了这些运行环境，并负责协调用户、模型、工具和应用状态之间的循环。

DeepSeek Harness 的特别之处在于，扩展机制并不是附着在核心外面的一层接口；它自身的内置能力也由开发者可使用的同一套插件机制组合而成。这使团队能够：

- 替换模型供应商、工具实现、沙箱、存储或子智能体，而不必维护整个项目的分叉；
- 用可复用组件组装编码、研究、运维或垂直领域智能体；
- 将插件和配置作为带版本的 Bundle 分发；
- 通过 Profile 与 Patch 为不同部署覆盖配置；
- 卸载或热替换插件，并自动清理它注册的副作用。

## 一分钟理解 DeepSeek Harness

### 1. Cordis 负责组合

DeepSeek Harness 基于 [Cordis](https://github.com/cordiverse/cordis) 构建。Cordis 是一个强调“时空可组合性”的元框架：插件向共享 Context 提供服务、类型化事件和可逆副作用；依赖通过声明表达，插件移除时，相关注册能够被撤销。

### 2. 运行中的 Harness 是一棵插件树

`dsh` 从一个 **Profile** 启动，按顺序叠加多个 **Bundle**，再应用用户级与命令行 **Patch**。官方提供的 `web` 和 `headless` 也是组合出来的运行形态，而不是不可替换的特殊核心。

| 概念 | 含义 |
| --- | --- |
| Plugin | 挂载到 Cordis Context 的 TypeScript 模块、对象或服务类。 |
| Bundle | 通过 `dsh.bundle` 提供配置层的 npm 包。 |
| Profile | 列出 Bundle 并保存本地插件依赖的一套可运行组合。 |
| Patch | 插入或替换配置行的 YAML 覆盖层；越晚应用的层优先级越高。 |
| Service | 由一个插件提供、被其他插件消费的类型化能力。 |
| Event | 用于记录事实、观察或拦截智能体流程的持久事件或实时扩展点。 |

### 3. 智能体循环也可替换

默认流程会组装系统提示词和工具 Schema，流式请求模型，通过受保护的执行管线调用工具，将关键事实写入会话事件日志，并持续执行，直到没有待完成的工作。智能体循环、模型适配器、工具注册表和会话日志本身也都是插件提供的能力接缝。

### 4. Host 与 Client 分离

官方 Monorepo 将 Node.js Host 包与浏览器 Client 包分开。Host 服务可以为 Web UI 生成远程 API，Client 插件可以扩展界面。开发插件前，需要先判断能力应位于运行时、浏览器，还是同时跨越两端。

## 本项目计划覆盖的内容

- **快速开始**：运行 Web UI、配置模型、选择工作区和理解权限。
- **架构原理**：Context、可逆副作用、Service、Event、Scope、会话日志与 Turn/Step 生命周期。
- **插件基础**：`apply(ctx)`、依赖注入、配置 Schema、资源清理和热替换。
- **工具插件**：类型化输入、规范化输出、模型可见渲染、策略钩子和受控执行。
- **能力提供者**：模型、文件系统、子进程、沙箱、存储、遥测与子智能体。
- **Web 扩展**：Host/Client 边界、远程 API、UI 扩展与构建顺序。
- **打包分发**：Bundle、Profile、`cordis.patch.yml`、npm/Git 安装、版本与发布。
- **质量与安全**：测试、权限边界、密钥、安装脚本、依赖审查和发布检查。

## 使用官方项目快速开始

准备受支持的 Node.js 版本，以及 DeepSeek API Key（也可以配置其他模型端点），然后运行：

```bash
npx @deepseek-ai/dsh web
```

Web UI 默认位于 `http://127.0.0.1:3080`。在 **Settings → Models** 中添加模型凭证，选择工作区后即可开始会话。

从源码运行：

```bash
git clone https://github.com/deepseek-ai/deepseek-harness.git
cd deepseek-harness
pnpm install
pnpm run build
pnpm dsh web
```

当前环境要求与命令以官方的 [Web UI 指南](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/guide/index.zh.md)和[开发指南](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/development.zh.md)为准。

## 最小插件示例

```ts
import type { Context } from '@deepseek-ai/cordis'

export const name = 'hello-plugin'

export function apply(ctx: Context) {
  ctx.effect(() => {
    console.log('[hello-plugin] loaded')
    return () => console.log('[hello-plugin] unloaded')
  })
}
```

用本地 Patch 挂载模块：

```yaml
- insert:
    - id: hello
      name: '/absolute/path/to/hello-plugin.ts'
```

```bash
pnpm dsh web --patch ./cordis.patch.yml
```

真实插件还应通过 `inject` 声明所消费的 Service，使用 Schemastery `Config` Schema 暴露可配置项，并通过 `ctx` 注册工具或服务，让副作用遵循插件生命周期。建议从官方[第一个插件教程](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.zh.md)开始。

## 值得探索的插件方向

| 方向 | 插件示例 | 主要扩展点 |
| --- | --- | --- |
| 工具 | Issue 管理、数据库查询、部署、代码搜索 | `ctx.tools` |
| 模型 | OpenAI 兼容接口或私有推理服务 | `ctx.llm` |
| 运行环境 | 远程文件系统、容器沙箱、云端子进程 | Capability Service |
| 智能体行为 | 规划、校验、路由、交接、子智能体 | `agent/*` 事件与 Agent Service |
| 持久化 | 团队会话存储、审计归档、对话导出 | Session Event 与 `ctx.sessions` |
| 界面 | 垂直领域面板、设置页、工具结果卡片 | Client Plugin 与远程 API |
| 治理 | 审批策略、脱敏、遥测、成本控制 | Policy 与 Telemetry Event |

## 规划中的 Agent Skills

本指南里的 **Skill** 指 AI 编码助手可复用的指令工作流，和 DeepSeek Harness 运行时 **Plugin** 不是同一个概念。以下 Skill 目前处于规划阶段，**尚未发布**：

| Skill | 用途 |
| --- | --- |
| `dsh-repository-explorer` | 修改代码前梳理 Package、插件配置行、Service、Event 和 Host/Client 归属。 |
| `dsh-plugin-scaffold` | 创建最小插件、配置 Schema、Patch、测试和包元数据。 |
| `dsh-tool-builder` | 构建带校验、渲染、策略钩子和生命周期安全注册的类型化工具。 |
| `dsh-plugin-review` | 审查依赖注入、副作用清理、权限、密钥、打包方式和兼容性风险。 |

未来每个 Skill 都会明确触发场景、前置上下文、执行步骤、安全边界、验证方法，并链接到对应的官方契约。

## 推荐学习路径

1. 运行 `dsh web`，完成一次普通的代码仓库任务。
2. 阅读官方[架构说明](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.zh.md)。
3. 按官方教程构建最小插件。
4. 为插件添加类型化工具与配置 Schema。
5. 使用 `dsh --profile web --dump-config` 查看实际插件树。
6. 将插件打包为 Bundle，并安装到临时 Profile 中验证。
7. 添加测试，审查权限，再考虑发布。

## 权威资料

- [DeepSeek Harness 官方仓库](https://github.com/deepseek-ai/deepseek-harness)
- [DeepSeek Harness 官方文档](https://deepseek-harness.github.io/deepseek-harness/)
- [架构说明](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.zh.md)
- [插件入门](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.zh.md)
- [工具开发](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/tool.zh.md)
- [插件打包与安装](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.zh.md)
- [Cordis](https://github.com/cordiverse/cordis)
- [时空可组合性论文](https://github.com/cordiverse/paper)

## 参与贡献

欢迎提交勘误、翻译、示例、插件案例和可复用 Skill。请让关键结论链接到官方源码或可复现代码；对预览 API 做清晰标注；示例中不要包含真实凭证。

- [技术指南](GUIDE_zh.md)
- [贡献规范](CONTRIBUTING_zh.md)
- [项目路线图](ROADMAP_zh.md)

当前文档结构：

```text
deepeseek-harness-guide/
├── README*.md        # 项目介绍与语言入口
├── GUIDE*.md         # 多语言技术架构指南
├── CONTRIBUTING*.md  # 信息来源、审查与翻译规范
├── ROADMAP*.md       # 分阶段项目演进计划
└── LICENSE
```

## 许可证

[MIT](LICENSE)
