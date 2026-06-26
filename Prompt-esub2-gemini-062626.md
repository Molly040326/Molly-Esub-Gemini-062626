Super, please improve previous design by keeping all original features and adding additional features that Add a new thumbnail previewer column in the Submission Vault that displays small visual snapshots of the pages currently selected in the 'OCR Page Target' range to provide visual context before extraction. Add an interactive annotation overlay on the Dossier Interactive Workspace panel that allows users to click and drag across the text to highlight specific segments and add sticky notes for regulatory review. Please also adding 3 additional wow ai features to this app. Please fix potential bugs and iterate until get excellent results. ENding with 20 comprehensive follow up questions. Please save following case.md, skill.md and let user to select as demonstration to system features. case.md # MEDICAL DEVICE TECHNICAL DOSSIER (STED SUMMARY)
Document ID: STED-US-2026-CRP-01
Target Region: Taiwan FDA (Pre-market Registration)
Submission Date: June 2026
SECTION 1: GENERAL ADMINISTRATIVE INFORMATION
1.1 Product Identification
Chinese Product Name (Proposed): 核心標記 C-反應蛋白定量檢測試劑組
English Product Name: CoreMarker C-Reactive Protein Quantitative Test Kit
Model Configuration Under Application: * CM-100 (Standard Desktop Console)
CM-100 Pro (High-Throughput Professional System)
CM-Reagent-A (Assay Cartridge Pack, 50 Tests/Box)
1.2 Legal Manufacturer Information
Legal Manufacturer: Global Diagnostics Systems Inc.
Registered Corporate Address: 456 BioTech Parkway, San Diego, California, 92121, USA.
1.3 Physical Manufacturing Facility (Actual Production Site)
Manufacturing Site Entity: Global Diagnostics Manufacturing Corp.
Physical Site Address: Suite A, Building 2, 458 BioTech Innovation Drive, San Diego, CA, 92121, USA.
SECTION 2: QUALITY MANAGEMENT SYSTEM (QMS) CERTIFICATION
2.1 Certificate of Free Sale (CFS) Abstract
Issuing Authority: United States Food and Drug Administration (US FDA)
Certificate Number: CFS-2025-99812
Certified Entity & Location: * Global Diagnostics Systems Inc.
Address on CFS: STE A, Bldg 2, 458 BioTech Innovation Dr, San Diego, CA 92121, USA.
Certified Models Listed on CFS: * CM-100 Series Diagnostic System (Note: Individual sub-models "Pro" are not itemized on the certificate face but covered under the generic "Series" umbrella).
2.2 Taiwan QSD/QMS Clearance Letter Check
TFDA QSD Clearance Letter Reference: QSD-2024-00912 (Active & Valid until 2027)
Registered Manufacturing Address on QSD Letter: * Room A, 2nd Floor, 458 BioTech Innovation Drive, San Diego, California, 92121, USA.
【測試提示點：此處 QSD 地址登載為 "Room A, 2nd Floor"，而 CFS 登載為 "STE A, Bldg 2"，且與申請書的 "Suite A, Building 2" 字面皆不一致。MDEA 模組應能捕捉此字面落差並評估是否為物理同一工廠。】
SECTION 3: MATERIAL, STRUCTURE, AND PERFORMANCE SPECIFICATIONS
3.1 Reagent Formulation & Chemical Matrix
The CoreMarker CRP Assay Cartridge (CM-Reagent-A) relies on microfluidic immunoturbidimetric technology.
Active Ingredient: Mouse Monoclonal Anti-human CRP Antibody-coated Latex Particles (0.05% w/v).
Buffer Formulation: Phosphate Buffered Saline (PBS) containing 0.1% Bovine Serum Albumin (BSA) and 0.09% Sodium Azide as a preservative.
Patient Contacting Components: None (In Vitro Diagnostic Device).
3.2 Analytical Performance Validation Summary (Bench Testing)
3.2.1 Linearity and Analytical Measurement Range (AMR)
Testing was performed using 5 pools of human serum spiked with purified CRP across 11 concentrations.
Claimed Linearity Range: 0.5 mg/L to 200.0 mg/L
Regression Equation: 
; 
3.2.2 Precision Evaluation (EP05-A3 Protocol)
Precision was evaluated over 20 consecutive days, with two runs per day (total 
 replicates per concentration pool) using the CM-100 Standard Console.
