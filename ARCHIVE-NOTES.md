# dsh-purge 本机完整快照 / Local Full Snapshot

> 归档说明。本仓库是 **dsh-purge 1.1.71 在本机上的完整工作副本**（安装态 + 运行态 + 本地修补痕迹），用于留档、复现与二次备份。上游项目与作者信息见下方「来源」。

## 来源 / Provenance

| 项 | 值 |
|---|---|
| 插件名 | `dsh-purge` |
| 版本 | **1.1.71**（`package.json`） |
| 许可证 | MIT |
| 上游仓库 | https://github.com/YuJunZhiXue/dsh-purge |
| 上游许可要求 | 借用名称、思路、代码或提示词必须署名，并写明来源仓库 `YuJunZhiXue/dsh-purge` |
| 本快照来源路径 | `C:\Users\Administrator\.dsh\profiles\desktop\node_modules\dsh-purge` |
| 安装方式 | pnpm `file:` 依赖，源自 `dsh-purge-1.1.71.tar.gz`（原 tarball 已不在桌面，仅 pnpm-lock 留有 integrity 记录） |
| 快照时间 | 2026-11-17（本机时区） |
| 宿主 | DeepSeek Harness desktop profile（`DSH_PROFILE=desktop`） |

## 目录结构 / Layout

```
dsh-purge/
├── bin/                  CLI 入口（dsh-purge.js）
├── lib/                  核心实现（54 个文件，含补丁表、HTTP 路由、更新器）
├── presets/              预设：redteam / liangshen 等 agent 平面组合
├── skills/               23 个技能定义
├── scripts/              构建与客户端注入脚本
├── docs/                 文档与图（11 个文件）
├── .github/              workflow
├── client.js             面板客户端 bundle
├── cordis.patch.yml      插件声明
├── README.md / README.zh-CN.md
├── release-notes.md      版本历史（含全部中英文变更记录）
└── runtime-state/        ← 本机运行态快照（见下）
```

## 本地修补痕迹 / Local Patch Artifacts

本机在 1.1.71 上做过三处缺口修复，**下列 `.bak` / `.bak2` 文件是修补前原件**，一并归档以便对比与回滚：

| 文件 | 说明 |
|---|---|
| `lib/core.js.fix-20261117-001407.bak` | `lib/core.js` 修补前原件（#28 liangshen 候选路径 + markersAny） |
| `client.js.renderfull.bak` | 客户端 bundle 改「全量平铺」渲染前原件 |
| `lib/index.js.renderfull.bak` / `.bak2` | 服务端渲染改全量平铺的两个阶段原件 |

修补的完整根因与验证证据见 `runtime-state/repair-2026-11-17.md`。要点：

- **#80 MINIMAL_OFFICIAL_TOOLSET** 由 `skipped` → `applied`：给 shipped preset 顶部补 `# [dsh-purge] minimal official toolset` marker。
- **#76 PRESET_UNRESTRICTED_IDENTITY_STRIP** 由 `na` → `applied`：新建 `$DSH_HOME/.agent-presets/unrestricted/`（`agent.cordis.yml` + `preset.yml`）作为真实锚点。
- **#28 LIANGSHEN_PHASE1_KEEP_INJECT** 由 `na` → `applied`：新版 liangshen 把 phase-1 白名单合并进 `minimal-prompt.mjs`，候选路径与 `markersAny` 相应扩展。
- 修补后全量补丁计数：**82 / 82 applied**（修补前 79 applied / 2 na / 1 skipped）。

## 运行态快照 / Runtime State

`runtime-state/` 是本机 `$DSH_HOME\dsh-purge\` 的原始副本：

| 文件 | 说明 |
|---|---|
| `applied.json` | 已应用补丁清单 |
| `installed-rev` | 已安装修订号 |
| `drill-grant.json` | 演练台授权状态 |
| `last-cwd.txt` | 上次工作目录 |
| `repair-2026-11-17.md` | 缺口修复记录（根因 / 动作 / 验证） |

## 注意 / Notes

- 本仓库是**本机快照**，不是上游仓库的 fork 分支；上游代码版权归原作者，按 MIT 与署名要求保留 `LICENSE` 与 README 原文未改动。
- `runtime-state/` 中的授权与路径信息属于本机状态，换机使用时不要直接覆盖。
- 被上游 `.gitignore` 排除的提示词原文 `lib/default-prompt-inject.md` 若在本机存在，本仓库以例外规则强制纳入，因为它是注入内容的实际来源。
