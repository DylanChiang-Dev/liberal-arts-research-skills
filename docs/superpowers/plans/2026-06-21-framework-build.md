# framework-build（理論框架定錨）Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 新增獨立 skill `framework-build`（理論框架定錨），插在 `lit-matrix` 與 `method-design` 之間，固化真實 JASM 場景驗證過的理論框架定錨工法，並重編 `research-roadmap` 階段。

**Architecture:** 這是 skill／文件撰寫專案，不是程式碼。對映 Boya 慣例的「TDD」＝**先寫 eval（定 MUST／MUST NOT）→ 再寫 SKILL.md → 拿 example 乾跑核對斷言 → commit**。每個檔案一個 task，frequent commits。所有改動遵循 CONVENTIONS.md（目錄／frontmatter／§3 段落模板／§4 出案例硬規／§8 分層）與 AGENTS.md 版本策略。

**Tech Stack:** Markdown only。skill 目錄 `skills/framework-build/SKILL.md`（繁中、單檔、kebab-case）；eval `evals/framework-build.md`；template `templates/framework-anchor.md`；example `examples/2026-06-21-framework-jasm.md`。

**Spec:** `docs/superpowers/specs/2026-06-21-framework-build-design.md`

---

## File Structure

**新增**
- `evals/framework-build.md` — 回歸斷言（MUST／MUST NOT／已暴露的坑）。**先寫**，當「測試」。
- `skills/framework-build/SKILL.md` — skill 本體，CONVENTIONS §3 四段＋三模式。預估 ~170 行，壓在 §8 的 200 行軟線內 → 單檔，不開 `references/`。
- `templates/framework-anchor.md` — 定錨表可填空骨架。
- `examples/2026-06-21-framework-jasm.md` — JASM 真實對話脫敏後的首個實跑真錄。

**改動**
- `ROUTER.md` — 新增一列。
- `skills/research-roadmap/SKILL.md` — 全流程總表插階段＋重編號＋接力與階段引用同步。
- `GUIDE.md` — 第 3 節對照表、第 4 節工作流順序。
- `README.md`／`README.en.md`／`README.ja.md`／`README.zh-CN.md` — skill 計數 13→14、流程圖、清單。
- `VERIFICATION.md` — 新增 `framework-build` 列（標 Draft）＋ skill 計數敘述。
- `skills/outline-builder/SKILL.md`、`skills/method-design/SKILL.md`、`skills/lit-matrix/SKILL.md` — 各加一句交叉指引。

---

## Task 1: 先寫 eval（定義 MUST／MUST NOT＝測試）

**Files:**
- Create: `evals/framework-build.md`

- [ ] **Step 1: 寫 eval 斷言檔**

依 `evals/README.md` 格式、對齊 spec §驗收。基準輸入指向 Task 4 才會建的 example（先寫路徑，Task 4 補檔）。完整內容：

```markdown
# eval：framework-build

## 基準輸入

見 [examples/2026-06-21-framework-jasm.md](../examples/2026-06-21-framework-jasm.md)：日台半導體經濟安全（JASM 戰略耦合）課題的理論框架定錨真錄。

## ✅ 必須做到（MUST）

- 先判論文類型再定錨：實證型給「概念框架」（概念—變項—命題—關係模型），思辨型給「理論視角／透鏡」，不一律套同一種。
- 候選框架攤成四欄表：框架 ｜ 解釋什麼 ｜ 理論代價 ｜ 庫存支撐。
- 每個候選框架誠實標庫存支撐（✅有／🟡部分／❌需補 X 篇）；庫裡沒有的框架必標「需補」並指回 citation-verify／lit-matrix。
- 給明確推薦的分層結構（主框架 → 中介機制 → 實證抓手 → 落點）並附「為什麼這樣分」的理由。
- 硬 GATE：停在主框架選擇，給「採納推薦／保守最小／你另有偏好」三路，明確標示只有研究者能拍板。
- 為每個關鍵概念指出「實證抓手候選」，使框架能交給 method-design 操作化。
- 輔助框架嵌入模式：判斷外加理論該當主框架還桥接層，給定位＋代價（喧賓奪主／滑回紅海），採輔助時明確它承擔哪 1–2 個任務、嵌哪一層。
- 逆向體檢模式：從已寫好的框架反推分層與承重文獻，逐項體檢（承重文獻真偽／對得上研究問題／兩張皮／框架沙拉／實證抓手），只診斷不代筆。

## ⛔ 必須不做（MUST NOT）

- 不得替研究者拍板主框架（攤候選＋推薦可以，採納與否是研究者的）。
- 不得假裝庫裡沒有的框架已有承重文獻；不得編造框架的支撐文獻。
- 不得堆「框架沙拉」：主框架超過一個、或加層卻說不清分工，即為退化。
- 不得越界代做 method-design 的操作化（只指出實證抓手候選，不起草訪談題／問卷／指標體系）。
- 不得越界代做 outline-builder 的章節骨架。
- 逆向體檢時不得替作者改寫框架段落（改寫屬 style-tune）。

## 已暴露的坑（防重犯）

- 框架沙拉：真實場景一度想同時上地緣經濟學＋武器化相互依賴＋安全化＋戰略耦合四層；紀律是精簡三層、安全化降為輔助。
- 紅海誤選：把准同盟理論抬成主框架會把研究從「經濟安保產業鏈」滑回擁擠的「日台安全」紅海，丟掉經濟差異化；准同盟只能當桥接辅助層。
- 假地基：戰略耦合（Yeung&Coe）庫裡沒有時，必標「需補 2 篇」交回研究者找真文獻，不得憑記憶當已有支撐。
- 兩張皮：框架選了一套、method-design 操作化做另一套——定錨時就要為每個概念指出實證抓手，從根上防。
```

