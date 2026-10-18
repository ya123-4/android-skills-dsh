# android-skills-dsh（中文说明）

Google 官方 [android/skills](https://github.com/android/skills) v1.0.15 的 DeepSeek Harness skill 插件包。

把上游 `.claude-plugin/plugin.json` 声明的全部 30 个 skill（含全部支持文件）逐字节打包为
可由模型调用的 DSH skill。纯 skill 包：不含 hooks、不含原生工具、不含安装脚本，也不依赖
Android CLI。

[English](README.md)

## 安装

```powershell
# 在 DSH profile 里安装；升级已安装的包需要 DSH 完全退出，首次安装可直接用插件管理器
plugin_manager install_bundle <路径>\dsh-plugin-build\dist\android-skills-dsh-1.0.15.tgz
```

包内自带一个 filesystem skill provider，根目录就是本包的 `skills/`，无需其它配置。

## 与上游同步

本包由 `skill-sync` 管理（registry 条目 `android-skills-dsh`，`kind: github-tags`，
`tagPrefix: v`）。包版本跟随上游 tag 线，`check` 拿 `1.0.15` 与最新 `vX.Y.Z` 比较：

```powershell
python dsh-plugin-build\skill-sync-dsh\skills\skill-sync\scripts\skill_sync.py check android-skills-dsh
python dsh-plugin-build\skill-sync-dsh\skills\skill-sync\scripts\skill_sync.py update android-skills-dsh
```

`update` 会下载上游 tag 的 tarball、按 plugin.json 的清单重新打平、从
`recipes/overlays/descriptions-zh.json` 重新写中文 description、校验后以带回滚的事务落地，
最后打包 `dist/android-skills-dsh-<version>.tgz`。若上游 skill 集合发生变化，命令以 exit 4
报告，需要人工确认后加 `--accept-structure-change`。

## 包含的 30 个 skill

| 领域 | Skill | 用途 |
|---|---|---|
| 构建 | `agp-9-upgrade` | 升级或迁移项目到 Android Gradle Plugin 9 |
| 相机 | `camerax` | CameraX 功能、异步录制生命周期、与 ML Kit/Media3 配合 |
| 车机 | `android-for-cars-parked-apps` | 在 Android Auto / Automotive OS 上做驻车应用 |
| 端侧 AI | `appfunctions` | 把应用工作流暴露为 AppFunctions 供端侧 agent 调用 |
| 端侧 AI | `litert-runtime` | 用 CompiledModel 在 CPU/GPU 上跑 LiteRT `.tflite` 推理 |
| 端侧 AI | `ml-kit-genai-prompt-api` | Gemini Nano 上的 ML Kit GenAI Prompt API |
| 工具 | `android-cli` | 安装并使用 `android` CLI |
| 身份 | `restore-credentials` | 用 `androidx.credentials` 实现 Restore Credentials |
| 身份 | `verified-email` | 通过 Credential Manager 获取已验证邮箱 |
| Compose | `adaptive` | 按窗口尺寸与输入设备适配 UI（MediaQuery、Grid、FlexBox） |
| Compose | `jetpack-compose-performance` | 诊断并优化 Compose 重组开销 |
| Compose | `migrate-xml-views-to-jetpack-compose` | XML View 迁移到 Compose 的完整流程 |
| Compose | `styles` | 接入 Compose Styles API 与主题体系 |
| 媒体 | `media3-cast-integration` | 用 Jetpack Media3 接入 Google Cast |
| 导航 | `navigation-3` | Jetpack Navigation 3：scene、深链、多返回栈 |
| 导航 | `navigation-event` | 用 NavigationEvent 库做预测性返回 |
| 性能 | `r8-analyzer` | 分析 R8 keep 规则与构建配置中的冗余 |
| Play | `engage-sdk-integration` | 接入与排查 Play Engage SDK |
| Play | `play-billing-library-version-upgrade` | 升级 Play Billing Library 版本 |
| Play | `play-policy-insights` | Play 政策合规审计（静态分析 + 商店声明） |
| 性能剖析 | `android-profiler` | 录制/分析 Perfetto trace、卡顿、ANR、启动耗时 |
| 安全 | `android-intent-security` | Intent 重定向与组件暴露审计 |
| 安全 | `android-permissions-security` | 权限与 IPC 组件安全审计 |
| 系统 | `edge-to-edge` | 迁移到自适应 edge-to-edge 并修 inset 问题 |
| 系统 | `glance-canonical-layouts` | 用 Glance Canonical Layouts 做桌面小组件 |
| 测试 | `testing-setup` | 测试策略、测试库与测试脚手架搭建 |
| TV | `leanback-to-compose-tv-migration` | Leanback/Views 迁移到 Compose for TV |
| Wear | `wear-compose-m3` | Wear OS Compose Material 3 应用与迁移 |
| Wear | `wear-widgets` | 用 Glance Wear / Remote Compose 做 Wear OS 小组件 |
| XR | `display-glasses-with-jetpack-compose-glimmer` | 用 Compose Glimmer 开发 Android XR 显示眼镜应用 |

## DeepSeek Harness 适配说明

- **只做打平。** 上游 skill 嵌套两到三层，DSH 只发现一层 `<root>/<name>/SKILL.md`，
  所以每个 skill 打平到 `skills/<name>/`；名字与正文都不改。
- **零正文改写。** 与 mattpocock 包不同，上游正文没有 Claude Code 工具名和斜杠命令，
  因此不需要规范化。
- **中文描述。** 每个 skill 的中文 `description` 来自 skill-sync 的
  `descriptions-zh.json`，每次同步都会重新写入；缺条目时保留上游英文并在 stderr 提示。
- **不用上游的安装入口。** `android skills add`（Android CLI）和 Claude Code / Codex
  插件市场面向其他 harness；DSH 直接加载本包。项目开发时仍建议装上游 `android` CLI，
  用法见 `android-cli` skill。

## 许可

skill 内容：Apache License 2.0，Copyright Google LLC — 见 [LICENSE](LICENSE) 与
[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。
