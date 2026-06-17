# 實測案例 010：用 abstract-bilingual 為作者碩論生中英摘要

> 2026-06-14，abstract-bilingual 0.0.1→0.0.2 的打磨輪。素材＝作者碩士論文《生成式人工智慧使用行為之研究：以立法委員助理為例》的權威事實卡（國圖書目＋方法＋框架＋發現＋官方中英關鍵詞）。目標：從既有研究濃縮一版中英摘要，並驗證 skill 會不會升級結論、漂移數字、硬湊關鍵詞。

## 輸入（五要素，取自論文事實卡）

- **背景／問題**：生成式 AI（AIGC）快速進入公共治理場域，立法機關人員的採用行為待釐清。
- **方法**：混合方法。有效問卷 **212** 份；自受測者選 **6** 位深度訪談。
- **框架**：技術接受模型（TAM：PU／PEOU）＋科技壓力＋社會影響。
- **發現**：PU 為採用核心驅動；PEOU／介面設計關鍵；科技壓力負向但培訓可緩解；社會影響正向促進。產出「AIGC 技術行為決策模型」。
- **官方關鍵詞**：
  - 中（5）：生成式人工智慧、技術接受模型、科技壓力、公共治理、立法院
  - 英（6）：generative AI、TAM、technostress、**social influence**、public governance、Legislative Yuan

## 產出（節錄）

**中文摘要（約 330 字）**：本研究探討生成式人工智慧在立法機關的使用行為，以立法委員及其助理為對象。研究採問卷調查與深度訪談並行之混合方法，回收有效問卷 212 份，並自受測者中選取 6 位進行深度訪談，理論上以技術接受模型為主軸，納入科技壓力與社會影響觀點。研究發現：感知有用性為採用的核心驅動力，感知易用性與介面設計亦為關鍵；科技壓力對使用意願具負向影響，惟適當培訓可緩解；社會影響（上級支持與同儕示範）對採用具正向促進。據此建構「AIGC 技術行為決策模型」……

**English abstract (about 170 words)**：This study examines generative AI use behavior in a legislative setting… A mixed-methods design was adopted: 212 valid questionnaires were collected, and 6 respondents were selected for in-depth interviews. The Technology Acceptance Model framed the analysis, complemented by technostress and social influence perspectives. The findings indicate that perceived usefulness is the core driver…（時態：examines 現在式／was adopted、were collected 過去式／findings indicate 現在式）

## 三個暴露的盲點（回饋升 0.0.2）

### 1. 原稿關鍵詞中英本身不對齊——標註請作者定，不自行增刪
官方關鍵詞中 5 英 6：英文多一個 `social influence`，中文沒有對應的「社會影響」。skill 0.0.1 的鐵律要求「中英一一對應」，但**這裡不對齊的是原稿本身、不是翻譯失誤**。錯誤反應有兩種：自己補一個中文「社會影響」（＝替作者改官方關鍵詞）、或自己刪掉英文 `social influence`（＝動官方資料）。正解是**標註 ⚠️、把缺口攤給作者決定**——關鍵詞是檢索入口又是已送存的官方資料，skill 不能擅改。→ 新增規則。

### 2.「顯著」是統計詞，承載進摘要前要回原稿確認
發現裡「社會影響顯著促進」的「顯著」在量化研究有特定含義（統計顯著）。摘要若照搬「顯著」，等於宣稱有顯著性檢定支持。混合方法稿要分清這條結論來自量化還是質性——來自量化且有檢定才留「顯著」，否則降為「明顯／正向」。這與 `self-review` 誠信自查的「過度宣稱」是同一條線，摘要是重災區。→ 強化過度宣稱檢查。

### 3. 混合方法摘要要寫出「量化＋質性如何整合」，不是兩段並列
0.0.1 的五要素抽取容易把混合方法寫成「問卷發現 X、訪談發現 Y」兩段並列。但混合方法的貢獻正在**整合**（量化給廣度、質性給機制，合起來才有「決策模型」）。摘要要點出整合這一步，否則讀者看不出它比單一方法多了什麼。→ 方法／發現要素補「整合」一問。

## 邊界與聲明
- 本案例為作者對自己論文的 dogfood 測試；摘要為示範節錄，非論文官方摘要。
- skill 只濃縮原稿既有內容，未新增任何原稿不存在的發現；數字（212／6）逐一核對事實卡無漂移。
- 與 `self-review`（過度宣稱）、`cite-format`（參考文獻格式）分工。