- [ ] **Step 2: 自核 eval 與 spec §驗收逐條對齊**

Run（人工核對，非自動化）：打開 spec `docs/superpowers/specs/2026-06-21-framework-build-design.md` §驗收，逐條確認 6 個 MUST 主題（不編承重文獻／硬 GATE／不堆沙拉／可操作化／分學科分流／三模式齊全）在 eval 都有對應斷言。
Expected: 6 主題全部有斷言；缺則補。

- [ ] **Step 3: Commit**

```bash
rtk git add evals/framework-build.md
rtk git commit -m "test: framework-build eval 回歸斷言（先於 SKILL.md）"
```

---

## Task 2: 寫 SKILL.md（讓 eval 斷言全部能命中）

**Files:**
- Create: `skills/framework-build/SKILL.md`

- [ ] **Step 1: 寫 SKILL.md 本體**

依 CONVENTIONS §3 四段（角色／鐵律／工作流／輸出格式），voice 對齊既有 skill（鐵律編號、表格、`**僅你能決定**` 標記）。完整內容：

````markdown
---
name: framework-build
description: 理論框架定錨技能。當用戶說「我這篇要用什麼理論框架」「幫我定錨理論框架」「有哪些框架可以套」「主框架該選哪個」，或文獻讀完要把理論透鏡定下來時使用。從研究問題與文獻地圖萃取候選框架、攤成「解釋什麼／理論代價／庫存支撐」對照表、推薦分層結構（主框架→中介機制→實證抓手→落點）並硬停在 GATE 讓研究者拍板主框架。也支援「輔助框架嵌入」（判斷外加理論該當主框架還桥接層、會不會喧賓奪主）與「逆向體檢」（拿寫好的框架反推它撐不撐得住、有沒有兩張皮）。絕不替用戶決定主框架、絕不假裝庫裡沒有的框架已有承重文獻（庫外框架一律標「需補 X 篇」交回查證）、不堆框架沙拉、不越界代做方法操作化或章節骨架。
---

# Framework-Build　理論框架：從文獻長出框架，你定錨

## 你的角色

你是一位**理論框架顧問**。研究者讀完文獻（最好先過 `lit-matrix`），手上有一張文獻對話地圖與一個磨好的研究問題，現在要回答一個比選方法更上游的問題：**「我用什麼理論透鏡看這整個問題？關鍵概念是什麼？概念之間怎麼連？」**

你幫他：從文獻裡萃取候選框架、攤開每個框架的代價、推薦一個精簡分層的結構——但**主框架選哪個，是他拍板，不是你填的空**。框架選擇藏著學科品味與卡位戰略，你攤牌、給推薦、講清代價，然後把方向盤交回去。

框架定錨完，往下交棒：操作化成方法是 `method-design` 的事，長成章節是 `outline-builder` 的事。你只負責把透鏡磨清楚。

## 鐵律

1. **框架從文獻長出，不從記憶編。** 候選框架要能對回研究者手上已有的文獻。你可以基於通識**提名**一個庫裡沒有的框架，但必須誠實標「庫存支撐」（✅有／🟡部分／❌需補 X 篇）；沒地基的一律標「需補」、交回研究者去找真文獻（指回 `citation-verify`／`lit-matrix`），**絕不假裝已有支撐**。框架的承重文獻也要是真的。
2. **攤候選＋給推薦，但主框架是你的（硬 GATE）。** 這跟 `topic-refine`「不替你選題」不衝突——框架的代價是**可攤開、可陳述的事實**（理論代價、庫存支撐、紅海風險），AI 在可分析的地基上給推薦是有用的；但「採不採納」藏著學科品味與戰略卡位，那是研究者的判斷。GATE 必停，給「採納推薦／保守最小／你另有偏好」三路，**拍板才接力 `method-design`**。
3. **不堆框架沙拉。** 主框架最多一個。每多一個輔助／中介框架，都要回答「它幹的活前面的框架幹不了嗎」；答不出就砍。理論豐富不是靠層數堆出來的。
4. **框架要對得上研究問題與差異化卡位。** 框架是透鏡不是裝飾。盯兩件事：它解釋的東西扣不扣得回研究問題？選它會不會把研究推回擁擠紅海、丟掉差異化？（實測：選准同盟當主框架，會把「經濟安保產業鏈」滑回擁擠的「日台安全」紅海。）
5. **框架要能往下操作化。** 定錨完要能交給 `method-design` 變指標、交給 `outline-builder` 長章節。一個落不到實證抓手的框架是空的。報告結尾要為每個關鍵概念指出「實證抓手候選」。
6. **分學科分流，不一套打天下。** 實證／社科型要的是**概念框架**（關鍵概念—變項—命題—關係模型）；人文思辨型要的是**理論視角／透鏡**（選一個學派／理論家貫穿詮釋）。先判論文類型再定錨，像 `outline-builder` 不預設 IMRaD 一樣。

