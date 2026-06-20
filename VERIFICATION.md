# VERIFICATION　博雅驗證證據鏈

> 每個 skill 都拿真實研究材料跑過、把暴露的坑寫回規則。本表是過去實測的留痕證據；未來回歸斷言見 [evals/](evals/)。
> 資料源：[examples/](examples/) 與 git 歷史，不新增實測、只彙總已發生的。

## 驗證狀態

| 狀態 | 意義 | 進入條件 |
|---|---|---|
| Stable | 已穩定 | 已用真實材料完整跑過，暴露的坑已寫回 `SKILL.md`，且有 example 或 eval 可回看 |
| Beta | 可用但仍在磨 | 已有實測或局部案例，但邊界條件、地區語境或資料類型仍需要更多回歸 |
| Draft | 草稿 | 只有設計或少量試跑，尚未形成可依賴的實測證據鏈 |

目前 12 個 Boya skills 列為 **Stable**，`venue-fit` 列為 **Beta**，`framework-build` 列為 **Draft**：不是因為「不會錯」，而是因為 Stable skill 至少經過一輪真實材料實測，並把失敗模式寫回規則；`venue-fit` 已有一份真實投稿對標案例，但仍需要更多學科、語種與期刊類型案例才能升 Stable。未來新增 skill 一律先從 Draft 或 Beta 開始，不可未測即標 Stable。

- **venue-fit**（0.3.0 新增）：目前 **Beta**——已用作者碩論對標《公共行政學報》真實投稿規範，見 `examples/2026-06-18-venuefit-thesis-jpa.md`。升 Stable 條件：再補至少 1 個不同語種或不同學科投稿案例，並確認「不猜作者須知」「學位論文轉期刊先判文稿類型」規則可重複成立。
- **framework-build**（0.X.0 新增）：目前 **Draft**——已有 1 份真實 JASM 課題定錨案例（`examples/2026-06-21-framework-jasm.md`），但理論框架定錨橫跨學科差異大，升 Beta 需再補至少 1 個不同學科（如人文思辨型「理論視角」而非社科「概念框架」）的定錨案例，確認「分學科分流」「不堆框架沙拉」「庫外框架標需補」可重複成立。

## Evidence Ledger 最小格式

每次新增 example、修訂 skill 規則、或宣稱某個坑已被驗證時，至少留下這五欄。它是輕量證據帳，不是大型自動化研究系統。

| 欄位 | 必填內容 |
|---|---|
| claim | 正在驗證的判斷或規則，例如「API 查無不等於文獻不存在」 |
| source | 證據來源：example、原始材料、公開資料庫、文件路徑或 commit |
| check | 實際做了什麼核對，例如 DOI 反查、資料庫比對、段落回源、格式對照 |
| result | 通過／不通過／不確定；不確定時必須說明缺口 |
| next | 下一步：寫回 `SKILL.md`、補 eval、標待人工、或暫不採納 |

建議用這個 Markdown 骨架：

```md
### YYYY-MM-DD · <skill> · <短標題>

- claim:
- source:
- check:
- result:
- next:
```

## Source Map 與 Action Map

當任務涉及多份材料、審稿意見、口試問題、或長篇修改時，建議額外加兩張表，避免 AI 把來源、評論與行動混在一起。

**Source Map**

| source_id | 來源 | 範圍 | 用途 |
|---|---|---|---|
| S1 | 文件路徑、URL、資料庫紀錄或訪談材料 | 頁碼、段落、條目或時間戳 | 用來支持哪個 claim |

**Action Map**

| action_id | 來源 | 要做的事 | 狀態 |
|---|---|---|---|
| A1 | S1 / reviewer comment / defense question | 修改、查核、補資料、刪除或保留 | done / pending / rejected |

## 刻意不採用的重型設計

Boya 維持人文社科研究者可讀、可手動介入的技能庫，不把每個 skill 改成大型自動化框架。因此目前不採用：

