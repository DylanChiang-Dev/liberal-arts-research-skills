# 實測案例 001：用 citation-verify 查核作者自己的碩士論文

> 2026-06-12，citation-verify 0.0.2 的打磨輪實測。作者把工具對準自己已通過、已送存國圖的碩士論文參考文獻——47 筆全量查核，不抽樣。本頁同時作為該論文書目的**公開勘誤表（errata）**。

## 結果總覽

| 狀態 | 筆數 | 說明 |
|---|---:|---|
| ✅ 已核實 | 22 | 標題、作者、年份與權威記錄相符 |
| ⚠️ 有誤需更正 | 12 | 見下方勘誤表 |
| ❓ 待人工 | 13 | 專書、章節、學位論文、研討會論文、區域註冊 DOI 等 API 覆蓋外類型 |
| ❌ 查無此文 | 0 | 無虛構引用 |

## 勘誤表（Errata）

### 一、DOI 貼錯（DOI 可解析，但指向另一篇文章）

| 條目 | 原 DOI（誤） | 更正 DOI |
|---|---|---|
| Brooks, Longstreet, & Califf (2017), *AIS THCI* | `10.17705/1thci.00009`（實指向期刊審稿人致謝頁） | `10.17705/1thci.00091` |
| De Vries, Tummers, & Bekkers (2018), *PPMG* | `10.1093/ppmgov/gvx003`（實指向另一篇文章） | `10.1093/ppmgov/gvy001` |
| Wu & Lin (2016), *Technological Forecasting and Social Change* | `10.1016/j.techfore.2016.06.011`（實指向同刊另一篇） | `10.1016/j.techfore.2016.06.028` |

### 二、作者署名錯誤

| 原條目（誤） | 更正 |
|---|---|
| Thomas, E., & Rogers, B. E. M. (1998) | **Backer, T. E., & Rogers, E. M.** (1998)（「Thomas E. Backer」一名被誤拆為姓 Thomas） |
| Perceived usefulness, perceived ease of use... (n.d.). *MIS Quarterly* | **Davis, F. D. (1989)**. Perceived usefulness, perceived ease of use, and user acceptance of information technology. *MIS Quarterly*, 13(3), 319–340.（原條目缺作者、年份誤標 n.d.） |

### 三、出處資訊錯誤或不完整（題首無作者的網路條目）

| 原條目（誤/不全） | 更正 |
|---|---|
| A Primer on Generative Artificial Intelligence. (2023). MDPI. | **Kalota, F. (2024)**. *Education Sciences*, 14(2), 172. `10.3390/educsci14020172` |
| The impact of generative AI (GenAI)... (2024). Tandonline. | **Chiu, T. K. F. (2023)**. *Interactive Learning Environments*. `10.1080/10494820.2023.2253861` |
| Unveiling the evolution of generative AI (GAI)... (2024). *Journal of the Evolution of AI Technology* | 作者 **Akhtar**（2024），期刊實名 ***Journal of Electrical Systems and Information Technology***（原條目期刊名不存在）。`10.1186/s43067-024-00145-1` |
| Generative artificial intelligence in the metaverse era. (2023). ScienceDirect. | **Lv, Z. (2023)**. *Cognitive Robotics*. `10.1016/j.cogr.2023.06.001` |
| Preliminary evidence of the use of generative AI in health care... (2024). NCBI. | **Yim et al. (2023)**，JMIR 系列。`10.2196/preprints.52073`（建議改引正式刊出版） |
| Comparative analysis of generative AI risks in the public sector. (2024). ACM. | 第一作者 **Beltran**（2024），dg.o 會議論文。`10.1145/3657054.3657125`（補全完整作者列表後引用） |
| 引自 ResearchGate / DATAVERSITY 的條目 | ResearchGate 與聚合站不是出處，應改引原始發表版本 |

### 四、重複條目

- Goodfellow et al. (2014) 與 Vaswani et al. (2017) 在參考文獻中各出現兩次，各刪一筆。

### 待核（非錯誤，建議覆核）

- Setiyani, Effendy, & Slamet (2021)：權威記錄卷期為 3(2)，原文寫 3(1)。
- 「Generative artificial intelligence in innovation management (2024)」一筆本輪未能定位正身，補全作者篇名後複查。

## 方法與聲明

- 工具：本倉庫 `citation-verify` 0.0.2；數據源：Crossref、DataCite、OpenAlex、Semantic Scholar 公開 API。
- 查核僅確認「文獻存在且書目正確」，不評價文獻內容，也不證明引用支持原文論點。
- 以上錯誤均為書目層面的筆誤，不影響論文的論證與結論。依學術慣例以本頁公開更正；作者個人留存版已同步修正。
- 本案例同時回饋了工具本身：DataCite 回退、DOI 錯位反查、URL 內嵌 DOI 抽取、重複偵測等規則皆源自本輪實測（見 `MEMORY.md` 版本記錄）。

> 寫在最後：每本論文都有這種錯，差別只在有沒有工具把它照出來、照出來之後敢不敢公開改。這正是本技能庫的立場——**品質，不是遮掩**。
