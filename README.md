# DNF 86 / S4A 开源项目聚合构建

[![Daily build](https://github.com/zmbksyyr/86/actions/workflows/daily-build.yml/badge.svg)](https://github.com/zmbksyyr/86/actions/workflows/daily-build.yml)
[![Latest release](https://img.shields.io/github/v/release/zmbksyyr/86?display_name=release&label=release)](https://github.com/zmbksyyr/86/releases/tag/daily-build)

本仓库是 DNF 86 版本相关开源项目的**源码镜像与自动构建仓库**。它将分散在 GitGud 的 A12、A21 服务端、客户端补丁和 GM 工具统一同步到一个 GitHub 仓库，并通过 GitHub Actions 定时生成可下载的 Windows 构建包。

本仓库不替代上游项目，也不改变上游项目的目标框架、依赖版本和业务代码。功能实现、问题反馈和开发讨论应优先参考对应上游仓库。

## 项目目的

- 在 GitHub 中保存六个上游项目的最新源码快照，便于统一浏览、检索和备份。
- 自动完成 .NET 服务端、辅助工具和 Visual C++ 客户端补丁的 Release 构建。
- 将不同项目的产物分开压缩，避免同名文件覆盖或版本混淆。
- 提供固定下载入口，减少手动拉取源码、配置环境和逐项编译的工作。

## 自动化效果

[Daily build](.github/workflows/daily-build.yml) 每天北京时间 **06:00** 自动运行，也可以在 GitHub Actions 页面手动触发。每次运行依次执行：

1. 从六个上游仓库浅克隆最新默认分支。
2. 将源码镜像到本仓库 `main` 分支根目录，并自动提交有变化的文件。
3. 按上游项目原始配置安装 .NET SDK、还原依赖并构建 Windows x64 应用。
4. 使用 Visual Studio 2022 / MSVC v143 构建 Win32 客户端补丁。
5. 按项目分别压缩产物，并更新固定的 [`daily-build`](https://github.com/zmbksyyr/86/releases/tag/daily-build) Release。

Release 始终只使用一个 `daily-build` 标签。新构建会清理旧附件并上传本次产物，不会每天创建新标签。压缩包名称包含北京时间秒级时间戳，例如：

```text
86JP-2026-09-07-004341.zip
86JPGMTool-2026-09-07-004341.zip
S4A12ClientPatch-2026-09-07-004341.zip
ServerS4A21-2026-09-07-004341.zip
S4A21ClientPatch-2026-09-07-004341.zip
S4A21GmTool-2026-09-07-004341.zip
```

## 项目来源

| 本仓库目录 | 上游仓库 | 内容 | 发布包 |
| --- | --- | --- | --- |
| [`86JP/`](86JP/) | [rewio/86JP](https://gitgud.io/rewio/86JP) | A12/86JP 服务端、PvfProxy 和客户端补丁 | `86JP-*.zip` |
| [`86JPGMTool/`](86JPGMTool/) | [rewio/86JPGMTool](https://gitgud.io/rewio/86JPGMTool) | A12 Web GM 工具 | `86JPGMTool-*.zip` |
| [`S4A12ClientPatch/`](S4A12ClientPatch/) | [rewio/S4A12ClientPatch](https://gitgud.io/rewio/S4A12ClientPatch) | A12 客户端补丁组件 | `S4A12ClientPatch-*.zip` |
| [`ServerS4A21/`](ServerS4A21/) | [rewio/ServerS4A21](https://gitgud.io/rewio/ServerS4A21) | A21 服务端和 PvfProxy | `ServerS4A21-*.zip` |
| [`S4A21ClientPatch/`](S4A21ClientPatch/) | [rewio/S4A21ClientPatch](https://gitgud.io/rewio/S4A21ClientPatch) | A21 客户端补丁及扩展 | `S4A21ClientPatch-*.zip` |
| [`S4A21GmTool/`](S4A21GmTool/) | [rewio/S4A21GmTool](https://gitgud.io/rewio/S4A21GmTool) | A21 Web GM 工具 | `S4A21GmTool-*.zip` |

同步过程会排除每个上游仓库自身的 `.git` 目录，因此这里保存的是源码快照，不包含上游的完整 Git 历史。每次同步产生的聚合提交可以用来查看相邻快照之间的变化。

## 发布包说明

- `86JP`：包含 A12 服务端、PvfProxy 和对应客户端补丁。
- `ServerS4A21`：包含 A21 服务端和 PvfProxy。
- `86JPGMTool`、`S4A21GmTool`：分别对应 A12、A21 的 Web GM 工具。
- `S4A12ClientPatch`、`S4A21ClientPatch`：包含对应版本的客户端补丁 DLL 和随项目提供的配置文件。
- .NET 应用按 `win-x64` 自包含方式发布；客户端补丁按 Win32 Release 配置构建。

发布包仅包含上游仓库能够直接构建的内容，**不包含游戏客户端、`Script.pvf`、用户数据库、账号数据或其他受限制资源**。服务端及工具运行前仍需按照各子项目 README 准备必要的数据文件和配置。

## 使用方式

1. 打开 [Releases](https://github.com/zmbksyyr/86/releases/tag/daily-build)。
2. 根据需要下载对应项目的 ZIP，不需要下载其他项目的产物。
3. 解压后阅读压缩包对应源码目录中的 README，按项目要求放置配置和数据文件。

需要检查源码时，可以直接浏览本仓库根目录下的六个项目目录。上游有更新后，定时任务会自动同步；也可以在 [Actions](https://github.com/zmbksyyr/86/actions/workflows/daily-build.yml) 页面手动运行。

## 来源、许可与免责声明

- 六个子项目的代码均来源于上表列出的 GitGud 上游仓库，著作权归各自作者和贡献者所有。
- 各目录继续适用其上游许可证和声明；本聚合仓库不会通过同步或构建行为改变原有许可条件。
- 本项目用于开源代码保存、研究、学习和自动化构建，不提供官方游戏服务，也不隶属于游戏开发商或发行商。
- 自动构建成功只表示源码在对应 CI 环境中完成编译，不代表所有功能均经过实际客户端、网络环境或生产数据验证。
- 使用服务端、GM 工具或客户端补丁前，请自行确认所在地法律法规、软件许可和数据安全要求，并提前备份重要数据。