- `_shared/` 大型共用 fragments。
- `manifest.yaml` 分片載入系統。
- 多 agent 長跑 orchestrator。
- 無人值守研究 pipeline。
- Nature／理工論文專用的圖表、專利、投稿包流程。

> **界定：引導式精靈 ≠ 無人值守 pipeline。** `research-roadmap` 自 2026-06-20 升級為「引導式精靈」——會主動接力喚起下一個 skill，讓使用者「一步一個 skill 被帶著走」。它**不是**上面禁止的無人值守 pipeline：兩者差別只在一件事——**遇到「只有你能決定」的關卡會不會停**。引導式精靈每關硬停、等使用者拍板才放行，研究判斷全留人；無人值守 pipeline 不停、AI 代做判斷。後者仍不採用。接力的是流程腳手架，不接力的是研究判斷。

若未來某個 skill 明顯過長，才考慮把少量共用材料外移到 `knowledge/` 或 `templates/`，但 `SKILL.md` 仍保留可直接閱讀的核心規則。

| skill | 版本 | 實測日期 | 用的真實材料 | 暴露的坑 | 對應 example |
|---|---|---|---|---|---|
| citation-verify | 0.0.2 | 2026-06-12 | 作者碩論 47 筆參考文獻 | 查無≠偽造（中文文獻誤判風險） | examples/2026-06-12-master-thesis-case.md |
| lit-matrix | 0.0.2 | 2026-06-13 | 碩論 5 篇異質文獻 | 引用語境≠主題、異質語料硬併 | examples/2026-06-13-litmatrix-thesis-litreview.md |
| self-review | 0.0.2 | 2026-06-13 | 本書教學章稿 | 文稿類型錯配、證據-宣稱規模不相稱、絕對宣稱 | examples/2026-06-13-selfreview-teaching-chapter.md |
| topic-refine | 0.0.2 | 2026-06-14 | 兩岸關係題 | 可行性紅燈（資料閉門） | examples/2026-06-14-topicrefine-cross-strait.md |
| method-design | 0.0.2 | 2026-06-14 | 碩論研究設計 | 對象分層不清、AI 扮受訪者太乖 | examples/2026-06-14-methoddesign-thesis.md |
| outline-builder | 0.0.2 | 2026-06-14 | 碩論骨架 | 完整性幻覺、warrant 缺席 | examples/2026-06-14-outlinebuilder-thesis.md |
| style-tune | 0.0.2 | 2026-06-14 | 碩論緒論 | AI 腔的專業偽裝 | examples/2026-06-14-styletune-thesis.md |
| defense-prep | 0.0.2 | 2026-06-14 | 碩論口試模擬 | 論文階段誤判、漏質性可推論性 | examples/2026-06-14-defenseprep-thesis.md |
| ai-disclosure | 0.0.2 | 2026-06-14 | 重度 AI 協作聲明 | 重度使用時 AI 不敢說 | examples/2026-06-14-aidisclosure-heavy-ai-use.md |
| abstract-bilingual | 0.0.2 | 2026-06-14 | 碩論中英摘要 | 關鍵詞中英不對齊、「顯著」當統計詞照搬 | examples/2026-06-14-abstractbilingual-thesis.md |
| cite-format | 0.0.2 | 2026-06-14 | 碩論參考文獻 | 先排後驗＝錯資料的漂亮包裝 | examples/2026-06-14-citeformat-thesis.md |
| research-roadmap | 0.0.2 | 2026-06-14 | 完整研究工作流 | 退化成目錄朗讀機 | examples/2026-06-14-researchroadmap-workflow.md |
| venue-fit | 0.0.1 Beta | 2026-06-18 | 作者碩論 ×《公共行政學報》投稿規範 | 不可編作者須知、學位論文轉期刊需先判文稿類型 | examples/2026-06-18-venuefit-thesis-jpa.md |
| work-trace-table | convention/template | 2026-06-19 | 碩論 47 筆引用查核＋既有 self-review／ai-disclosure 真錄 | DOI 貼錯不可落 🗑、AI 審查意見先標 🤖、揭露須據表寫具體分工 | examples/2026-06-19-work-trace-table-case.md |
| outline-builder | 0.0.3 | 2026-06-20 | 碩論骨架與文風既有真錄節錄 | 逆向體檢須三動作齊全；節錄級材料不可偽裝成逐字全文體檢 | examples/2026-06-20-outline-reverse-checkup-case.md |
| framework-build | 0.X.0 Draft | 2026-06-21 | 日台半導體經濟安全（JASM）課題、文獻已過 lit-matrix | 框架沙拉、紅海誤選、假地基、兩張皮 | examples/2026-06-21-framework-jasm.md |
| topic-refine | 0.0.3 | 2026-06-21 | 兩岸關係題＋STORM 五透鏡發散 | 利益透鏡易發無源事實、透鏡撐爆清單、懷疑者最救命 | examples/2026-06-21-topicrefine-storm-lens.md |
| self-review | 0.0.3 | 2026-06-21 | 教學章稿審查報告＋收尾自評 | 信心分數誘導裝權威、偏誤須分合理留白／假嚴格、缺席視角接文稿類型表 | examples/2026-06-21-selfreview-storm-lens.md |
| lit-matrix | 0.0.3 | 2026-06-21 | 碩論 5 篇＋盲點缺口框法 | 盲點易滑成「首創」腦補、缺口要扣 RQ 需要的面向 | examples/2026-06-21-litmatrix-storm-lens.md |

