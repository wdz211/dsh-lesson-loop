# 模块清单

本仓是**纯文档仓**，不含任何源码。代码在下面各自的单插件仓里。

| 模块 | 它在链路里的职责 | 仓库 |
|---|---|---|
| `dsh-lessons` | 错题本本体。三段式（采集 → 整理 → 修复）+ 账本 + 审计 + 看板。提供 `capture.mjs` / `judge.mjs` / `consolidate.mjs` / `ledger.mjs` / `repair.mjs` / `dedupe.mjs` / `audit.mjs` / `sweep.mjs`，以及 `lessons_check` 工具与 `/x-lessons/state.json` 数据路由 | 尚未公开（本批新建） |
| `dsh-work-queue` | 工单侧。把 `work_report` 里 `failed` / `blocked` 的失败**自动**追加进错题本输入口（`captureToLessons`），并带上 `problemClass` 供跨单聚合。**它自己不写账本** —— 边界见 ARCHITECTURE §4.3 | 尚未公开（本批新建） |
| `dsh-ledger-console` | 双账本只读面板。把工单账本和错题本账本摆在同一屏，顶部按 `problemClass` 现算复现模式，点一侧自动筛出另一侧的相关条目。**一个字节都不写** | [wdz211/dsh-ledger-console](https://github.com/wdz211/dsh-ledger-console) |

## 链接校验

三个链接逐个用 `gh repo view wdz211/<name>` 实际探过：

- `dsh-ledger-console` → 命中，返回真实 URL（见上表）
- `dsh-lessons` → `Could not resolve to a Repository`，故标"尚未公开"
- `dsh-work-queue` → `Could not resolve to a Repository`，故标"尚未公开"

前两个是这一批整理出来的东西，仓库还没建/还没 push，所以链接先不给死地址——
写一个点不开的链接比写"尚未公开"更糟。等建成后把上表第二列换成
`https://github.com/wdz211/dsh-lessons` 与 `https://github.com/wdz211/dsh-work-queue` 即可。

## 依赖方向

```
dsh-ledger-console ──(只读)──► dsh-work-queue 的工单目录
                   ──(只读)──► dsh-lessons 的账本
dsh-work-queue ──(append 一行)──► dsh-lessons 的输入口
```

没有反向依赖：错题本不知道工单存在，工单不 import 错题本（`anchorOf` 的规则是**故意重复实现**的，
好让两边都能独立安装）。这条"箭头不回头"是整条链路能各自单独发布的原因。