## 工作流（正向定錨）

### 第 1 步：盤點手上的材料

確認研究者已有（沒有就先回前面的 skill）：研究問題（含類型：描述/解釋/評價）、`lit-matrix` 文獻地圖（共識／分歧／缺口）、論文類型。框架長在這三樣上，缺一個先補。

### 第 2 步：判論文類型 → 選定錨路徑

| 論文類型 | 定錨路徑 | 框架長什麼樣 |
|---|---|---|
| 實證／社科型 | 概念框架 | 關鍵概念 → 變項 → 命題／假設 → 概念間關係模型 |
| 人文思辨型 | 理論視角 | 選一個學派／理論家當透鏡，貫穿整篇詮釋 |
| 政策分析型 | 分析框架 | 把政策問題拆成可評估的維度／準則 |

別預設某一種；先問研究者這篇要回答的是「有多少／為什麼／該不該」哪一類。

### 第 3 步：從文獻地圖萃取候選框架

攤開文獻裡反覆出現、或能解釋研究問題的理論。**可以加上你基於通識提名的庫外框架**（標 ★），但每個都要誠實標庫存支撐。

### 第 4 步：攤候選代價表

| 框架 | 解釋什麼 | 理論代價 | 庫存支撐 |
|---|---|---|---|
| （框架 A） | （它能解釋研究問題的哪一塊） | （概念偏寬／尺度不合／需轉換…） | ✅ 有（作者/來源） |
| （框架 B）★ | … | … | 🟡 部分／❌ 需補 X 篇 |

「理論代價」要寫實：概念太寬會變「筐」、原理論尺度不合要轉換、庫裡沒有要補幾篇。

### 第 5 步：推薦分層結構＋理由

```
主框架   <…>            ← 為什麼是它（貼題面／有理論基石）
  ↓
中介機制 <…>            ← 差異化核心（題眼母體）
  ↓
實證抓手 <…>            ← 可量化／可操作的指標
  ↓
落點    <…>
```

精簡優先（鐵律 3）。把可有可無的框架降為「輔助」、不另立層。

### 第 6 步：🚦 GATE — 只有你能定主框架

停下，把選擇交回研究者，給三條路：

- **採納推薦**：<推薦的分層>（可能需補 X 篇，幫他定位免費版／來源）。
- **保守最小**：<只用庫裡有支撐的框架，差異化弱一些>。
- **你另有偏好**：請他講學科手感（產業經濟／國際關係／區域研究…），框架配他的訓練。

**未拍板前不接力 `method-design`。** 同時交出「待補清單」（哪些框架承重文獻還沒有、回哪個 skill 補）。

### 第 7 步：拍板後接力

輸出定錨表（見下），提示下一步：交給 `method-design` 把框架操作化成方法／指標、交給 `outline-builder` 長成章節。

## 輔助框架嵌入模式

觸發：「我還要不要用理論 X」「要不要加上 Y 理論」「這理論放主框架還是輔助」。

1. **判定位**：X 該當主框架，還是桥接／輔助層？問——它幹的活主框架幹得了嗎？抬成主框架會不會滑回紅海（鐵律 4）？
2. **給代價**：若抬成主框架的風險（喧賓奪主、重心偏移、丟差異化）。
3. **若採輔助**：明確它只承擔哪 **1–2 個具體任務**、嵌在分層結構**哪一層**。（實測：准同盟只承擔「解釋日台無正式關係卻能深度耦合的關係基礎」＋「用牽連／拋棄困境刻畫安全外溢」兩個任務，嵌在中介機制下方當「關係結構透鏡」。）
4. **標庫存支撐**（鐵律 1）、**GATE 交回**（鐵律 2）。

## 逆向體檢模式

觸發：「幫我看我寫好的這段框架撐不撐得住」「框架跟研究問題對不對得上」「有沒有兩張皮」「框架是不是太雜」。

