# outline-builder「逆向體檢」模式 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 為 `outline-builder` 新增「逆向體檢」模式——拿已寫好的草稿反推每段骨架、用段落微觀清單逐段體檢、再對回主線，輸出逆向體檢表——把借自 Master-cai/Research-Paper-Writing-Skills 的「逆向大綱＋段落流暢度」想法落成 Boya 自己的工法。

**Architecture:** 合一份、落在 `outline-builder`。既有「正向搭骨架」模式不動，新增並列的「逆向體檢」模式（三動作：反推骨架→微觀清單→對主線）。`style-tune`／`self-review` 各加一句交叉指引。不新增 skill，不碰其餘 skill 內臟。

**Tech Stack:** 純 Markdown（skill／eval／example／convention 文件）。無程式碼、無建置鏈。驗證走 Boya 既有機制：`evals/outline-builder.md` 回歸斷言 ＋ `examples/` 實跑真錄 ＋ `VERIFICATION.md` 證據帳。

**重要約束：**
- **本計畫不執行 git commit**（依使用者指示）。原模板的「Commit」步驟一律替換為「停下檢查點」。
- **繁體中文優先**。
- **只診斷不代筆**：逆向體檢只標問題與方向，不替作者改寫句子（改寫屬 `style-tune`）。
- **DRY**：warrant 不重新定義，指回既有第 4 步；`style-tune`／`self-review` 用交叉指引，不重抄逆向體檢內容。
- **不搬 STEM 工法**：不引入 teaser／pipeline 圖、實驗章節、minimal-ink 表格等。
- **不新增 skill**，四份 README skill 計數維持 13。

---

## 微觀清單與輸出表（本計畫共用，實作以 outline-builder SKILL.md 為準）

**段落微觀清單四項（＝想法 #2）：**

| 檢查項 | 問法 |
|---|---|
| 一段一意 | 這段只講一件事嗎？混了兩三件就標出。 |
| 首句點題 | 第一句就講了那件事嗎？沒有 → 指出該把哪句提前。 |
| 句關係清楚 | 每一句到下一句是 因果／轉折／推論／細化 哪一種？接不上＝邏輯斷。 |
| 名詞／術語 | 名詞能自己站住嗎？術語先定義再用了嗎？ |

**逆向體檢表欄位：** 段落 ｜ 抽出的主題句 ｜ 一段一意 ｜ 首句點題 ｜ 句關係 ｜ 扣回主線 ｜ 處置（砍／補／搬／改）。

---

## File Structure（先鎖定，再拆任務）

- `skills/outline-builder/SKILL.md`（修改）— 核心。description 補觸發語；開頭區分正向／逆向兩模式；新增「逆向體檢模式」工作流（三動作＋微觀清單＋輸出表）；陷阱區補兩條。
- `skills/style-tune/SKILL.md`（修改）— 邊界情況加一句交叉指引。
- `skills/self-review/SKILL.md`（修改）— 邊界情況加一句交叉指引。
- `evals/outline-builder.md`（修改）— 補逆向體檢回歸斷言。
- `examples/2026-06-20-outline-reverse-checkup-case.md`（新建）— 實跑真錄。
- `VERIFICATION.md`（修改）— 補實測列。

依賴順序：Task 1（核心模式）→ Task 2／3（交叉指引，可平行）→ Task 4（eval）→ Task 5（example 實跑）→ Task 6（文件同步）。

---

### Task 1: outline-builder 新增「逆向體檢」模式

**Files:**
- Modify: `skills/outline-builder/SKILL.md`（frontmatter description；「## 你的角色」區；於「### 第 5 步：檢查章節邏輯」之後、「## 已知陷阱」之前新增整段；「## 已知陷阱」清單末補兩條）

- [ ] **Step 1: 擴充 description 觸發語**

把 frontmatter `description:` 結尾（現為「…特別盯緊最易被省略的 warrant（推理橋）。」）之後，補上一句：

