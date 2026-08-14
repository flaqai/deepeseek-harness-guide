# DeepSeek Harness 使用手册

[English](USAGE.md) · [其他语言](#其他语言)

本手册把 DeepSeek Harness 的架构概念转化为实际操作。官方项目仍处于开发者预览阶段，命令、包名和契约可能变化；部署前请用目标版本核对。

## 先选择正确入口

| 目标 | 建议入口 | 原因 |
|---|---|---|
| 交互式使用 DSH | 官方 Web Profile | 最快完成模型配置并查看会话 |
| 自动化或嵌入 Runtime | Headless Profile 或官方 Python SDK | 执行链不依赖浏览器 |
| 增加单项能力 | Runtime Plugin | 最小的生命周期管理单元 |
| 分发插件和配置 | Bundle | 对有序配置行进行版本化分发 |
| 组装产品或环境 | Profile | 组合 Bundle 与本地依赖 |
| 覆盖某个部署 | Patch | 在后置配置层插入或替换配置 |
| 只改变 Agent 行为 | Preset、Prompt 或 Policy Plugin | 不必新建进程架构 |
| 给编码 Agent 增加复用流程 | Agent Skill | 指导开发，不会自动运行在 DSH 内部 |

## 1. 启动官方 Web UI

按官方项目要求安装受支持的 Node.js 版本，然后运行：

```bash
npx @deepseek-ai/dsh web
```

打开 `http://127.0.0.1:3080`，在 Settings 中配置模型服务，创建一次性工作区，并先运行无破坏性的简单任务。

基线检查：

```bash
dsh --help
dsh --profile web --dump-config
```

第一条确认当前 CLI 能力；第二条输出最终插件树，是判断 Bundle 与 Patch 顺序的首要依据。

## 2. 从源码运行

源码阅读、插件开发或固定版本验证使用：

```bash
git clone https://github.com/deepseek-ai/deepseek-harness.git
cd deepseek-harness
pnpm install
pnpm run build
pnpm dsh web
```

修改前先阅读仓库 `AGENTS.md`、记录 Commit、查看工作区包结构并运行相邻测试。第三方教程可能对应另一个预览版本。

## 3. 使用 Python SDK

官方 SDK 面向程序化嵌入，并包含所需 Runtime。安装前先在官方 SDK 文档核对 Python 版本与平台支持：

```bash
python -m venv .venv
. .venv/bin/activate
python -m pip install deepseek-harness-sdk
```

凭证放入环境变量或密钥存储，不要写进源码。首次调用使用无破坏性请求，设置超时，处理结构化错误，并明确宿主如何取消任务和关闭 Runtime。

## 4. 模块地图

### Runtime 组合层

- **Cordis Context**：能力可见范围与生命周期归属。
- **Service**：Provider 与 Consumer 之间的类型化能力接口。
- **Fiber**：一次真实挂载的插件实例。
- **Effect**：由 Fiber 持有、可在卸载时清理的副作用。
- **Event**：插件之间的通知与流程拦截。
- **Loader**：把有序配置持续调和为运行中的插件图。

### Agent 执行层

- **Model Adapter**：转换标准请求与流式响应。
- **Prompt/Context 组装**：生成稳定的模型可见输入。
- **Agent Loop**：在模型输出、工具执行和继续推理之间循环。
- **Tool Registry/Pipeline**：验证、授权、执行并渲染工具结果。
- **Policy、Approval、Sandbox**：三种不同控制，不能相互替代。
- **Subagent/Workflow**：编排更多执行图。

### 状态与呈现层

- **Session Event Log**：模型可见历史的持久事实源。
- **Compaction/Memory**：在不静默改写事实的前提下缩小上下文。
- **Host**：提供 Runtime 能力和高权限操作。
- **Client/Surface**：观察远端状态并呈现 Web 或其他界面。
- **Telemetry/Operations**：输出健康、追踪、失败和清理信号。

## 5. 扩展模块分类

| 类别 | 常见模块 | 首要审查问题 |
|---|---|---|
| 工作流与 Agent | Loop、研究、计划、团队、自动化 | 谁负责取消、预算与完成判断？ |
| 上下文与会话 | 记忆、压缩、输入、回退、Prompt 管理 | 模型可见状态能否由 Event 重放？ |
| 工具与集成 | 文件、Shell、浏览器、MCP、外部 API | 验证、审批与沙箱分别在哪里执行？ |
| 浏览器、视觉与 UI | Web Client、Remote API、OCR、卡片、通知 | UI 是否来自规范化的 Host 状态？ |
| 客户端与发行形态 | 桌面、TUI、移动远控、容器 | 它是插件、客户端、桥接还是 Runtime Fork？ |
| 主题与呈现 | Client 样式与视觉组件 | 是否避免申请不必要的 Runtime 权限？ |
| 开发与运维 | 脚手架、验证、Payload 检查、可观测性 | 诊断结果是否移除密钥和会话隐私？ |

不要把所有生态项目都叫作插件。Runtime Fork 会重新打包 DSH，Client 连接 DSH，Bridge 转换协议，Plugin 则挂载进配置图；四者的安装、信任、升级和回滚方式不同。

## 6. 安全安装外部插件

先使用一次性 Profile。审查源码、许可证、Package Script、所需 Service、Node.js 直接访问、网络目标、子进程、数据保留策略，以及它与固定 DSH Commit 的兼容性。

```bash
dsh plugin --profile demo add <package-or-git-spec>
dsh --profile demo --dump-config
```

Git 依赖固定到 Commit，不要依赖浮动分支。如果包管理器要求授权 Build Script，先停止并审查脚本。随后：

1. 在配置输出中确认配置行和顺序；
2. 只启动一次性 Profile；
3. 测试启动、正常使用、拒绝、超时、卸载、重启和数据持久化；
4. 确认日志和 UI 结果不包含密钥；
5. 在推广使用前记录卸载与回滚方法。

被生态清单收录只代表可发现，不代表通过安全背书。

## 7. 开发插件

先创建职责单一的插件并用本地 Patch 挂载：

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

```yaml
- insert:
    - id: hello
      name: /absolute/path/to/hello-plugin.ts
```

使用该覆盖层启动源码版本：

```bash
pnpm dsh web --patch ./path/to/cordis.yml
```

生产插件还应添加类型化 Config Schema、明确的 `inject`、生命周期安全注册、聚焦测试与打包元数据。预览 API 可能变化，应优先复用目标 Commit 中相邻插件的模式。

## 8. 构建模型可调用工具

把工具拆成六份契约：

1. 严格的输入 Schema；
2. 授权与用户审批；
3. 带取消和超时的有界执行；
4. 规范化结构结果；
5. 简洁、确定、无密钥的模型渲染；
6. 结果继续对模型可见时写入持久 Session Event。

测试 Schema 拒绝、拒绝执行、重复调用、部分失败、密钥脱敏、超大输出、取消和资源清理。Schema 验证不是授权，依赖注入也不是沙箱。

## 9. 按插件图顺序排障

1. 固定并记录 DSH 与插件 Commit。
2. 输出最终配置。
3. 检查 Profile、Bundle、Patch 顺序与插件行。
4. 检查所需 Service 和 `inject` 名称。
5. 检查 Fiber 挂载、Event 注册和 Effect 清理。
6. 检查 Agent Loop、工具流水线和 Session Event。
7. 最后检查 Remote API 和 Client 投影。

该顺序能把配置、生命周期、执行、持久化和呈现故障分开。

## 10. 使用本仓库 Skills

本项目在 [`skills/`](skills/) 提供四个编码 Agent Skill：

- `dsh-repository-explorer`：梳理包、配置、Service、Event、Session 与 Host/Client 边界。
- `dsh-plugin-scaffold`：创建生命周期安全的小型插件，以及可选 Bundle/Profile/Patch。
- `dsh-tool-builder`：设计类型化、受策略控制、可重放的工具。
- `dsh-plugin-review`：审查生命周期、兼容性、供应链、权限、密钥与重放风险。

把完整 Skill 目录复制到编码 Agent 支持的 Skill 路径，或在支持项目级发现时保留于仓库中。Skill 用于指导开发，不通过 `dsh plugin` 安装，也不会自动成为 Runtime Plugin。

## 发布检查表

- [ ] 固定 DSH 与依赖版本。
- [ ] 验证最终插件树。
- [ ] 测试启动、失败、取消、卸载、重新挂载和重启。
- [ ] 确认模型可见状态能够持久重建。
- [ ] 分开描述依赖注入、审批、策略和沙箱。
- [ ] 审查安装脚本、文件、网络、子进程、遥测与保留策略。
- [ ] 从文档和 Fixture 中移除凭证、私有会话、截图、二维码和联系方式。
- [ ] 记录兼容版本、许可证、安装、卸载、迁移与回滚。

## 其他语言

[English](USAGE.md) · [繁體中文](USAGE_tw.md) · [日本語](USAGE_ja.md) · [한국어](USAGE_ko.md) · [Deutsch](USAGE_de.md) · [Español](USAGE_es.md) · [Français](USAGE_fr.md) · [Italiano](USAGE_it.md) · [Português](USAGE_pt.md) · [Русский](USAGE_ru.md) · [العربية](USAGE_ar.md) · [Bahasa Indonesia](USAGE_id.md) · [ไทย](USAGE_th.md) · [Tiếng Việt](USAGE_vi.md)

## 资料来源

- [DeepSeek Harness 官方仓库](https://github.com/deepseek-ai/deepseek-harness)
- [官方架构说明](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.zh.md)
- [官方第一个插件教程](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.zh.md)
- [官方插件打包与安装](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.zh.md)
- [社区生态分类参考](https://github.com/libukai/awesome-deepseek-harness)：仅参考其文字分类，不复用截图、二维码或联系方式。
