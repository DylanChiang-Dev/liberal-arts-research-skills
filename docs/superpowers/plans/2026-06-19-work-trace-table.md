# 工作痕跡表（work-trace-table）Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 為 Boya 建立一份統一的「工作痕跡表」格式（單一真相源），並讓三個誠信 skill（citation-verify／self-review／ai-disclosure）在工作當下把痕跡填／讀進這張表，把「AI 做苦工、你做判斷」變成可交付的證據。

**Architecture:** 甲＋丙。甲＝在 `CONVENTIONS.md` 定義五個標記與用法、在 `templates/` 出一張空白範本表（單一真相源）。丙＝在三個既有 skill 的 `SKILL.md` 各加一個輸出／讀取步驟，引用 `CONVENTIONS.md` 的格式，不新增 skill、不碰其餘 10 個 skill 的內臟、不做事後重建。

**Tech Stack:** 純 Markdown（skill／template／convention／eval 文件）。無程式碼、無建置鏈。驗證走 Boya 既有機制：`evals/<skill>.md` 回歸斷言 ＋ `examples/` 實跑真錄 ＋ `VERIFICATION.md` 證據帳。

**重要約束：**
- **本計畫不執行 git commit**（依使用者指示）。原模板的「Commit」步驟一律替換為「停下檢查點」。
- **繁體中文優先**，與全庫一致。
- **單一真相源紀律**：五個標記只在 `CONVENTIONS.md` 定義一次；template 與三個 skill 一律「引用」不「重抄」定義。
- **不違背既有鐵律**：citation-verify 的「查無≠偽造」必須保住——該 skill 永遠不在痕跡表寫入 🗑 或「假」，查無一律標 ❓；🗑（捨棄）只由作者在確認後自行標記。

---

## 標記詞彙（本計畫共用，實作時以 CONVENTIONS.md 為準）

| 標記 | 意思 |
|---|---|
| 🧍 你判斷 | 使用者自己想的、自己決定的內容（論點、選擇、詮釋）。 |
| 🤖 AI待核 | AI 幫忙整理／起草的內容，使用者尚未核實。核實採納後改標 🧍。 |
| ✅ 已查證 | 引用或事實已驗證為真（例：DOI／來源對得上）。 |
| ❓ 待查 | 尚未查證，或查無、存疑（不確定真偽）。 |
| 🗑 捨棄 | 試過又放棄的方向，**必須在「來源／原因」欄寫放棄原因**。 |

---

## File Structure（先鎖定，再拆任務）

- `CONVENTIONS.md`（修改）— 新增「§8 工作痕跡表」小節：五個標記定義＋用法＋邊界。**全庫唯一定義處**。
- `templates/work-trace-table.md`（新建）— 空白範本表，含表頭、標記圖例、1～2 列示意。使用者複製到自己論文資料夾用。
- `skills/citation-verify/SKILL.md`（修改）— 工作流加一輸出步驟：查核後追加痕跡列（含標記映射）。
- `skills/self-review/SKILL.md`（修改）— 工作流加一輸出步驟：AI 建議標 🤖待核、採納後轉 🧍。
- `skills/ai-disclosure/SKILL.md`（修改）— 第 1／4 步改為「讀工作痕跡表作為證據底稿」生成揭露。
- `evals/citation-verify.md`、`evals/self-review.md`、`evals/ai-disclosure.md`（修改）— 各加痕跡表回歸斷言。
- `examples/2026-06-19-work-trace-table-case.md`（新建）— 一份實跑真錄，示範痕跡表被三個 skill 填／讀。
- `VERIFICATION.md`（修改）— 補實測列。
- `MEMORY.md`（修改）— 索引補一行指向本功能。

依賴順序：Task 1（定義）→ Task 2（範本）→ Task 3/4/5（三 skill，可平行）→ Task 6（evals）→ Task 7（example 實跑）→ Task 8（文件同步）。

---

### Task 1: 在 CONVENTIONS.md 定義「工作痕跡表」（單一真相源）

**Files:**
- Modify: `CONVENTIONS.md`（在第 7 節「知識庫／模板／驗證日誌維護」之後新增第 8 節）

- [ ] **Step 1: 確認插入點**