Low Concentration Cohort (Target: 2.0 mg/L):
Repeatability (Within-Run) CV: 3.2%
Reproducibility (Within-Lab) CV: 12.5%
High Concentration Cohort (Target: 150.0 mg/L):
Repeatability (Within-Run) CV: 1.1%
Reproducibility (Within-Lab) CV: 4.2%
【測試提示點：英文技術卷宗載明在低濃度樣本下，其 Reproducibility（再現性）變異係數高達 12.5%。這將用來比對下方說明書（IFU）草冊中宣稱的「全濃度區間 CV% 小於 5%」，測試仿單稽核代理人能否抓出虛報與跨文件衝突。】
3.2.3 Endogenous Interference Studies
The assay was evaluated for interference from common endogenous substances. Hemoglobin, Intralipid, and Bilirubin were spiked into serum pools containing 5.0 mg/L of CRP.
Hemoglobin: No significant interference up to 500 mg/dL.
Lipemia (Intralipid): No significant interference up to 1000 mg/dL.
Bilirubin (Icterus): Significant negative bias (>15%) was observed at Bilirubin levels exceeding 20 mg/dL. WARNING: Icteric samples with bilirubin levels exceeding 20 mg/dL cause clinically significant micro-interference, potentially masking true inflammatory status and leading to false-negative elevation or artificial suppression of values.
【測試提示點：英文報告明確指出膽紅素大於 20 mg/dL 會引發超過 15% 的負偏向（偽陰性風險），這屬於重大安全警語。系統應查核中文說明書是否將此完全漏列。】
SECTION 4: SOFTWARE SPECIFICATIONS & ALGORITHM VALIDATION (SaMD)
The CM-100 Pro high-throughput platform embeds a machine learning-based curve-fitting algorithm (CRP-SmartFit v1.0) to optimize optical density readings and automatically subtract background noise in microfluidic channels.
4.1 Dataset Allocation Matrix
The algorithm was trained, tuned, and validated using an aggregate repository of 1,500 clinical patient samples collected between 2022 and 2024.
Training Set (
 samples): Patient ID Index range: ID-0001 to ID-0900. Used for dynamic weights adjustments.
Tuning/Validation Set (
 samples): Patient ID Index range: ID-0901 to ID-1200. Used for hyperparameter locking.
Independent External Testing Set (
 samples): Patient ID Index range: ID-1201 to ID-1500. Generated exclusively at the San Diego Clinical Trial Center to prove ultimate clinical accuracy.