```
也支援「逆向體檢」模式：當用戶說「幫我看這稿讀不讀得順」「邏輯有沒有洞」「這幾段鬆不鬆散」「檢查我寫好的這幾段」時，拿已寫好的草稿反推骨架、逐段用微觀清單體檢、再對回主線；只診斷不代筆，改寫交給 style-tune。
```

- [ ] **Step 2: 在「## 你的角色」區加入兩模式說明**

於「## 你的角色」段落最後（現為「…那不是他想清楚的路。」之後）新增一段：

```markdown
本 skill 有兩種模式：
- **正向搭骨架（寫之前）**：從研究問題、文獻、方法長出大綱與論證鏈——即下方「## 工作流」。
- **逆向體檢（寫之後）**：拿已寫好的草稿反推骨架、抓鬆散、對回主線——即「## 逆向體檢模式」。兩者用的是同一套「論點—主題句—證據」邏輯，只是方向相反。
```

- [ ] **Step 3: 新增「## 逆向體檢模式」整段**

在「### 第 5 步：檢查章節邏輯」結尾（現為「…變成『會動的身體』。」）之後、「## 已知陷阱」之前，插入：

````markdown
## 逆向體檢模式（對已寫好的草稿）

當研究者已有草稿、想檢查「讀不讀得順、邏輯有沒有洞、段落鬆不鬆散」時走這條。**只診斷、給方向，不替作者改寫句子**（改寫交給 `style-tune`）。三個動作：

### 動作 1：反推骨架

拿草稿，逐段抽出「這段的主題句（一句話它到底想說什麼）＋它給的證據」。先做章層級（沿用第 3 步「每章一句話串起來讀」），再往下到**段落層級**逐段抽。抽不出單一主題句的段落，本身就是警訊。

### 動作 2：逐段套微觀清單

對每一段檢查四項：

| 檢查項 | 問法 |
|---|---|
| 一段一意 | 這段只講一件事嗎？混了兩三件就標出。 |
| 首句點題 | 第一句就講了那件事嗎？沒有 → 指出該把哪句提前。 |
| 句關係清楚 | 每一句到下一句是 **因果／轉折／推論／細化** 哪一種？接不上＝邏輯斷。 |
| 名詞／術語 | 名詞能自己站住嗎？術語先定義再用了嗎？ |

### 動作 3：對主線

逐層扣回：每段主題句扣得回該章論點嗎？章論點扣得回研究問題嗎？證據扣得回主題句嗎（warrant 在不在，見第 4 步）？扣不上的標處置：**該砍／該補／該搬**。

### 輸出：逆向體檢表

```markdown
# 逆向體檢報告：<稿件／章節>

| 段落 | 抽出的主題句 | 一段一意 | 首句點題 | 句關係 | 扣回主線 | 處置 |
|---|---|---|---|---|---|---|
| 第2章第3段 | （它其實在說……） | ✓ | ✗ 首句沒點題 | ✓ | ✓ | 改：把第3句提到開頭 |
| 第2章第4段 | （說不清） | ✗ 混了兩件事 | — | ✗ 第2句突兀 | ✗ 扣不回 | 拆成兩段或刪 |

## 一句話總評
<最該先處理的鬆散點>
```

> 體檢只指出洞與方向；砍不砍、補不補、怎麼補，作者決定（沿用鐵律 1「骨架是你的」）。
````

- [ ] **Step 4: 「## 已知陷阱」補兩條**

在「## 已知陷阱」清單末（現為第 5 條「完整性幻覺」之後）加：

```markdown
6. **只抽骨架不對主線**：逆向體檢只做動作 1（抽出主題句）就交差，漏了動作 3（扣回主線）。對治：三動作必須齊全，抽完一定要對回章論點與研究問題。
7. **把改寫當診斷（越界代筆）**：逆向體檢時忍不住替作者重寫整段。守「只診斷不代筆」——標出問題與方向即可，改寫交給 `style-tune`。
```

