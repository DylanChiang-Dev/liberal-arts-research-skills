# 博雅分層參考料規矩 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 在博雅 repo 的 `CONVENTIONS.md` 補一節「分層參考料」規矩，並把 RPWS 打包模式對照以指針接到已 commit 的 spec。

**Architecture:** 純文檔變更。`CONVENTIONS.md` 是操作公約（怎麼寫 skill），新增 §8 承載三層收納＋決策樹＋軟警戒線；RPWS 對照表已存在於 spec 內，CONVENTIONS.md 只以一行指針引用，避免兩處重抄（DRY）。不改任何既有 SKILL.md、不動書稿。

**Tech Stack:** Markdown。驗證靠人讀一致性 + 連結解析檢查，無程式測試。

---

## File Structure

- Modify: `CONVENTIONS.md` — 在現有 §7 後新增 §8「分層參考料」；§8 末尾加一行指針指向 spec。
- Reference（已存在，不改）：`docs/superpowers/specs/2026-06-19-skill-reference-layering-design.md` — 對照表與決策依據的正本。

唯一改動檔是 `CONVENTIONS.md`。對照表不複製進公約，只留指針，確保單一真相源。

---

### Task 1: CONVENTIONS.md 新增 §8 分層參考料

**Files:**
- Modify: `CONVENTIONS.md`（在 §7 之後、檔尾新增 §8）

- [ ] **Step 1: 確認當前 §7 是最後一節**

Run: `tail -8 CONVENTIONS.md`
Expected: 最後一節為 `## 7. 知識庫／模板／驗證日誌維護`，其下三條 bullet，檔案到此結束。

- [ ] **Step 2: 在檔尾追加 §8 全文**

在 `CONVENTIONS.md` 末尾追加以下內容（一字不改）：

```markdown

## 8. 分層參考料

當一塊參考料超出 SKILL.md 該怎麼放，按三層收納 ＋ 一條軟警戒線。此規矩是**讀者不可見的內部維護決定**：書正文只教概念、按名字叫 skill，不描述目錄結構，故本節不影響書稿。

1. **預設單檔**。`SKILL.md` 只放 §3 四段（角色／鐵律／工作流／輸出格式）。常態，既有 skill 不動。
2. **跨 skill 複用的料 → repo 級共享庫**（同 §7）：參考卡進 `knowledge/`，可填空骨架進 `templates/`。
3. **該 skill 獨佔、又塞不進四段的長料 → `skills/<name>/references/`**。

**觸發判斷（語意為主、行數為輔）：**

- 主判據：是「長範例／長對照表／長清單」，**且**只服務這一個 skill、不被別的 skill 引用。
- 軟警戒線：SKILL.md **> 200 行**時強制檢查一次是否該拆；未超線不必拆。行數是提醒，非鐵律。

**決策樹：**

```
要新增一塊參考料 →
  能塞進 SKILL.md 四段且不臃腫？ → 留在 SKILL.md
  否則 → 會被別的 skill 用到嗎？
            會 → knowledge/ 或 templates/（共享）
            不會 → skills/<name>/references/（獨佔）
```

**邊界紅線**：`references/` 只收獨佔料；任何可能複用的料一律進共享庫，避免兩套機制打架。

> RPWS（`Master-cai/Research-Paper-Writing-Skills`）打包模式對照與取捨依據，見 [docs/superpowers/specs/2026-06-19-skill-reference-layering-design.md](docs/superpowers/specs/2026-06-19-skill-reference-layering-design.md)。
```

- [ ] **Step 3: 驗證 §8 已落位且結構完整**

Run: `grep -n "^## 8. 分層參考料" CONVENTIONS.md && grep -c "skills/<name>/references/" CONVENTIONS.md`
Expected: 印出 §8 標題行號；`references/` 出現次數 ≥ 2（規則 3 與邊界紅線各一）。

- [ ] **Step 4: 驗證指向 spec 的連結路徑存在**

Run: `test -f docs/superpowers/specs/2026-06-19-skill-reference-layering-design.md && echo OK`
Expected: `OK`（連結目標檔存在，相對路徑正確）。

- [ ] **Step 5: 人讀一致性檢查**

讀 §8 全文，確認：①與 §1「單檔」、§7「共享庫」不矛盾（§8 是其延伸，非推翻）；②決策樹的三個分支與三層收納一一對應；③無 placeholder（TBD/TODO）。

- [ ] **Step 6: Commit**

```bash
git add CONVENTIONS.md
git commit -m "docs: CONVENTIONS 補 §8 分層參考料規矩（單檔／共享庫／per-skill references）"
```

---

### Task 2: 收尾驗證（不改既有 SKILL.md、不動書稿）

**Files:**
- 只讀驗證，無修改。

- [ ] **Step 1: 確認 13 個 SKILL.md 皆未被本輪改動**

Run: `git status --porcelain skills/`
Expected: 空輸出（skills/ 下無任何變更）。

- [ ] **Step 2: 確認所有 SKILL.md 仍在 200 行軟警戒線內（佐證本輪無需拆 references/）**

Run: `wc -l skills/*/SKILL.md | sort -n | tail -3`
Expected: 最大者 ≤ 200 行（目前約 124 行），印證決策一「既有 skill 不動」的前提成立。

- [ ] **Step 3: 確認書稿 repo 未被觸碰**

說明：書稿在另一個專案目錄（`/Volumes/001/01.projects/2026/20260612-第一本書-AI研究工作流--active`），本計畫僅在博雅 repo 內操作，天然不涉書稿。無需指令，確認本輪所有 commit 皆落在博雅 repo 即可。

- [ ] **Step 4: 最終確認 commit 歷史**

Run: `git log --oneline -3`
Expected: 最新兩筆為本輪的 spec commit（`8072249`）與 §8 commit；無其他越界改動。

---

## Self-Review

**Spec coverage：**
- 決策一（分層規矩）→ Task 1 Step 2 §8 全文落地。✓
- 決策二（RPWS 對照表）→ spec 已含正本，Task 1 Step 2 以指針引用，DRY。✓
- 決策三（書耦合登記，本輪不改書）→ spec 已登記；Task 2 Step 3 確認不觸書稿。✓
- 實作範圍「不改既有 SKILL.md」→ Task 2 Step 1–2 驗證。✓

**Placeholder scan：** 無 TBD/TODO；§8 全文逐字給出，指針路徑具體。✓

**一致性：** §8 引用的 `knowledge/`／`templates/`／`references/`／`SKILL.md` 命名與 §1、§7 及 spec 完全一致；200 行軟警戒線數值在 spec、§8、Task 2 Step 2 三處一致。✓