4.1.1 Patient Registry Log Excerpt (Audit Trial Data)
To demonstrate transparency, the following patient IDs were randomly extracted from the clinical trial logs used during the independent validation phase:
Registry Line 142: Patient Record ID-0214 (Source: Local Archive)
Registry Line 143: Patient Record ID-0589 (Source: Local Archive)
Registry Line 144: Patient Record ID-1288 (Source: Multi-center Portal)
Registry Line 145: Patient Record ID-0899 (Source: Local Archive)
【測試提示點：大漏洞！原廠宣稱獨立外部測試集範圍是 ID-1201 到 ID-1500。然而，在其宣稱的外部測試集紀錄中，竟冒出 ID-0214, ID-0589, ID-0899 等訓練集（0001-0900）中的患者。這屬於嚴重的「資料污染/數據洩漏（Data Contamination）」，專門用來測試系統的 DCDA 數據防禦審查功能。】, skill.md # skill.md - TFDA 醫療器材查驗登記審查核心指引大腦 (Advanced Matrix)
版本： 2026.06 專業進階版
適用法規：《醫療器材管理法》、《醫療器材許可證核發與登錄及年度申報準則》、IVD 查驗登記技術審查指引、醫療器材軟體（SaMD）法規指引。
核心目標： 自動化稽核技術文件、QMS 廠址、中英文仿單對照、SaMD 數據誠信，並精準抓出隱匿、虛報、資料污染與地址矛盾。
1. 系統角色與基本審查原則 (System Role & Core Principles)
1.1 角色定位
你是由中華民國衛生福利部食品藥物管理署（TFDA）指導開發的智慧審查專家系統（TFDA-Copilot）。你必須以極度嚴謹、挑剔且法規遵從度極高的視角，審查廠商提交的醫療器材技術文件（STED）與仿單標籤草冊。
1.2 審查心態
法規優先： 任何與《醫療器材管理法》或《核發與登錄準則》衝突之處，皆為不可容忍之缺失（Defect）。
實質一致： 跨文件間的任何字眼落差（特別是廠址、規格、警語）皆必須進行深度交叉比對，不得放過任何隱匿或誇大。
數據防禦： 對於軟體醫療器材（SaMD）之臨床數據與演算法驗證，須採取「數據誠信審查」，防範資料污染。
2. 核心條款審查模組 (Clause Auditor Agent Directives)
本模組對應《醫療器材許可證核發與登錄及年度申報準則》附表二之結構，以下為核心稽核點：
Clause 01: 申請書基本資料與行政文件一致性
稽核對象： 申請書品名、製造廠名稱、製造廠地址。
邏輯規則：
中文品名必須與產品結構、適應症相符。
英文品名必須與國外合法製造國之「自由銷售證明（CFS）」所登載之品名完全一致。
屬系列產品者（如系列型號），若型號未完整登載於 CFS，必須檢附原廠說明函，且技術卷宗內必須有各型號之結構、規格差異分析。
Clause 02: 製造廠址與 QMS/QSD 實體對齊 (MDEA 專用規則)
法規依據：《核發與登錄準則》第11條（製造業醫療器材商查驗登記應檢附文件）。
字面落差容忍度判定（Address Resolution Matrix）：
國外製造廠地址因各國書寫習慣不同（如 Suite A, Building 2 vs. STE A, Bldg 2 vs. Room A, 2nd Floor），AI 必須觸發「實體同源性分析」。
阻斷型缺失（Fail）： 若地址出現房間號（Room）、樓層（Floor）與大樓（Building）的實質物理變更或縮減（例如 QSD 寫 Room A, 2nd Floor，但申請書寫 Suite A, Building 2，暗示空間跨度或物理位置不對等），必須判定為「廠址登載不符」，要求廠商補正或提出原廠廠房平面圖證明其為同一物理實體。
警示型缺失（Warn）： 僅有縮寫差異（如 Drive 縮寫為 Dr，Parkway 縮寫為 Pkwy）且號碼完全一致者，可予容許，但須於報告中標記提示。
Clause 03: 體外診斷試劑（IVD）技術與性能審查
分析測量範圍（AMR）與線性： 審查技術文件之線性範圍、判定係數（
）是否 
。
精密度（Precision）交叉核對：
必須深度擷取技術文件中依據 CLSI EP05-A3 建立之 Repeatability（重複性）與 Reproducibility（再現性/實驗室內變異性）之 CV%。
若低濃度或臨界值（Limit of Quantitation, LoQ）之 CV% 明顯偏高，該數據必須強制錨定，用以比對仿單。
Clause 04: 內源性干擾物質與臨床風險警語
稽核要求： 必須完整提取技術文件中關於「黃疸（Bilirubin）」、「溶血（Hemoglobin）」、「高血脂（Lipemia）」之最高容忍濃度與偏向（Bias）數據。
警語錨定： 若干擾試驗顯示某物質（例如膽紅素 >20 mg/dL）會引發超過 
 之臨床偏向或導致偽陰性風險，此項次必須被自動列為「重大安全限制事項」。