- [ ] **Step 5: 自我核對（對照 spec §設計①②③④）**

確認：description 含逆向觸發語；兩模式說明清楚；三動作齊全＋微觀清單四項＋輸出表；陷阱補「只抽骨架不對主線」「把改寫當診斷」；warrant 指回第 4 步未重新定義；無 TODO。

- [ ] **Step 6: 停下檢查點**

回報：outline-builder 已加逆向體檢模式，貼出新增段落供審。不 commit。

---

### Task 2: style-tune 加交叉指引

**Files:**
- Modify: `skills/style-tune/SKILL.md`（「## 邊界情況」區）

- [ ] **Step 1: 確認插入點**

Read `skills/style-tune/SKILL.md`，定位「## 邊界情況」最後一項（現為「與前後串用：骨架（outline-builder）→ 本 skill 填血肉 → self-review…」）。

- [ ] **Step 2: 加入交叉指引**

在「## 邊界情況」清單末新增一項：

```markdown
- **讀起來鬆散／邏輯斷，不只是詞句問題** → 先回 `outline-builder` 跑「逆向體檢」（反推骨架、逐段對主線），把結構性鬆散找出來再來潤詞句。潤稿治不了論證鬆散（守鐵律 1）。
```

- [ ] **Step 3: 停下檢查點**

回報：style-tune 已加交叉指引。不 commit。

---

### Task 3: self-review 加交叉指引

**Files:**
- Modify: `skills/self-review/SKILL.md`（「## 邊界情況」區）

- [ ] **Step 1: 確認插入點**

Read `skills/self-review/SKILL.md`，定位「## 邊界情況」最後一項（現為「與 citation-verify／lit-matrix 串用…」）。

- [ ] **Step 2: 加入交叉指引**

在「## 邊界情況」清單末新增一項：

```markdown
- **段落層級鬆散／讀不順**（非整篇論證問題）→ 交給 `outline-builder` 的「逆向體檢」逐段處理；本 skill 的一桌審稿人專攻整篇論證與誠信，段落微觀流暢由逆向體檢補。
```

- [ ] **Step 3: 停下檢查點**

回報：self-review 已加交叉指引。不 commit。

---

### Task 4: evals/outline-builder.md 補逆向體檢斷言

**Files:**
- Modify: `evals/outline-builder.md`

- [ ] **Step 1: MUST 區補斷言**

Read `evals/outline-builder.md`。在「## ✅ 必須做到（MUST）」清單末加：

```markdown
- 逆向體檢模式三動作齊全：反推骨架 → 逐段套微觀清單（一段一意／首句點題／句關係四種／術語） → 對回主線；不可只抽骨架不對主線。
- 逆向體檢輸出逆向體檢表，每段給處置（砍／補／搬／改）並附一句總評。
```

- [ ] **Step 2: MUST NOT 區補斷言**

在「## ⛔ 必須不做（MUST NOT）」清單末加：

```markdown
- 逆向體檢時不得替作者改寫整段（只診斷不代筆，改寫屬 style-tune）。
```

- [ ] **Step 3: 「已暴露的坑」補一條**

在「## 已暴露的坑（防重犯）」末加：

```markdown
- 逆向體檢只抽骨架不對主線＝半套；抽完必須扣回章論點與研究問題才算數。
```

- [ ] **Step 4: 停下檢查點**

回報：eval 已補斷言。不 commit。

---

### Task 5: 實跑真錄一份 example（CONVENTIONS §4 升版前置）

**Files:**
- Create: `examples/2026-06-20-outline-reverse-checkup-case.md`

> 說明：CONVENTIONS §4 規定 skill 升版號前須有 ≥1 篇實跑真錄。本任務需**真的跑一次**，不可虛構輸出。建議重用既有真實材料 `examples/2026-06-14-outlinebuilder-thesis.md` 的碩論章節，或作者其他真實草稿段落。

