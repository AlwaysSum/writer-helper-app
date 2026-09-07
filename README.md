# Writer Helper · 必火写作助手

> 跨平台网文写作助手 —— 一款面向网络小说作者的**作品管理与 AI 生成一体化**工具。

本仓库是 **Writer Helper App 的官方发布仓库**：包含产品介绍与 GitHub Actions 持续构建配置，**自动从 GitCode 源码仓库**（[Sunflower816/writer_helper_app](https://gitcode.com/Sunflower816/writer_helper_app)）拉取最新代码，构建 **Windows / macOS / Linux / Android** 安装包，并发布到本仓库的 [Releases](https://github.com/AlwaysSum/writer-helper-app/releases) 页面供下载。

---

## ✨ 产品简介

Writer Helper（必火写作）基于 Flutter（Material 3）实现，配合 AppFlowy 富文本编辑器与内置 AI 智能体，覆盖网文创作从「构思 → 码字 → 润色 → 测试」的完整链路。

### 核心能力

- **作品管理**：章纲、大纲、设定集、伏笔线索、备忘录，创作要素统一沉淀。
- **富文本编辑器**：内置 AppFlowy 编辑器，专注码字体验，支持自动保存、查找替换。
- **AI 智能体对话**：AiAgent 对话式创作辅助，支持章节生成、剧情推演、润色与续写。
- **可视化生成工作流**：节点化编排 AI 生成流程，实现「上一章 → 推演 → 生成正文 → 自动审查」。
- **时间线网络**：以时间线组织章节与设定，把握剧情脉络。
- **向量语义检索**：跨作品素材、设定、正文的语义级检索。
- **辅助工具**：AI 书名测试、拆书仿写、作品章节测试、作品分享广场。

### 支持平台

| 平台 | 产物 |
| --- | --- |
| Windows | 安装包 `WriterHelperSetup-<版本>.exe`（Inno Setup） |
| macOS | `.app` 打包的 `writer-helper-macos-<版本>.zip` |
| Linux | 免安装 `writer-helper-linux-<版本>.tar.gz`（解压即用） |
| Android | APK `writer-helper-android-<版本>.apk` |

> 表格中各安装包均由下方 GitHub Actions 在本仓库构建生成。

---

## 🚀 构建产物下载

打开本仓库 **Releases** 页，选择目标版本即可下载对应平台的安装包：

- [前往 Releases](https://github.com/AlwaysSum/writer-helper-app/releases)
- 每个 Release 附带 **SHA256 校验和**，下载后请核验文件完整性。

---

## ⚙️ 自动构建（GitHub Actions）

本仓库通过 [build.yml](.github/workflows/build.yml) 实现「源码在 GitCode、构建与发布在 GitHub」的流水线：

```
GitCode 源码仓库 ──git clone──▶ GitHub Actions Runner
                                   ├─ Windows  job：flutter build windows + Inno Setup
                                   ├─ macOS    job：flutter build macos --release（关闭强签名）
                                   ├─ Linux    job：flutter create linux + build linux
                                   └─ Android  job：flutter build apk --release
                                        │
                                        ▼
                             上传 Actions 产物 → 发布 GitHub Release（安装包附件）
```

### 触发方式

1. **手动触发（推荐）**：仓库 `Actions` 页 → `build-release` → **Run workflow**，
   - 可指定 `gitcode_url` / `gitcode_branch`；
   - 勾选 *publish* 后构建完成会自动发布 GitHub Release。
2. **打 tag 触发**：向本仓库推送形如 `v*` 的 tag（如 `v1.1.1`）即可自动构建并发布 Release。

### 关键说明 / 注意事项

- **Linux 目录**：源码仓库当前没有 `linux/` 平台目录，workflow 在构建前自动执行
  `flutter create --platforms=linux` 生成后再编译。
- **Android 签名**：当前 `release` 构建使用 debug 签名（源码 `android/app/build.gradle.kts` 默认配置），
  可直接安装验证；如需上架应用商店，请在源码侧配置正式签名后再触发构建。
- **macOS 签名**：CI 默认不进行公证/签名（`CODE_SIGNING_ALLOWED=NO`），生成的 `.app`
  仅适用于本地测试；对外分发建议接入 Apple 开发者证书与 notarization。
- **原生依赖**：Windows 依赖 Inno Setup（workflow 自动安装）；Linux 依赖 gtk3/clang 等（已自动安装）。
  media_kit 的 mpv/ANGLE 资源已随源码仓库提交，构建时无需联网下载。

---

## 🛠 技术栈

Flutter · Dart · Riverpod · go_router · hive_ce · dio · AppFlowy Editor · dart_agent_core

## 📄 许可

本仓库用于发布构建产物；应用源码遵循其源码仓库的许可协议。