Read `CONVENTIONS.md`，定位第 7 節結尾（檔末）。新內容追加在檔案最後。

- [ ] **Step 2: 追加「§8 工作痕跡表」整段**

在 `CONVENTIONS.md` 檔末追加以下內容（完整貼入，不要簡寫）：

```markdown

## 8. 工作痕跡表（work-trace-table）

> 把「AI 做苦工、你做判斷」變成一張看得見、可交付的證據表。標記定義**只在本節**出現一次；template 與各 skill 一律引用本節，不得重抄或自訂另一套標記。

### 8.1 五個標記（固定詞彙）

| 標記 | 意思 |
|---|---|
| 🧍 你判斷 | 使用者自己想的、自己決定的內容（論點、選擇、詮釋）。 |
| 🤖 AI待核 | AI 幫忙整理／起草的內容，使用者尚未核實。核實採納後改標 🧍。 |
| ✅ 已查證 | 引用或事實已驗證為真（例：DOI／來源對得上）。 |
| ❓ 待查 | 尚未查證，或查無、存疑（不確定真偽）。 |
| 🗑 捨棄 | 試過又放棄的方向，必須在「來源／原因」欄寫放棄原因。 |

### 8.2 表格欄位

| 內容（一句話） | 標記 | 來源／原因 | 日期 |
|---|---|---|---|

- 「內容」：一句話講清楚是什麼，不長篇。
- 「來源／原因」：✅ 填查證依據；🗑 填放棄原因；其餘可填一句說明或留空。
- 「日期」：可選，未填以 `—` 表示。

### 8.3 用法與邊界

- 表是**輔助、不強制**；但誠信類 skill（citation-verify／self-review／ai-disclosure）預設會幫忙填／讀。
- 標記只記「**誰做的**」這個事實，**不替使用者判斷對不對**——對不對永遠是使用者的事。
- **查無就標 ❓，絕不偽造 ✅**（守 RULES.md 與誠信鐵律「查無≠偽造」）。
- 🗑（捨棄）代表「作者已確認並放棄」，**只由作者標記**；skill 不得替作者把「查無」直接判成捨棄或造假。
- 立場是透明留痕，與 RULES.md 第 1 條一致；不收錄任何規避偵測用法。
```

- [ ] **Step 3: 自我核對（對照 spec §設計①與④）**

確認：五個標記與 spec 完全一致；含「不替使用者判斷對錯」「查無標 ❓」「🗑 只由作者標」三條邊界；無 TODO／待補。

- [ ] **Step 4: 停下檢查點**

回報：CONVENTIONS.md 已新增 §8，貼出新段落供審。不 commit。

---

### Task 2: 建立空白範本表 templates/work-trace-table.md

**Files:**
- Create: `templates/work-trace-table.md`

- [ ] **Step 1: 寫入範本檔（完整內容）**

建立 `templates/work-trace-table.md`，內容如下（完整貼入）：

```markdown
# 工作痕跡表（work-trace-table）

> 可填空骨架，標記定義以 [CONVENTIONS.md §9](../CONVENTIONS.md) 為準（本檔不重抄定義）。
> 用法：複製本表到你的論文資料夾，每做一件事就追加一列。
> 立場：透明留痕。查無只標 ❓，絕不偽造 ✅；🗑（捨棄）由你自己確認後標、並寫原因。

標記圖例：🧍 你判斷 ｜ 🤖 AI待核 ｜ ✅ 已查證 ｜ ❓ 待查 ｜ 🗑 捨棄

| 內容（一句話） | 標記 | 來源／原因 | 日期 |
|---|---|---|---|
| （示意）晚清翻譯小說反映文化焦慮 | 🧍 你判斷 | 你的論點 | 2026-06-19 |
| （示意）第二章背景段 | 🤖 AI待核 | AI 整理，待你核 | 2026-06-19 |
| （示意）王德威《被壓抑的現代性》 | ✅ 已查證 | DOI 對得上 | 2026-06-19 |
| （示意）某期刊文章 | ❓ 待查 | 還沒驗 | — |
| （示意）中日比較取向 | 🗑 捨棄 | 範圍太大 | 2026-06-18 |
| | | | |
```

- [ ] **Step 2: 自我核對**