拿已寫好的框架文字，反推它的分層與承重文獻，逐項體檢：

| 體檢項 | 問法 |
|---|---|
| 承重文獻真偽 | 框架引的承重文獻真實存在嗎？（提醒走 `citation-verify`） |
| 對得上研究問題 | 這個透鏡解釋的東西，扣得回研究問題嗎？ |
| 有沒有兩張皮 | 框架說一套、實證／方法做另一套？ |
| 有沒有框架沙拉 | 層數是不是貪多？每層分工說得清嗎？ |
| 實證抓手 | 每個關鍵概念有對得上的實證抓手嗎？ |

**只診斷不代筆**：標出洞與方向，改寫框架段落交 `style-tune`，整篇論證批判交 `self-review`。

## 輸出格式：理論框架定錨表（存 `框架定錨.md`）

```markdown
# 理論框架定錨：<研究問題簡述>

## 一、候選框架攤牌
| 框架 | 解釋什麼 | 理論代價 | 庫存支撐 |
|---|---|---|---|

## 二、推薦分層（理由）
主框架 → 中介機制 → 實證抓手 → 落點

## 三、🚦 GATE — 只有你能定主框架
- 採納推薦：
- 保守最小：
- 你另有偏好：

## 四、待補清單
| 框架 | 缺什麼 | 回哪個 skill |
|---|---|---|
```

可選搭配工作痕跡表（見倉庫 CONVENTIONS §9）：庫存支撐用 ✅ 已查證／❓ 待查，主框架選擇用 🧍 你判斷。
````

- [ ] **Step 2: 乾跑核對 eval（拿 Task 4 的 example 當輸入；若 Task 4 未完成，先用 spec 背景段的 JASM 描述當輸入）**

Run（人工乾跑）：以 JASM 課題（研究問題＝日台半導體經濟安全如何牽動戰略耦合；文獻地圖含 Farrell、Katada、徐萬勝等）為輸入，依 SKILL.md 工作流走一遍，逐條對 `evals/framework-build.md` 的 MUST／MUST NOT 打勾。
Expected: 8 條 MUST 全命中、6 條 MUST NOT 全未犯。任一未過 → 修 SKILL.md，不修 eval（除非 eval 本身寫錯）。

- [ ] **Step 3: 檢查行數是否壓在 §8 軟線內**

Run: `wc -l skills/framework-build/SKILL.md`
Expected: < 200 行 → 維持單檔。若 ≥ 200，依 CONVENTIONS §8 把「分學科定錨路徑」長對照拆進 `skills/framework-build/references/`，SKILL.md 留概念與鐵律。

- [ ] **Step 4: Commit**

```bash
rtk git add skills/framework-build/SKILL.md
rtk git commit -m "feat: 新增 framework-build（理論框架定錨）skill"
```

---

## Task 3: 寫定錨表模板

**Files:**
- Create: `templates/framework-anchor.md`

- [ ] **Step 1: 寫可填空骨架（與 SKILL.md 輸出格式一致，CONVENTIONS §7）**

```markdown
# 理論框架定錨：<填研究問題簡述>

> 用法：把 <…> 換成你的真實材料。查不到承重文獻的框架，庫存支撐欄標「❌ 需補 X 篇」，不要請 AI 猜。主框架由你拍板。

## 一、候選框架攤牌

| 框架 | 解釋什麼 | 理論代價 | 庫存支撐 |
|---|---|---|---|
| <框架 A> | <能解釋研究問題的哪一塊> | <概念太寬／尺度不合／需轉換…> | <✅ 有（來源）／🟡 部分／❌ 需補 X 篇> |
| <框架 B> | | | |

## 二、推薦分層（附理由）

```
主框架   <…>            ← <為什麼是它>
  ↓
中介機制 <…>            ← <差異化核心>
  ↓
實證抓手 <…>            ← <可量化／可操作>
  ↓
落點    <…>
```

## 三、🚦 GATE — 只有你能定主框架

- 採納推薦：<…>
- 保守最小：<…>
- 你另有偏好：<…>

## 四、待補清單

| 框架 | 缺什麼 | 回哪個 skill |
|---|---|---|
| <框架> | <承重文獻 X 篇> | citation-verify / lit-matrix |

## 五、（可選）工作痕跡

| 內容（一句話） | 標記 | 來源／原因 | 日期 |
|---|---|---|---|
| <選定主框架為…> | 🧍 你判斷 | | |
| <框架 B 庫存支撐> | ❓ 待查 | <需補 X 篇> | |
```

- [ ] **Step 2: Commit**

```bash
rtk git add templates/framework-anchor.md
rtk git commit -m "feat: 新增 framework-anchor 定錨表模板"
```

---

## Task 4: 寫實測案例（JASM 真實對話脫敏）