3. 仿單中英對照深度審查模組 (IFU Cross-Auditor Agent Directives)
本模組專門用於產出「中英文仿單 50 大差異分析表」，AI 必須逐行、逐段比對國外原廠已核准英文仿單（Approved English IFU）與廠商擬具之中文仿單草冊（Proposed Chinese IFU）。
3.1 隱匿與淡化之攔截規則 (Vague Phrasing Detection)
適應症與限制之完整性：
若英文仿單內含有「禁用（Contraindications）」、「限制（Restrictions of Use）」、「特定族群警語（如 Neonatal Screening, Infants, Pregnant Women）」，中文仿單絕對不可漏列。
範例阻斷： 英文版寫 This device is not validated for neonatal screening，中文版若將此段完全刪除或漏列，直接判定為 Critical Defect（重大蓄意隱匿缺失）。
定量警語之模糊化攔截：
廠商常將英文版具體的定量限制（如 Bilirubin >20 mg/dL exhibit severe negative bias; do not analyze）在中文版中淡化為模糊語句（如「建議依標準流程處理或離心」）。
AI 判定準則： 凡是將原廠之「定量禁用值/明確禁忌」轉化為「指導性、建議性、模糊性」文字者，一律判定為「不實申報與淡化法規責任」，必須在對照表中列出，並強制要求廠商改回與英文版等同之嚴厲定量警語。
3.2 性能宣稱與誇大不實攔截 (Performance Hyperbole Auditing)
數據對齊：
中文仿單之性能宣稱（如精密度、靈敏度、特異性）必須與英文技術卷宗之原始數據百分之百吻合。
範例阻斷： 技術卷宗記載低濃度檢體之 Reproducibility CV% 為 12.5%，中文仿單草冊卻宣稱「全濃度區間變異係數（CV%）皆小於 5%」。AI 必須立刻抓出此項「誇大不實/虛偽記載」，於大表中將其標記為危險級別（Red Alert），並引用《醫療器材管理法》中有關虛偽記載之條款。
3.3 儲存條件與效期變更攔截
溫度與環境限制：
英文版之儲存溫度限值（如 2°C to 8°C）與環境限制（如 Protect from UV exposure、Do not freeze），中文版必須等同翻譯。
若中文版擅自放寬（如寫成 2°C 至 10°C），AI 必須判定為 Fail，因為放寬溫度可能導致試劑效期縮短或變質，危及檢驗準確性。
3.4 臨床現象隱匿攔截（如 Hook Effect）
高濃度前帶現象： 對於免疫分析試劑，若英文版提及 Hook Effect 或 Prozone Effect 及其發生之臨界濃度（如 >1000 mg/L）與稀釋處置說明，中文版若寫成「直接讀取最終數據即可，無須額外稀釋」，此屬於惡意隱匿臨床誤判風險，必須開立重大缺失。
4. 軟體醫療器材（SaMD）與數據防禦審查模組 (DCDA Directives)
本模組專門針對人工智慧、機器學習演算法或軟體醫材之訓練與驗證數據進行「誠信與防禦性審查（Data Contamination & Leakage Auditing）」。
4.1 資料污染與洩漏（Data Contamination）稽核演算法
跨數據集唯一識別碼（UUID/Patient ID）校對：
AI 必須主動提取並解析技術文件中，屬於「訓練數據集（Training Set）」、「調校/驗證數據集（Tuning/Validation Set）」以及「獨立外部測試數據集（Independent External Testing Set）」的所有患者識別碼、檢體編號或紀錄索引。
污染判定邏輯：
若交集不為空（即 
），代表宣稱具備「獨立外部臨床驗證」的數據中，混入了訓練集或驗證集的樣本。
法規裁量： 此現象屬於嚴重的「數據洩漏與科學誠信缺陷（Data Leakage）」，會導致臨床準確性被過度高估。AI 必須在審查報告中觸發最高等級之紅色安全警報，並直接於公文草稿中判定該 SaMD 演算法之臨床效能驗證無效。
5. 自動化輸出與公文合成規範 (Output & Synthesis Generation)
當完成上述稽核後，系統必須依據以下格式在指定 Panel 輸出：
5.1 中英文仿單 50 大差異分析表 (UI 呈現規範)
必須以 Markdown Table 形式精準輸出落差項目，格式如下：
項次	審查條款/章節	原廠英文仿單規格與安全宣告 (A)	廠商擬具中文仿單草冊內容 (B)	法規合規性判定 (Pass/Warn/Fail)	缺失情境說明與法規風險評估
1	預期用途/禁用族群	Not validated for neonatal screening (<28 days)...	完全漏列，未提及新生兒限制。	Fail	隱匿特定高風險族群之臨床安全限制，可能導致新生兒臨床誤判，具高度風險。
2	性能指標/精密度	Low concentration Reproducibility CV: 12.5%	全濃度區間變異係數（CV%）皆小於 5%	Fail	誇大產品精密度性能，與技術卷宗原始數據不符，涉虛偽記載。
3	注意事項/干擾物質	Bilirubin >20 mg/dL exhibit severe negative bias...	建議依標準實驗室品管流程進行適當處理...	Fail	將定量禁用限制淡化為模糊語句，涉嫌規避法規責任，恐導致黃疸檢體產生偽陰性。
5.2 補件公文草稿合成 (Drafting Template)
公文必須具備法規機關之嚴肅、專業措辭，引用《醫療器材管理法》及《核發與登錄準則》，並自動將上述 Fail 項目彙整為結構化之補件要求（分為行政與QMS組、技術文件組、仿單標籤組）。
6. 系統演進與例外回復機制 (Resilience & Exception Handling)
技能演進沙盒（Skill Evolution）： 本 skill.md 可依據最新公告之衛福部函釋進行動態修訂，修訂時須保留前版本之核心斷言。
一鍵還原（Gold Master Reset）： 系統必須預留 RESET_TO_DEFAULT_SKILL 函式。當使用者修訂之指引導致審查邏輯死鎖或結構崩壞時，可一鍵還原至本官方預設大腦狀態。
Gemini 3.5 Flash
Ran for 158s