- [ ] **Step 1: 選材並實跑逆向體檢**

取既有碩論某章真實草稿段落，實跑逆向體檢三動作，產出逆向體檢表（含至少一段「混了兩件事」「首句沒點題」「扣不回主線」的真實案例）。

- [ ] **Step 2: 把真錄與暴露的坑寫成 example**

建立 `examples/2026-06-20-outline-reverse-checkup-case.md`，比照既有 example 結構記錄：用什麼真實材料、逆向體檢表長怎樣、暴露了什麼坑（如句關係四分類是否夠用、主題句抽取的歧義）、怎麼寫回規則。

- [ ] **Step 3: 把暴露的坑回寫 SKILL.md／eval**

若實跑暴露新坑，回 Task 1／4 對應檔補規則（Boya 的「跑→寫回」紀律，不可略過）。

- [ ] **Step 4: 停下檢查點**

回報：example 已建立，列出暴露的坑與已回寫的規則。不 commit。

---

### Task 6: 文件與索引同步

**Files:**
- Modify: `VERIFICATION.md`
- Modify: `MEMORY.md`

- [ ] **Step 1: VERIFICATION.md 補實測列**

Read `VERIFICATION.md`。在底部實測表加一列（或 Evidence Ledger 補一筆）：skill = outline-builder（逆向體檢新增）、日期 2026-06-20、用的材料、暴露的坑、對應 example 路徑。

- [ ] **Step 2: MEMORY.md 索引補一行**

Read `MEMORY.md`，加一行 hook：outline-builder 新增「逆向體檢」模式（拿成稿反推骨架＋段落微觀清單＋對主線），借自 Master-cai/Research-Paper-Writing-Skills 的想法、自行重寫。

- [ ] **Step 3: 確認 README 計數不變**

確認四份 README skill 計數仍為 13（本案不新增 skill）。如於 README 提及逆向體檢，僅加一句描述、不動 13。

- [ ] **Step 4: 停下檢查點**

回報：VERIFICATION／MEMORY 已同步，README 計數確認未動。彙整全案改動清單交付使用者。不 commit。

---

## Self-Review（計畫對 spec 的覆蓋核對）

- **spec §設計①（兩模式、何時用、觸發語）** → Task 1 Step 1-2。✅
- **spec §設計②（三動作：反推骨架／微觀清單／對主線）** → Task 1 Step 3（動作 1-3＋微觀清單四項）。✅
- **spec §設計③（逆向體檢表輸出）** → Task 1 Step 3 輸出區。✅
- **spec §設計④（邊界：只診斷不代筆、骨架是作者的、DRY 交叉指引）** → Task 1 Step 3 引言＋總評、Task 1 Step 4 陷阱 7、Task 2／3 交叉指引、warrant 指回第 4 步。✅
- **spec §具體改動清單（6 項）** → Task 1（outline-builder）、Task 2（style-tune）、Task 3（self-review）、Task 4（eval）、Task 5（example）、Task 6（VERIFICATION＋MEMORY）。✅
- **spec §驗收（example＋eval＋VERIFICATION）** → Task 4／5／6。✅
- **spec §非目標（不新增 skill／不搬 STEM 工法／不代筆／不重定義 warrant）** → 全程無新增 skill；無 STEM 內容；Task 1 陷阱 7＋邊界守不代筆；warrant 指回第 4 步。✅
- **spec §版號（README 計數不變 13）** → Task 6 Step 3。✅
- **用字一致性**：微觀清單四項（一段一意／首句點題／句關係四種＝因果／轉折／推論／細化／名詞·術語）、三動作（反推骨架／微觀清單／對主線）、輸出表欄位，在 Task 1／4／計畫頂部說明全程一致。✅
- **無 placeholder**：每個檔案改動都給了完整可貼入的 Markdown 內容。✅
