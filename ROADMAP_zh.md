# 路线图

[English](ROADMAP.md) | [简体中文](ROADMAP_zh.md)

本路线图优先推进小型、有源码依据、容易审查的增量，而不是一次性编写包含大量推测的手册。

## 第一阶段：基础建设

- [x] 建立多语言项目介绍。
- [x] 支持 15 种语言入口。
- [x] 添加英文与简体中文完整技术架构指南。
- [x] 为全部支持语言添加精简技术指南。
- [x] 定义来源、贡献与翻译规范。

## 第二阶段：可复现插件开发

- [ ] 添加遵循生命周期安全的最小插件示例。
- [ ] 添加带配置校验的类型化工具插件。
- [ ] 添加 Bundle、Profile 与 Patch 打包示例。
- [ ] 添加依赖缺失、Provider 替换、配置更新与卸载测试。
- [ ] 添加 `--dump-config` 排障实践。

## 第三阶段：Runtime 与 Session 深入说明

- [ ] 用可运行示例解释 Context 继承与隔离 Service Realm。
- [ ] 演示 Effect 清理、顺序与失败行为。
- [ ] 从输入到工具结果追踪完整 Turn/Step 生命周期。
- [ ] 添加 Session Resume、Fork、Compaction 与投影示例。
- [ ] 添加 Prompt Surface 稳定性与 Provider Cache 实验。

## 第四阶段：安全与质量

- [ ] 发布第三方插件安全审查模板。
- [ ] 添加沙箱与审批边界示例。
- [ ] 为上游 Commit 与 DSH Release 定义兼容性元数据。
- [ ] 自动检查 Markdown、链接、翻译导航与术语一致性。
- [ ] 建立母语翻译审校标签。

## 第五阶段：可复用 Agent Skill

- [ ] `dsh-repository-explorer`
- [ ] `dsh-plugin-scaffold`
- [ ] `dsh-tool-builder`
- [ ] `dsh-plugin-review`

每个 Skill 必须包含触发条件、前置要求、有序工作流、安全边界、验证步骤，并链接到对应官方契约。

## 第六阶段：生态

- [ ] 整理社区插件，并记录源码、许可证、版本与安全说明。
- [ ] 添加固定到上游版本的完整插件案例。
- [ ] 记录 Host/Client 与远程 API 扩展模式。
- [ ] 用实测证据比较本地、远程和沙箱 Capability Provider。

本路线图用于表达方向，不构成兼容性承诺。优先级会随 DeepSeek Harness 演进而调整。