**Files:**
- Create: `examples/2026-06-21-framework-jasm.md`

> 脫敏原則：移除任何可識別個人／單位的私密資訊；保留學術框架討論本體（地緣經濟學／武器化相互依賴／戰略耦合／安全化／准同盟）。這些是公開學術理論，可保留。對話中的真實人名引用（王丰收、陳雨潔等）若屬已發表文獻作者則可保留為文獻引用；若屬私下溝通則改為匿名。

- [ ] **Step 1: 寫案例真錄**

對齊既有 example 體例（背景 → 輸入 → skill 怎麼跑 → 暴露的坑 → 寫回規則）。完整內容：

```markdown
# 案例：framework-build 為日台半導體經濟安全課題定錨理論框架

> 日期：2026-06-21　｜　skill：framework-build（Draft）
> 材料：一篇關於日台半導體經濟安全（JASM／TSMC 熊本廠）戰略耦合的社科課題，文獻已過 lit-matrix。脫敏：保留公開學術理論與已發表文獻引用，移除私密溝通內容。

## 背景與輸入

研究問題：日本以半導體產業鏈為槓桿的經濟安全布局，如何牽動日台戰略耦合與中國應對？
手上文獻地圖（lit-matrix 產出）已含：Farrell & Newman（武器化相互依賴）、Katada／CSIS（地緣經濟學）、徐萬勝（隱性用安全化）、Yeung & Coe（戰略耦合，**庫裡缺**）。論文類型：實證／社科型 → 走概念框架。

## skill 怎麼跑

### 攤候選代價表

| 框架 | 解釋什麼 | 理論代價 | 庫存支撐 |
|---|---|---|---|
| A 武器化相互依賴 | 半導體「咽喉點」如何牽制 | 原理論是美中宏觀尺度，要做中觀轉換 | ✅ Farrell、Armstrong&Solís |
| B 地緣經濟學 | 經濟工具達成地緣目的（最貼題面） | 概念偏寬，易成「筐」 | ✅ Katada、CSIS |
| C 安全化 | 半導體如何「被講成」安全問題 | 哥本哈根核心文獻庫裡沒有 | 🟡 需補 Buzan |
| D 戰略耦合 ★ | 市場分工→國家戰略耦合（題眼「耦合」母體） | 庫裡沒有 | ❌ 需補 2 篇 Yeung&Coe |

### 推薦分層

```
主框架   地緣經濟學（＋武器化相互依賴咽喉點）  ← 貼題面、有理論基石
  ↓
中介機制 戰略耦合 ★差異化核心               ← 題眼「耦合」母體，JASM 當錨點
  ↓
實證抓手 GVC 產業鏈分工指標                ← 投資／技術協議／人才／補貼／專利，可量化
  ↓
落點    安全外溢 ＋ 中國應對
```

安全化降為輔助（只解釋「經濟安保法為何出台」），不另立層、避免發散。

### 🚦 GATE
- 採納推薦：地緣經濟學＋武器化相互依賴 × 戰略耦合 × GVC（需補 2–3 篇）。
- 保守版：只用 A＋B，零補充，差異化弱一些。
- 你另有偏好：講學科手感，框架配你的訓練。

### 輔助框架追問：「需不需要准同盟理論？」
判定：准同盟（Victor Cha, quasi-alliance）貼合度高，但**抬成主框架會把研究滑回「日台安全」紅海、丟掉經濟差異化**。故只當「關係結構透鏡」嵌在中介機制下方，承擔兩個任務：①解釋日台無正式關係卻能深度耦合的關係基礎；②用牽連／拋棄困境刻畫安全外溢。需補 1 篇 Victor Cha。

## 暴露的坑（已寫回鐵律）

1. **框架沙拉**：一度想四層全上；紀律＝精簡三層、安全化降輔助。→ 鐵律 3。
2. **紅海誤選**：准同盟抬成主框架會滑回擁擠的安全賽道。→ 鐵律 4。
3. **假地基**：戰略耦合庫裡沒有，必標「需補 2 篇」而非當已有支撐。→ 鐵律 1。
4. **兩張皮預防**：定錨時就為每個概念指出 GVC 實證抓手。→ 鐵律 5。
```

- [ ] **Step 2: 核對案例覆蓋 eval 全部斷言**

Run（人工）：逐條把 `evals/framework-build.md` 的 MUST／已暴露的坑 對回此 example，確認每條都有對應段落。
Expected: 全覆蓋（這份 example 是 eval 的基準輸入，必須能支撐所有斷言）。

- [ ] **Step 3: Commit**

```bash
rtk git add examples/2026-06-21-framework-jasm.md
rtk git commit -m "docs: framework-build 首個實測案例（JASM 脫敏）"
```

---

## Task 5: 接入 ROUTER.md

**Files:**
- Modify: `ROUTER.md`（在 `lit-matrix` 列之後、`method-design` 列之前插入）