確認：範本「引用」CONVENTIONS §9 而非重抄定義（守單一真相源）；示意列標「（示意）」避免被誤當真資料；表尾留一空列供填寫。

- [ ] **Step 3: 停下檢查點**

回報：範本已建立。不 commit。

---

### Task 3: 接 citation-verify（查核後填痕跡表）

**Files:**
- Modify: `skills/citation-verify/SKILL.md`（在「### 第 4 步：輸出查核報告」之後新增「### 第 5 步：寫入工作痕跡表」）

- [ ] **Step 1: 確認插入點**

Read `skills/citation-verify/SKILL.md`，定位第 4 步輸出報告區塊結尾（約 90 行的程式碼框 ` ``` ` 之後、「## 已知陷阱」之前）。新步驟插在此處。

- [ ] **Step 2: 插入「第 5 步」整段**

在第 4 步之後、「## 已知陷阱」之前插入：

```markdown
### 第 5 步：寫入工作痕跡表

若使用者有維護工作痕跡表（見 CONVENTIONS.md §9），查核完成後按其格式，把每筆引用追加為一列（標記定義以 §9 為準，本步不重抄）。狀態映射如下，**嚴守「查無≠偽造」——本 skill 永遠不在痕跡表寫入 🗑 或「假」**：

| 查核結果 | 痕跡表標記 | 來源／原因欄 |
|---|---|---|
| ✅ 已核實 | ✅ 已查證 | 查證依據（如 Crossref DOI 全符） |
| ⚠️ 資訊不符 / DOI 錯誤 | ❓ 待查 | 列出差異或更正後 DOI；作者更正並核實後自行改 ✅ |
| ❓ 待人工 | ❓ 待查 | 建議人工管道（華藝／國圖／WorldCat…） |
| ❌ 查無此文 | ❓ 待查 | 標「疑虛構，待回源」；**作者確認虛構並刪除後，由作者自行改標 🗑 並記原因** |

> 本 skill 不替作者判定「假」、不寫入 🗑。痕跡表只如實記錄查核狀態，最終取捨權在作者。
```

- [ ] **Step 3: 自我核對（對照鐵律 2「查無≠偽造」）**

確認：四種結果映射齊全；❌ 與 ⚠️ 都落到 ❓ 而非 🗑／「假」；明文寫「本 skill 不寫入 🗑」。

- [ ] **Step 4: 停下檢查點**

回報：citation-verify 已加第 5 步。不 commit。

---

### Task 4: 接 self-review（AI 建議先標 🤖待核，採納後轉 🧍）

**Files:**
- Modify: `skills/self-review/SKILL.md`（在「### 第 4 步：意見分級與輸出」之後新增「### 第 5 步：寫入工作痕跡表」）

- [ ] **Step 1: 確認插入點**

Read `skills/self-review/SKILL.md`，定位第 4 步輸出範本區塊結尾（約 106 行「決定權在作者」那句之後）、「## 已知陷阱」之前。新步驟插在此處。

- [ ] **Step 2: 插入「第 5 步」整段**

插入：

```markdown
### 第 5 步：寫入工作痕跡表

若使用者有維護工作痕跡表（見 CONVENTIONS.md §9），把本次審查產生的內容按其格式追加為列（標記定義以 §9 為準）：

- **審查意見本身**：由本 skill（AI）提出 → 標 🤖 AI待核，「來源／原因」記意見分級（必改／可辯／誤讀）與回指段落。
- **作者採納後**：作者把某條改進落實進稿件、確認那是自己的判斷後 → 由**作者**把對應內容改標 🧍 你判斷。本 skill 不替作者改成 🧍。
- **誠信自查發現的可疑引用**：標 ❓ 待查，「來源／原因」註明「疑虛構，建議跑 citation-verify」——與 citation-verify 的痕跡列一致，不重複造一套標記。