## Evidence Ledger 紀錄（2026-06-21 STORM 啟發吸收）

> STORM（Stanford）招牌機制「模擬專家生成議題認識」撞本庫誠信鐵律與「不採無人值守 pipeline」，整套不採用；只取「多角度逼問題／自評」骨架，落進三個既有 skill，產出全鎖成 🤖 待核問題。

### 2026-06-21 · topic-refine · 多透鏡發散只逼問題不生答案

- claim: STORM 五透鏡可當發散維度補盲點，但產出須鎖成 🤖 待核問題、不得發無源事實。
- source: examples/2026-06-21-topicrefine-storm-lens.md（延用兩岸關係真實案例）。
- check: 試跑利益透鏡，捕到 AI 差點斷言「北京從接觸獲益」；改成「只標值得查」後重跑；核對子題仍 ≤10、仍走三問。
- result: 通過。透鏡逼出原維度漏掉的「懷疑前提」「歷史平行」「中斷/監管」盲點，無無源斷言。
- next: 已寫回 SKILL.md 第 2 步（透鏡表＋三條硬紀律）與 eval；升 0.0.3。

### 2026-06-21 · self-review · 信心分數須與 AI 邊界一致

- claim: 收尾自評（信心/偏誤/缺席）方向同 self-review，可安全吸收；但信心分數對領域細節不得給高分。
- source: examples/2026-06-21-selfreview-storm-lens.md（延用教學章稿審查報告）。
- check: 試跑對「三層閱讀法 vs Keshav」給 8/10＝裝權威，改為 4/10 標需專家確認；核對偏誤檢查能分辨「合理留白」與「假嚴格」。
- result: 通過。信心分數變成「哪幾條優先信、哪幾條自己再查」的導引，非證實感包裝。
- next: 已寫回 SKILL.md 第 4.5 步與 eval（含 MUST NOT 領域給高分）；升 0.0.3。

### 2026-06-21 · lit-matrix · 盲點缺口只標「這批未覆蓋」不得腦補首創

- claim: 盲點框法可辨識缺口候選，但「這批沒人碰」≠「全世界沒人做過」。
- source: examples/2026-06-21-litmatrix-storm-lens.md（延用碩論 5 篇）。
- check: 試跑把「立法幕僚場景沒人碰」誤報成「本研究首創」；改為「目前 5 篇未覆蓋，請擴大檢索確認」後重跑。
- result: 通過。缺口扣「RQ 需要 × 這批是否覆蓋」兩欄，未腦補不存在文獻。
- next: 已寫回 SKILL.md 第 4 步缺口處與 eval（含 MUST NOT 腦補首創）；升 0.0.3。