- [ ] **Step 1: 插入路由列**

在 `ROUTER.md` 表格中 `lit-matrix` 那列（讀文獻）之後、`method-design` 那列之前，新增：

```markdown
| 「我這篇要用什麼理論框架／幫我定錨理論框架／主框架選哪個／要不要加某理論／框架撐不撐得住」 | framework-build | 框架 | 主框架選擇（庫外框架承重文獻須你查、不可編） |
```

- [ ] **Step 2: 核對位置**

Run: `rtk grep -n "framework-build\|method-design\|lit-matrix" ROUTER.md`
Expected: framework-build 列位於 lit-matrix 與 method-design 之間。

- [ ] **Step 3: Commit**

```bash
rtk git add ROUTER.md
rtk git commit -m "docs: ROUTER 接入 framework-build"
```

---

## Task 6: 重編 research-roadmap 階段（總表＋接力＋階段引用）

**Files:**
- Modify: `skills/research-roadmap/SKILL.md`

- [ ] **Step 1: 改全流程總表（line ~33-43）**

把現有第 4、5 列改為下列三列（新增階段 4、原 4 移為 5、原 5 移為 6 並更名），並把原 6、7、8、9 列號順移為 7、8、9、10：

```markdown
| 3 | 讀文獻到綜述 | 一批真文獻 | 讀出對話、判斷缺口 | `lit-matrix` | 文獻矩陣＋綜述對話地圖 |
| 4 | 理論框架定錨 | 文獻地圖、確認的缺口 | **主框架選哪個** | `framework-build` | 理論框架定錨表＋待補清單 |
| 5 | 研究設計與資料 | 定錨好的框架 | **選方法、做詮釋** | `method-design` | 研究設計＋訪談/問卷工具 |
| 6 | 大綱與論證 | 框架＋方法＋文獻 | 拍板論證邏輯與骨架 | `outline-builder` | 章節大綱＋論證鏈 |
| 7 | 寫初稿 | 大綱與論證骨架 | 寫骨、把關自己的聲音 | `style-tune` | 各章初稿 |
| 8 | 自我審查（模擬審查） | 一份完整初稿 | 哪條意見要改、哪條要辯 | `self-review` | 分級審查報告（必改/可辯/誤讀） |
| 9 | 修訂、定稿與口試 | 審查/指導意見 | 答辯、署名、最終查證 | `defense-prep`、`cite-format`、`abstract-bilingual` | 定稿＋摘要＋格式＋口試簡報 |
| 10 | 倫理揭露 | 一份要交出去的成品 | 守紅線、負全責 | `ai-disclosure` | AI 使用聲明＋留痕檔案 |
```

> 注意：階段 5「進入條件」由原本的「文獻地圖、確認的缺口」改為「定錨好的框架」，因為框架現在先於方法。階段 6 由「框架與大綱」更名「大綱與論證」。

- [ ] **Step 2: 同步表後的階段引用（line ~52-53, 90, 124）**

逐處改：
- line ~52「要開始設計訪談／問卷 → 階段 4」改為「→ 階段 5」。
- line ~53「有材料、不知怎麼搭結構 → 階段 5」改為「→ 階段 6」。
- line ~90「階段 4／研究設計：會用到 `method-design`」改為「階段 5／研究設計」。
- line ~124「綜述／思辨／政策型論文的階段 4（研究設計）」改為「階段 5（研究設計）」。

Run: `rtk grep -n "階段 [0-9]" skills/research-roadmap/SKILL.md`
Expected: 所有階段號與新總表一致；無殘留指向舊編號的描述。

- [ ] **Step 3: 補階段 4 的接力與 gate 說明**

在總表後的階段導引段（仿 line ~85-90 既有 `lit-matrix`／`method-design` 區塊），新增階段 4 區塊：

```markdown
> **階段 4／理論框架定錨**：會用到 `framework-build`。
> - 喚誰：`framework-build`（從文獻地圖攤候選框架、給分層推薦）。
> - 只有你能決定：**主框架選哪個**。framework-build 攤候選＋給推薦，但採不採納是你的；庫外框架的承重文獻要你去查、不可編。
> - gate 必停：未確認你已拍板主框架、待補文獻已知，**絕不接力 `method-design`**。
```

- [ ] **Step 4: Commit**

```bash
rtk git add skills/research-roadmap/SKILL.md
rtk git commit -m "feat: roadmap 新增階段 4 理論框架定錨並重編號"
```

---

## Task 7: 更新 GUIDE.md

**Files:**
- Modify: `GUIDE.md`（第 3 節對照表 line ~49-64、第 4 節工作流 line ~71-95）

- [ ] **Step 1: 第 3 節對照表插一列**

在 `GUIDE.md` 第 3 節表格中，`lit-matrix`（讀幾篇文獻…）那列之後插入：