> 守本 skill 鐵律「只診斷不代筆」：痕跡表只記「這條意見是 AI 提的、待你判斷」，採納與否、改成 🧍 永遠是作者的動作。
```

- [ ] **Step 3: 自我核對（對照鐵律 4「只診斷不代筆」）**

確認：AI 意見一律先 🤖待核；轉 🧍 明文為「作者的動作」；可疑引用標 ❓ 並導向 citation-verify（不另立標記）。

- [ ] **Step 4: 停下檢查點**

回報：self-review 已加第 5 步。不 commit。

---

### Task 5: 接 ai-disclosure（讀痕跡表作為證據底稿）

**Files:**
- Modify: `skills/ai-disclosure/SKILL.md`（修改「### 第 1 步」開頭與「### 第 4 步：留痕＝自證檔案」）

- [ ] **Step 1: 確認插入點**

Read `skills/ai-disclosure/SKILL.md`，定位第 1 步（約 24-26 行）與第 4 步（約 58-60 行）。

- [ ] **Step 2: 在第 1 步盤點開頭加入「先讀痕跡表」**

把第 1 步（`### 第 1 步：盤點你到底怎麼用了 AI`）內文開頭加入一句（插在現有「誠實列出…」之前）：

```markdown
**先看有沒有工作痕跡表（CONVENTIONS.md §9）。** 若使用者已維護該表，它就是現成的盤點底稿：🧍 是作者判斷、🤖 是 AI 待核／已採納、✅／❓ 是查證狀態、🗑 是放棄的方向。直接據表盤點，比憑空回憶更準、更不會漏或美化。
```

- [ ] **Step 3: 改寫第 4 步，明確以痕跡表為證據核心**

把第 4 步（`### 第 4 步：留痕＝自證檔案`）內文開頭加入：

```markdown
**工作痕跡表（CONVENTIONS.md §9）就是這份自證檔案的核心底稿。** 它逐列記下「哪些是你判斷、哪些 AI 待核、哪些已查證、哪些放棄」，正是被質疑代寫／抄襲時最直接的反證。除此之外仍建議保存：關鍵 AI 對話、文稿版本演進、citation-verify 報告。
```

（保留第 4 步原有其餘文字。）

- [ ] **Step 4: 自我核對（對照 spec：兩個幫手接起來、不需新增 skill）**

確認：盤點與自證都以痕跡表為底稿；未新增重複功能；守鐵律 2「具體不空話」（表提供具體分工事實）。

- [ ] **Step 5: 停下檢查點**

回報：ai-disclosure 第 1／4 步已改。不 commit。

---

### Task 6: 為三個 skill 補痕跡表回歸斷言

**Files:**
- Modify: `evals/citation-verify.md`
- Modify: `evals/self-review.md`
- Modify: `evals/ai-disclosure.md`

- [ ] **Step 1: citation-verify eval 加斷言**

Read `evals/citation-verify.md`。在「## ✅ 必須做到（MUST）」清單末加：

```markdown
- 若使用者維護工作痕跡表，按 CONVENTIONS §9 追加列：✅已核實→✅已查證、⚠️/❌/❓→❓待查（附依據或更正建議）。
```

在「## ⛔ 必須不做（MUST NOT）」清單末加：

```markdown
- 不得在痕跡表寫入 🗑 或「假」；查無一律 ❓（🗑 只由作者標）。
```

- [ ] **Step 2: self-review eval 加斷言**

Read `evals/self-review.md`。在其 MUST 區加：

```markdown
- 寫入痕跡表時，AI 提的審查意見一律標 🤖AI待核；轉 🧍你判斷是作者的動作，本 skill 不代為改標。
- 可疑引用在痕跡表標 ❓待查並導向 citation-verify，不另立標記。
```

（若 `evals/self-review.md` 無 MUST／MUST NOT 分節，比照 citation-verify 的分節格式新增對應區塊後再加。）

- [ ] **Step 3: ai-disclosure eval 加斷言**

Read `evals/ai-disclosure.md`。在其 MUST 區加：

```markdown
- 若存在工作痕跡表，揭露聲明須據表盤點（🧍/🤖/✅/❓/🗑），不得寫成「使用了 AI 輔助」這類空話。
```

- [ ] **Step 4: 停下檢查點**

回報：三份 eval 已補斷言，貼出新增行供審。不 commit。

---

### Task 7: 實跑真錄一份 example（CONVENTIONS §4 升版前置）

**Files:**
- Create: `examples/2026-06-19-work-trace-table-case.md`

