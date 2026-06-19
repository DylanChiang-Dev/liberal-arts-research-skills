# 博雅 skill 庫：分層參考料規矩 + RPWS 打包模式對照

> 設計日期：2026-06-19
> 緣起：評估 `Master-cai/Research-Paper-Writing-Skills`（RPWS，~4k star）對博雅倉庫的借鑑意義。只讀結構、不轉述內容。
> 範圍：本 spec 只定**博雅 skill 庫的結構公約**。隨書書稿的對應修訂屬下游另一輪，本輪僅登記。

## 背景

- 博雅現狀（`CONVENTIONS.md` §1）：一個 skill 一個目錄 `skills/<name>/SKILL.md`，**單檔**；參考料走 repo 級共享庫 `knowledge/`（參考卡）、`templates/`（骨架）、`examples/`（實錄）、`evals/`（回歸）。
- 13 個 SKILL.md 目前 78–124 行，**無臃腫**。
- RPWS 是**單一大 skill**，把量丟進該 skill 自帶的 `references/`（含 nested `examples/`），SKILL.md 僅 99 行作路由。
- 兩邊 SKILL.md 都瘦，差別在「參考料放共享層，還是放 skill 自帶層」。

## 決策一：分層參考料規矩（採混合 C）

當一塊參考料超出 SKILL.md 該怎麼放，按三層收納 + 一條軟警戒線：

1. **預設單檔**。`SKILL.md` 只放現有 §3 四段：角色／鐵律／工作流／輸出格式。常態，13 個既有 skill 不動。
2. **跨 skill 複用的料 → repo 級共享庫**（不變）：參考卡進 `knowledge/`；可填空骨架進 `templates/`。
3. **該 skill 獨佔、又塞不進四段的長料 → `skills/<name>/references/`**（新增的合規出路）。

**觸發判斷（語意為主，行數為輔）：**

- 主判據：是「長範例／長對照表／長清單」，**且**只服務這一個 skill、不被別的 skill 引用。
- 軟警戒線：SKILL.md **> 200 行**時，強制檢查一次是否該往 `references/` 拆。未超線不必拆。行數是提醒，非鐵律。

**決策樹（寫進公約）：**

```
要新增一塊參考料 →
  能塞進 SKILL.md 四段且不臃腫？ → 留在 SKILL.md
  否則 → 會被別的 skill 用到嗎？
            會 → knowledge/ 或 templates/（共享）
            不會 → skills/<name>/references/（獨佔）
```

**邊界紅線**：`references/` 只收獨佔料；任何可能複用的料一律進共享庫，避免兩套機制打架。

**讀者可見性**：此規矩是**讀者不可見的純內部維護決定**。書正文只教「把判斷編碼成 skill」概念、按名字叫 skill，從不描述目錄結構。故採 C 不增加書的講解負擔、不逼正文改字。

## 決策二：博雅 vs RPWS 打包模式對照

只比結構/打包，不涉內容。取捨依「書的脊樑／讀者不可見／書耦合點」三分，**不以「書已這樣」當理由**（書本來就會回頭改）。

| 維度 | 博雅 | RPWS | 取捨類別 | 處置 |
|---|---|---|---|---|
| skill 數量 | 13 個獨立，一章一技能 | 1 個大 skill | 書的脊樑 | 維持多 skill |
| 參考料位置 | repo 級共享 `knowledge/`／`templates/` | 該 skill 自帶 `references/` | 讀者不可見 | 共享為主，獨佔料才下放 `references/`（決策一） |
| SKILL.md 角色 | 角色＋鐵律＋工作流＋輸出格式 | 工作流＋路由 | 讀者不可見 | 不變，新增 200 行軟警戒線 |
| SKILL.md 體量 | 78–124 行 | 99 行 | — | 兩邊都瘦，思路一致 |
| 範例/實錄 | repo 級 `examples/`，升版前必出真錄 | `references/examples/` 範文模板 | 書的脊樑（誠信差異化） | 保留真錄硬規 |
| 回歸驗證 | `evals/` + `VERIFICATION.md` | 無 | 書的脊樑 | 維持，不向 RPWS 看齊 |
| 多 AI 對齊 | `.claude-plugin` + `.codex-plugin` + AGENTS.md | SKILL.md + `agents/openai.yaml` | 讀者不可見 | 已具備；可參考其 per-platform metadata 寫法 |
| 版本策略 | 倉庫級 tag，三段式 | 無版本欄位 | 書的脊樑 | 維持三段式 |
| 語言 | 繁中優先，多語 README | 英＋中雙 README | 書的脊樑 | 維持繁中優先 |
| 授權/致謝 | MIT，誠信硬規 | MIT，重致謝原作者 | — | 一致 |

**一句話結論**：RPWS 唯一值得吸收的是「SKILL.md 瘦身、長料下沉 `references/`」這個動作；其餘（單 skill、無 evals、無版本、無真錄硬規）博雅都已有更嚴做法，不向它看齊。

## 決策三：書耦合點登記（本輪不改書）

唯一會牽動「回頭改書」的耦合：**附錄 A 指令速查 / 附錄 B 提示語庫 ↔ skill 庫的 `templates/`／`knowledge/`** 內容重複，會各改各的漂移。

定**單一真相源 = skill 庫**：

- 附錄 B（提示語庫）改為「節錄 ＋ QR 指向 skill 庫」，再版只換庫、書不動 —— 符合 PROJECT.md 抗過時三層（正文原理／附錄操作／線上更新頁）。
- 此項列入下游「回頭改書」清單，**本 spec 不執行書稿修改**。

## 實作範圍（待 writing-plans 細化）

1. 改 `CONVENTIONS.md`：在 §1 後補一節「分層參考料」，含三層收納、觸發判據、決策樹、邊界紅線（決策一）。
2. 把本對照表（決策二）落為 repo 內一份參考文件或併入 `CONVENTIONS.md` 附註。
3. **不改任何既有 SKILL.md**（13 個皆未超線、未臃腫，無需拆）。
4. 書稿修訂（決策三）不在本輪範圍。

## 非目標

- 不重構既有 skill、不拆分、不改名、不改 skill 數量。
- 不轉述 RPWS 任何文字內容。
- 不在本輪改動任何書稿檔案。