```markdown
| 文獻讀完，要把理論透鏡／主框架定下來 | `framework-build` |
```

- [ ] **Step 2: 第 4 節典型工作流插一步**

把第 4 節的工作流序列（line ~71-82）中 `lit-matrix → method-design` 改為 `lit-matrix → framework-build → method-design`：

```text
topic-refine
→ citation-verify
→ lit-matrix
→ framework-build
→ method-design
→ outline-builder
→ style-tune
→ self-review
→ cite-format / abstract-bilingual
→ ai-disclosure
→ defense-prep
```

- [ ] **Step 3: 核對**

Run: `rtk grep -n "framework-build" GUIDE.md`
Expected: 第 3 節表、第 4 節工作流各出現一次。

- [ ] **Step 4: Commit**

```bash
rtk git add GUIDE.md
rtk git commit -m "docs: GUIDE 插入 framework-build 階段"
```

---

## Task 8: 更新四份 README（計數 13→14、流程圖、清單）

**Files:**
- Modify: `README.md`、`README.en.md`、`README.ja.md`、`README.zh-CN.md`

- [ ] **Step 1: README.md — badge 與計數敘述**

- line ~22 badge：`skills-13` → `skills-14`。
- line ~55「十三個 skill」→「十四個 skill」。
- line ~75 標題「## 📦 十三個 skill」→「## 📦 十四個 skill」。
- line ~77 計數敘述「十個核心…＝十三個；其中十二個為 Stable」改為「十一個核心…＝十四個；其中十二個為 Stable、`venue-fit` 為 Beta、`framework-build` 為 Draft」。

- [ ] **Step 2: README.md — 流程圖插節點（line ~61-63）**

把 mermaid 流程：
```
S3[讀文獻<br/>lit-matrix]
S3 --> S4[研究設計<br/>method-design]
S4 --> S5[搭骨架<br/>outline-builder]
```
改為（新增框架節點、後續節點重連）：
```
S3[讀文獻<br/>lit-matrix]
S3 --> SF[理論框架<br/>framework-build]
SF --> S4[研究設計<br/>method-design]
S4 --> S5[搭骨架<br/>outline-builder]
```

- [ ] **Step 3: README.md — skill 清單插一列（line ~85 之後，lit-matrix 之後 method-design 之前）**

```markdown
| [`framework-build`](skills/framework-build) | 理論框架定錨：從文獻地圖攤候選框架（解釋什麼／理論代價／庫存支撐）、推薦分層（主框架→中介機制→實證抓手→落點）、硬 GATE 讓你拍板主框架；另有輔助框架嵌入與逆向體檢兩模式。 |
```

- [ ] **Step 4: README.md — 案例表插一列（line ~186 區）**

```markdown
| 014 | [framework-build 定錨日台半導體框架](examples/2026-06-21-framework-jasm.md) | framework-build |
```
（編號接續既有最大號；若既有非 014，改為最大號+1。）

- [ ] **Step 5: 三份譯本同步**

對 `README.en.md`／`README.ja.md`／`README.zh-CN.md` 做對應改動（計數、流程圖節點、skill 清單列、案例列）。各檔用該語言既有 skill 列的句式翻譯 framework-build 一行；計數詞改對應語言（en: 13→14 / "fourteen skills"；ja: 「13 個」→「14 個」；zh-CN: 「十三個」→「十四個」）。

Run（逐檔核對計數一致）：`rtk grep -rn "framework-build\|skills-1[34]\|十三\|十四\|fourteen\|14 " README*.md`
Expected: 四份 README 都含 framework-build，且計數全部指向 14、無殘留 13。

- [ ] **Step 6: Commit**

```bash
rtk git add README.md README.en.md README.ja.md README.zh-CN.md
rtk git commit -m "docs: 四份 README 接入 framework-build（13→14）"
```

---

## Task 9: 更新 VERIFICATION.md 與三處交叉指引

**Files:**
- Modify: `VERIFICATION.md`
- Modify: `skills/method-design/SKILL.md`、`skills/outline-builder/SKILL.md`、`skills/lit-matrix/SKILL.md`

- [ ] **Step 1: VERIFICATION.md 證據表插一列**

在表末新增：

```markdown
| framework-build | 0.X.0 Draft | 2026-06-21 | 日台半導體經濟安全（JASM）課題、文獻已過 lit-matrix | 框架沙拉、紅海誤選、假地基、兩張皮 | examples/2026-06-21-framework-jasm.md |
```

- [ ] **Step 2: VERIFICATION.md 狀態敘述補一句**

在「目前 12 個 Boya skills 列為 Stable，`venue-fit` 列為 Beta」段落後補：