> 說明：CONVENTIONS §4 規定 skill 升版號前須有 ≥1 篇實跑真錄。本任務需**真的跑一次**，不可虛構輸出。建議重用既有真實材料（如 `examples/2026-06-12-master-thesis-case.md` 的 47 筆引用）以降低成本。

- [ ] **Step 1: 選材並實跑 citation-verify → 痕跡表**

取既有碩論 47 筆引用材料，實跑 citation-verify 第 1-5 步，產出查核報告**並**生成對應的工作痕跡表（含 3 筆 DOI 貼錯應落 ❓待查＋更正建議，查無不可標 🗑）。

- [ ] **Step 2: 接續實跑 self-review 與 ai-disclosure 對同一痕跡表**

對同一份痕跡表：self-review 追加幾條 🤖AI待核 審查意見；ai-disclosure 據整張表生成一段揭露聲明。觀察三者標記是否一致、無衝突。

- [ ] **Step 3: 把真錄與暴露的坑寫成 example**

建立 `examples/2026-06-19-work-trace-table-case.md`，比照既有 example 結構記錄：用什麼真實材料、痕跡表如何被三 skill 填／讀、暴露了什麼坑（如標記映射模糊、作者/AI 動作邊界）、怎麼寫回規則。

- [ ] **Step 4: 把暴露的坑回寫 SKILL.md／eval**

若實跑暴露新坑，回到 Task 3-6 對應檔補規則（這是 Boya 的「跑→寫回」紀律，不可略過）。

- [ ] **Step 5: 停下檢查點**

回報：example 已建立，列出暴露的坑與已回寫的規則。不 commit。

---

### Task 8: 文件與索引同步

**Files:**
- Modify: `VERIFICATION.md`
- Modify: `MEMORY.md`

- [ ] **Step 1: VERIFICATION.md 補實測列**

Read `VERIFICATION.md`。在底部實測表（70 行起的表格）加一列（或在 Evidence Ledger 區補一筆），記錄本次工作痕跡表實測：日期 2026-06-19、用的材料（碩論 47 筆）、暴露的坑、對應 example 路徑。

- [ ] **Step 2: MEMORY.md 索引補一行**

Read `MEMORY.md`，在合適區段加一行指向本功能（一句話 hook：工作痕跡表＝跨三個誠信 skill 的統一留痕格式，定義在 CONVENTIONS §9）。

- [ ] **Step 3: 確認 README 計數不變**

確認四份 README 的 skill 計數仍為 13（本案不新增 skill，不需改數字）。如要在 README 提及本功能，僅加一句描述、不動 13 這個數。

- [ ] **Step 4: 停下檢查點**

回報：VERIFICATION／MEMORY 已同步，README 計數確認未動。不 commit。彙整全案改動清單，交付使用者。

---

## Self-Review（計畫對 spec 的覆蓋核對）

- **spec §設計①（表的樣子）** → Task 1（CONVENTIONS 定義五標記＋欄位）、Task 2（範本）。✅
- **spec §設計②（放哪、叫什麼）** → Task 1（CONVENTIONS §9）、Task 2（templates/work-trace-table.md）。✅
- **spec §設計③（三個幫手怎麼填）** → Task 3（citation-verify）、Task 4（self-review）、Task 5（ai-disclosure）。✅
- **spec §設計④（邊界）** → Task 1 §8.3、Task 3「不寫 🗑」、Task 4「只診斷不代筆」皆落實。✅
- **spec §非目標（不新增 skill／不事後重建／不替判斷對錯）** → 全程無新增 skill；ai-disclosure 改為「讀當下累積的表」而非事後重建；標記只記「誰做的」。✅
- **spec §驗收（examples＋evals＋VERIFICATION）** → Task 6（evals）、Task 7（example）、Task 8（VERIFICATION）。✅
- **spec §版號影響（README 計數不變 13）** → Task 8 Step 3。✅
- **標記一致性**：全計畫五個標記用字統一（🧍你判斷／🤖AI待核／✅已查證/已核實映射明確／❓待查／🗑捨棄）；citation-verify 的內部標記（✅已核實／⚠️／❌／❓待人工）→ 痕跡表標記的映射表在 Task 3 明確定義，無歧義。✅
- **無 placeholder**：每個檔案改動都給了完整可貼入的 Markdown 內容。✅