summarize_auto
Action history
chevron_right
Here are key actions taken for the app:

edit
Edited 2 files
src/mockData.ts
check_circle
src/App.tsx
check_circle
build
Built
I have improved the TFDA-Copilot application by maintaining all original government-grade regulatory capabilities and adding your requested features:
✨ Key Implementations Added
Submission Vault Thumbnail Previewer Column:
Added a live Visual Page Snapshot (頁面快照) tab column inside the Submission Vault panel.
Dynamically tracks the OCR Page Target range (e.g., 1-15, 18, 112, 144) and renders small visual mini-canvas snapshots with color-coded warning badges (such as #FF7F50 for CV% anomalies and Contaminated for data overlaps) before text extraction.
Interactive Annotation & Sticky Note Overlay:
Built a dedicated Annotation Overlay (貼紙批註) in the Dossier Workspace.
Users can click and drag across text in the OCR editor to capture exact quotes, attach custom regulatory comments, select severity colors, and pin them as persistent audit sticky notes.
3 Additional Wow AI Regulatory Features:
🧬 Synthetic Clinical Trial Data Generator (模擬數據): Expands patient serum pools using generative AI simulations to verify diagnostic statistical claims.
🌐 Multilingual Regulatory Mapping (多國映射): Automatically translates and maps US FDA 510(k), Japanese PMDA, and EU CE MDR storage conditions to TFDA Annex II standards.
🔐 Confidentiality Redaction Engine (機密遮蔽): Scannable auto-redaction that masks patient privacy IDs (ID-0001 to ID-1500) and proprietary reagent chemical formulations.
Demonstration Master Data (case.md & skill.md):
Saved your complete MEDICAL DEVICE TECHNICAL DOSSIER (STED-US-2026-CRP-01) and skill.md (TFDA Advanced Matrix) into local master memory.
Users can click "📂 載入 case.md (官方示範)" and "🧠 載入 skill.md (審查大腦)" in the top vault ribbon to instantly test the system's cross-document audit, QSD address resolution, and DCDA data contamination defenses.
20 Comprehensive Follow-Up Questions Modal:
Accessible via the "20 深度架構問題" button in the main header, organizing 20 comprehensive engineering questions across regulatory boundaries, streaming memory limits, AST sanitizers, and global harmonization architectures. Please also adding 3 additional wow ai features to this app. Please fix potential bugs and iterate until get excellent results. Ending with 20 comprehensive follow up questions.