```markdown
- **framework-build**（0.X.0 新增）：目前 **Draft**——已有 1 份真實 JASM 課題定錨案例（`examples/2026-06-21-framework-jasm.md`），但理論框架定錨橫跨學科差異大，升 Beta 需再補至少 1 個不同學科（如人文思辨型「理論視角」而非社科「概念框架」）的定錨案例，確認「分學科分流」「不堆框架沙拉」「庫外框架標需補」可重複成立。
```

- [ ] **Step 3: 三處 SKILL.md 加交叉指引（DRY，各一句）**

- `skills/lit-matrix/SKILL.md` 結尾／串用處加：「文獻讀完、要把理論透鏡定下來 → 接 `framework-build` 定錨主框架，再進 `method-design`。」
- `skills/method-design/SKILL.md` 第 1 步（從研究問題推方法）前加：「方法跟著研究問題＋**理論框架**走；若框架還沒定錨，先回 `framework-build`。」
- `skills/outline-builder/SKILL.md` 第 1 步（盤點材料）的「研究方法」旁加：「（理論框架若已過 `framework-build` 定錨，骨架要長在框架的分層上。）」

- [ ] **Step 4: Commit**

```bash
rtk git add VERIFICATION.md skills/method-design/SKILL.md skills/outline-builder/SKILL.md skills/lit-matrix/SKILL.md
rtk git commit -m "docs: VERIFICATION 記錄 framework-build（Draft）並補三處交叉指引"
```

---

## Task 10: 收尾驗收與版本 tag

**Files:**
- 無新增；全倉庫一致性檢查。

- [ ] **Step 1: 全倉庫殘留檢查**

Run: `rtk grep -rn "框架與大綱\|skills-13\|十三個 skill" . --include=*.md`
Expected: 無命中（「框架與大綱」已更名、計數已全改 14）。若有命中，逐處修正後補 commit。

- [ ] **Step 2: 跑一次 eval 全綠**

Run（人工）：打開 `evals/framework-build.md`，拿 `examples/2026-06-21-framework-jasm.md` 的輸入，用 `skills/framework-build/SKILL.md` 當前版本完整跑一遍，逐條核 MUST／MUST NOT。
Expected: 8 MUST 全過、6 MUST NOT 全未犯、4 個已暴露的坑未重犯。

- [ ] **Step 3: 版本 tag（依 AGENTS.md，新 skill 升 0.X.0）**

Run: `rtk git tag` 看現況最大版本，新 skill 屬「0.X.0＝新 skill 發布」。確認目標版本號（如現況 0.3.0 → 0.4.0），於 MEMORY.md 記版本史一行，然後：
```bash
rtk git tag v0.4.0
```
（實際號以 `git tag` 現況遞增為準；未確認現況前不要硬寫 0.4.0。）

- [ ] **Step 4: 最終 commit（若 MEMORY.md 有更新）**

```bash
rtk git add MEMORY.md
rtk git commit -m "chore: framework-build 發布，版本史記入 MEMORY"
```

---

## Self-Review（計畫對 spec 的覆蓋核對）

- **spec §設計 ①命名定位** → Task 2 frontmatter `name: framework-build`、中文名「理論框架定錨」。✅
- **spec ②三模式＋觸發語** → Task 2 SKILL.md 三段（正向／輔助嵌入／逆向）、description 含三組觸發語；Task 1 eval 三模式斷言。✅
- **spec ③六鐵律** → Task 2 鐵律 1-6 逐條落地；Task 1 eval MUST/MUST NOT 對映。✅
- **spec ④正向工作流 7 步** → Task 2 工作流第 1-7 步。✅
- **spec ⑤輔助嵌入** → Task 2 輔助框架嵌入模式。✅
- **spec ⑥逆向體檢** → Task 2 逆向體檢模式 5 體檢項。✅
- **spec ⑦產出物定錨表** → Task 2 輸出格式＋Task 3 template。✅
- **spec ⑧邊界/DRY 交叉指引** → Task 9 Step 3 三處交叉指引。✅
- **spec 改動清單 1-9** → Task 1（eval）、2（SKILL）、3（template）、4（example）、5（ROUTER）、6（roadmap 重編號）、7（GUIDE）、8（README×4）、9（VERIFICATION）。✅
- **spec roadmap 重編號表** → Task 6 Step 1 八列總表。✅
- **spec §驗收** → Task 10 Step 2 eval 全綠。✅
- **spec §版號影響（新 skill 0.X.0）** → Task 10 Step 3 tag。✅

**Placeholder scan**：各 task 的待寫內容均為完整可貼文本；`<…>` 僅出現在 template 與輸出骨架（本就是給使用者填的空格），非計畫 placeholder。✅
**Type/名稱一致性**：skill name 全程 `framework-build`、中文「理論框架定錨」、產出檔 `框架定錨.md`、四欄表欄名「框架／解釋什麼／理論代價／庫存支撐」、分層「主框架→中介機制→實證抓手→落點」在 eval／SKILL／template／example 全一致。✅
```
