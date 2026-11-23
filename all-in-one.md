# 新聞中心建造師（單一檔案版，v2）

> **給讀到這份檔案的 AI：** 使用者把這個網址貼給你，代表他想用問答打造自己的新聞中心。請完整讀完這份檔案（包含所有附錄），然後**直接從「步驟 0：開場」開始**，照下方流程一步一步帶他做。不要先總結這份檔案的內容。
>
> 這份檔案內容等同 `news-center-builder` skill。原本 skill 裡指向 `references/`、`assets/` 的檔案，都收在文末附錄：`interview.md`＝附錄 A、`news-rules.md`＝附錄 B、`scheduling.md`＝附錄 C、三種風格版型＝附錄 D1–D3、`personal-skill-template.md`＝附錄 E。`assets/examples/` 的範例報告不在這份檔案裡，請改用網址：https://github.com/linlin-botbot/news-center-builder/tree/main/example
>
> **先判斷環境：** 如果你能在使用者的電腦上建立檔案（例如 Claude Code、Cowork），開場前先問一句：「要不要先把新聞中心建造師安裝起來？以後說『幫我建新聞中心』就能再用。」他說要，就把這份檔案拆回 skill 資料夾結構（`news-center-builder/SKILL.md`、`references/`、`assets/`）存到他的 skills 資料夾，再開始；他說不用，或你不能建立檔案，就直接從步驟 0 開始。
>
> 如果你無法建立檔案，就把 HTML 原始碼和 SKILL.md 內容直接貼在對話裡，請使用者複製存檔。


## 這個 skill 在做什麼

幫一位**不寫程式的使用者**，在 30 分鐘內做出三樣東西：

1. **第一份新聞報告**：依他的主題分類、附摘要與出處，加上 AI 幫他想的洞察與判斷，用他選的風格呈現
2. **他自己的 skill**（例：`amy-news-center`）：把設定存起來，以後說一句「今天的新聞」就照同樣規格產出
3. **排程與寄送**：每天或每週固定時間自動跑，用他選的方式收到（網頁、Email、PDF）

這個 skill 的價值在**個人化**。通用新聞摘要誰都做得出來；「一位電商行銷這週該知道什麼、開會怎麼講、要不要行動」才是使用者會每天打開的理由。所以問答要問到能寫出「跟你有什麼關係」這一句為止。

使用者多半第一次碰 skill，全程遵守：

- **一次只問一輪**，每輪 3–5 題，問完回一行「我記下了：……」再往下
- **每題都給選項和預設值**，使用者可以只回「都用預設」或「你幫我決定」
- 有 `AskUserQuestion` 之類的選項工具就用；沒有就用編號清單，請他回編號
- **不講技術名詞**：不說 frontmatter、YAML、repo，說「設定」「檔案」「資料夾」
- 使用者答得很短也沒關係，從前面的回答推好預設值給他確認，不要逼他想

---

## 執行流程

### 步驟 0：開場（一段話就好）

> 嗨！我會用 6 輪問題幫你打造專屬新聞中心，大約 10 分鐘。每題都有預設選項，不確定就選「你幫我決定」。做完你會拿到：第一份新聞報告、一個你自己的新聞 skill、自動收報的設定方法。我們開始吧 👇

### 步驟 1–6：六輪問答

題目、選項、預設值與追問規則全部在 `references/interview.md`，照順序問，**不要調換順序**：先問用途，再問內容，最後才問長相與寄送。

| 輪次 | 主題 | 為什麼要問 |
|---|---|---|
| 1 | 你是誰、拿來做什麼 | 身分與用途決定報告結構，也決定後面幾輪的預設值 |
| 2 | 要看哪些新聞 | 目的、主題、廣度、深度、來源清單：帶他自己定義新聞從哪來、看多寬、看多深 |
| 3 | AI 要幫你想什麼 | 洞察（解讀）與判斷（篩選、決定），並告訴他事實查證規則 |
| 4 | 報告長什麼樣 | 簡約／報紙／雜誌／自訂，份量、語氣、名稱 |
| 5 | 什麼時候、怎麼收到 | 頻率、時間、時區、涵蓋多長時間、網頁／Email／PDF、用哪個 AI 工具 |
| 6 | 要不要分享 | 分享對象、版本、保密，必要時回頭調語氣與份量 |

**第 2 輪的主題要先幫他想**：根據第 1 輪的職業，主動列出 6–8 個具體主題讓他勾選。

**第 4 輪問風格時，能給他看範例就給**：`assets/examples/` 有同一份新聞的三種風格範例（`example-minimal.html` 簡約、`example-times.html` 報紙、`example-magazine.html` 雜誌），可以直接提供檔案讓他打開比較。

### 步驟 7：設定卡確認

六輪問完，整理成一張設定卡讓使用者確認：

```
📰 〔新聞中心名稱〕設定卡
━━━━━━━━━━━━━━━━━━━━━━
👤 你是：〔稱呼〕／〔職稱・產業〕／負責〔市場〕
🎯 看完拿去：〔用途〕
🧭 目的：〔跟上趨勢／盯特定對象／找素材／支援決策／風險預警〕
📌 主題：①… ②… ③…
🔍 特別追蹤：〔關鍵字・競品〕　🚫 不要：〔排除〕
↔️ 廣度：〔只看本業／本業＋上下游／本業＋相鄰產業／跨產業〕；地區〔…〕，國外新聞〔呈現方式〕
↕️ 深度：〔快訊／重點／深度／混合〕，〔要／不用〕追第一手資料
📚 來源清單：第一手〔…〕；媒體〔…〕；社群只當線索；排除〔…〕
🗞 份量：〔N 則・約 N 分鐘〕，涵蓋〔時間範圍〕
🧠 洞察：〔趨勢／影響／…〕　⚖️ 判斷：〔重要性／要不要行動／…〕
✍️ 跟你有什麼關係＋〔有／無〕今日三重點，語氣〔…〕
🎨 風格：〔簡約／報紙／雜誌／自訂：色票…〕
⏰ 收報：〔頻率〕〔時間〕（〔時區〕），用〔工具〕
📬 收件：〔網頁／Email／PDF〕
👥 分享：〔只有我／給誰、什麼版本、哪些不能外流〕
🔎 查證規則：連結都打開過、重要新聞兩個來源或第一手、數字核對原文、社群只當線索、推測要標示、不編新聞
━━━━━━━━━━━━━━━━━━━━━━
這樣可以嗎？要改哪一項直接說。
```

使用者說可以才往下。

### 步驟 8：產出第一份報告

依 `references/news-rules.md` 搜尋、篩選、撰寫，**不可跳過這份規則**，它是新聞中心可信度的底線。重點：

1. **只寫這次真的搜尋到的報導**，每則都要有能點開的原文連結；搜不到就少給幾則並說明，絕對不要編造新聞、數字或連結
2. **檢查發布日期**，超過設定的時間範圍就不收
3. **照「事實查證」七條跑**：來源分層、交叉查證、數字核對、日期、連結都打開過、事實／引述／推論分清楚、頁尾寫查證說明
4. **洞察與判斷要能指回某一則新聞**，推論一律標示「推測」或用「可能」

套版：
- 依風格選版型：`assets/style-minimal.html`（簡約）／`assets/style-times.html`（報紙）／`assets/style-magazine.html`（雜誌）。自訂就選他指定的版型當底，只改 `:root` 色票
- 版型開頭的註解寫了怎麼套、使用者沒勾的區塊怎麼刪
- 存成 `〔新聞中心名稱〕_YYYY-MM-DD.html`
- 選了 PDF 或 Email：照 `references/scheduling.md` 第一節處理

交付後問一次：「看起來如何？想調整內容、份量或風格嗎？」最多再調兩輪，避免課堂上卡太久；之後的微調告訴他隨時可以說「幫我改新聞中心設定」。

### 步驟 9：打包成他自己的 skill

1. 決定 skill 名稱：用使用者的英文名或暱稱拼音，格式 `xxx-news-center`（只能小寫英文、數字、連字號，不能有中文或空格）。沒有英文名就用 `my-news-center`
2. 複製 `assets/personal-skill-template.md`，把所有 `{{…}}` 換成設定卡的內容，存成資料夾裡的 `SKILL.md`
3. 把他**確認過的那份版型**（已換好顏色、刪好不要的區塊，新聞內容換回 `{{…}}`）存成資料夾裡的 `template.html`
4. 資料夾結構：

```
xxx-news-center/
├── SKILL.md
└── template.html
```

5. 壓成 `xxx-news-center.zip` 給他下載，並附安裝步驟（依他用的工具，見 `references/scheduling.md`）

打包前自我檢查：`SKILL.md` 的 `name` 跟資料夾名稱一致、`description` 裡有「今天的新聞」「我的新聞中心」等叫用語、沒有殘留 `{{`。

### 步驟 10：設定排程、寄送與分享

依第 5、6 輪的回答，照 `references/scheduling.md` 給他：
- 一段**可以直接複製貼上的排程指令**（已填好時間、時區、skill 名稱、寄送方式）
- 3–5 步的設定方法
- 有分享需求時，第三節對應的做法

**工具做不到的事要老實說**，退回「打開說一句『今天的新聞』」。不要讓 AI 自動寄信給使用者以外的人。

收尾：

> 🎉 完成了！你現在有：
> 1. 第一份新聞報告
> 2. 你的專屬 skill：`xxx-news-center`
> 3. 〔頻率〕〔時間〕自動送到〔管道〕
>
> 以後想改主題、加關鍵字、換風格，直接跟我說「幫我改新聞中心設定」就好。

---

## 使用者中途常見狀況

| 狀況 | 怎麼處理 |
|---|---|
| 「我不知道要看什麼主題」 | 依職業直接給 3 個建議主題，請他先用，之後再改 |
| 答案很模糊（「科技相關」） | 追問一次給選項：「是 AI 工具、半導體、手機新品，還是新創募資？」 |
| 想看太多主題（>6 個） | 建議先挑 3–5 個；主題太多每類只剩 1–2 則，反而沒重點 |
| 說不出要看什麼來源 | 依目的推薦三層來源各 2–3 個，請他先用，看幾期再增減 |
| 想看 PTT、Dcard、Threads 的討論 | 可以收當線索，但要找到媒體或第一手報導才放進報告 |
| 問「AI 查過就一定對嗎？」 | 不一定。教他每份抽查 2 則原文，錯了直接回報（見 news-rules.md「使用者端的驗證」） |
| 分不清洞察和判斷 | 「洞察是幫你解讀這件事代表什麼；判斷是幫你決定要不要看、要不要做」 |
| 洞察和判斷全部都要 | 可以，但提醒報告會變長；建議依用途對照表先選 2–3 項 |
| 想看付費牆媒體（WSJ、日經） | 說明可能只抓得到標題與導言，摘要會比較短 |
| 要寄給同事或客戶 | 建議先寄給自己再轉寄；不設定 AI 自動寄給別人 |
| 環境不能建檔或沒有網路搜尋 | 說明需要開啟「網路搜尋」與「程式碼執行與檔案建立」，告訴他在設定哪裡打開 |
| 想要投資建議 | 新聞中心只整理新聞，不給買賣建議；可保留「市場動態」主題 |

---

## 附錄 A：六輪問答題庫

### 六輪問答題庫（v2）

每輪問完回一行「我記下了：……」再進下一輪。標 ⭐ 的是預設值；使用者說「你幫我決定」就用預設，或依他前面的回答推一個更合適的值。

問的順序有道理，不要調換：**先問用途，再問內容，最後才問長相與寄送**。用途決定報告結構，長相只是外殼。

```
第 1 輪 你是誰、拿來做什麼   → 決定「跟你有什麼關係」怎麼寫
第 2 輪 要看哪些新聞         → 決定目的、廣度、深度、來源
第 3 輪 AI 要幫你想什麼      → 決定洞察與判斷
第 4 輪 報告長什麼樣         → 決定風格、份量、語氣、名稱
第 5 輪 什麼時候、怎麼收到   → 決定頻率、時間範圍、管道
第 6 輪 要不要分享           → 決定版本與保密，必要時回頭微調
```

---

#### 第 1 輪：你是誰、拿來做什麼

1. **怎麼稱呼你？**（報告會用這個名字跟你打招呼，例：「早安，Amy」）
2. **你的職稱、產業和主要負責的事？**
   - 選項：行銷／業務／主管或老闆／產品或企劃／顧問或講師／學生／自訂
   - 追問一句：「你平常在忙什麼？」（例：「負責公司的社群和電子報」）
3. **你負責哪個市場或地區？**（選填，例：台灣、日本、東南亞、全球）
4. **看完這份新聞，你會拿它去做什麼？**（可複選）**這題最重要**
   - ⭐ 開會或向主管報告
   - 寫內容（貼文、部落格、電子報、簡報的題材）
   - 盯競爭對手
   - 找客戶話題（跟客戶聊天、拜訪前準備）
   - 做決策（要不要投入、要不要調整做法）
   - 自己學習、跟上產業
   - 自訂

> 💡 第 4 題決定後面三輪的預設值（見第 3 輪的對照表），也決定「跟你有什麼關係」那一句的角度：
> 報告 → 「開會可以這樣講……」；寫內容 → 「可以寫成……」；客戶話題 → 「跟客戶聊時可以這樣切入……」；決策 → 「這可能影響你的……」

---

#### 第 2 輪：要看哪些新聞（目的、廣度、深度、來源）

這一輪要帶使用者自己**定義新聞從哪裡來、看多寬、看多深**，不是只丟幾個關鍵字。開場先講一句：

> 「同樣是看 AI 新聞，想跟上趨勢的人和要做採購決策的人，該看的來源和深度完全不同。我們先講清楚目的，再決定看多寬、看多深、從哪裡看。」

**先依第 1 輪的職稱與產業，主動提 6–8 個具體主題讓他勾。** 範例：

| 職業 | 建議主題 |
|---|---|
| 行銷 | AI 行銷工具、社群平台更新（Meta／LINE／Threads）、品牌案例、廣告投放趨勢、SEO 與 AI 搜尋 |
| 業務 | 主要客戶的公司新聞、競品消息、產業景氣、展會與標案 |
| 主管或老闆 | 產業政策與法規、競品動態、人才與薪資、景氣與匯率 |
| 科技業 | AI 模型與工具、半導體、雲端、資安、新創募資 |
| 醫療相關 | 健保政策、醫療法規、醫療科技、醫美產業 |
| 電商 | 平台政策與檔期、物流與金流、消費趨勢、廣告成本 |

1. **這些新聞要幫你達成什麼？（目的）** 承接第 1 輪的用途，再具體一層
   - ⭐ 跟上趨勢：知道產業在發生什麼
   - 盯特定對象：競品、客戶、主管機關的動作
   - 找素材：寫貼文、做簡報、跟客戶聊天
   - 支援決策：要不要投入、要不要調整
   - 風險預警：法規、輿論、供應鏈出狀況
2. **想追哪些主題？**（建議 3–5 個，最多 6 個）
   - 主題太大要追問一層：「AI」→「是 AI 工具、AI 法規，還是 AI 在你產業的應用？」
   - 追問：一定要追的公司、人物、競品、關鍵字？不想看的？
3. **要看多寬？（廣度）**
   - 只看本業
   - ⭐ 本業＋上下游（供應商、客戶所在的產業）
   - 本業＋相鄰產業（例：醫美也看美妝、保健食品）
   - 跨產業大趨勢（例：所有產業的 AI 應用）
   - 追問地區與語言：⭐台灣為主加國際重要新聞／只看台灣／台灣＋指定國家／國際為主；國外新聞⭐翻成繁中摘要、標題保留原文／全中文／保留原文
4. **每則要看多深？（深度）**
   - 快訊：一句話知道發生什麼
   - ⭐ 重點：3 個重點＋跟你有什麼關係
   - 深度：背景、各方說法、數字、可能影響
   - 混合：最重要的 3 則深度，其他快訊
   - 追問：「重要的新聞，要不要追到第一手資料？」（官方公告、財報、法規原文、研究報告）⭐要，附在該則下面
5. **從哪裡看？（來源清單）** 依他的產業，把來源分三層各推薦 2–3 個讓他勾、補：

| 層級 | 是什麼 | 怎麼用 | 例（以台灣行銷為例） |
|---|---|---|---|
| 第一手 | 官方公告、政府機關、公司新聞稿與財報、研究機構 | 事實以這層為準 | Meta 官方部落格、經濟部統計、上市公司重大訊息 |
| 媒體 | 主流媒體、產業專業媒體 | 主要新聞來源 | 經濟日報、數位時代、Social Media Today |
| 社群與意見 | 社群貼文、論壇、KOL、部落格 | **只當線索**，要找到上兩層的報導才收 | Threads、PTT、Dcard、個人電子報 |

   - 追問：有沒有一定要看的來源？一定要排除的？（例：內容農場、特定立場媒體）
   - 他說「你幫我決定」：依目的推薦（見下表），並告訴他之後可以隨時增減

**目的 → 預設廣度、深度、來源重點**（使用者說「你幫我決定」時用）

| 目的 | 廣度 | 深度 | 來源重點 |
|---|---|---|---|
| 跟上趨勢 | 本業＋相鄰產業 | 重點 | 媒體為主 |
| 盯特定對象 | 只看本業 | 重點，對象的新聞給深度 | 對象的官方公告＋媒體 |
| 找素材 | 跨產業大趨勢 | 快訊或重點 | 媒體＋社群線索 |
| 支援決策 | 本業＋上下游 | 深度，追第一手 | 第一手＋專業媒體 |
| 風險預警 | 本業＋上下游 | 重點，追第一手 | 政府機關＋第一手 |

> 💡 這一輪結束時，用一句話跟他確認：「你要的是〔目的〕，看〔廣度〕，每則〔深度〕，以〔來源重點〕為主。對嗎？」

---

#### 第 3 輪：AI 要幫你想什麼

開場先講一句，讓使用者分清楚兩題：

> 「洞察」是幫你**解讀**：這件事代表什麼。「判斷」是幫你**篩選和下決定**：這件事重不重要、要不要行動。

1. **除了摘要，要 AI 幫你多想哪一步？（洞察，可複選）**
   - 趨勢：這段時間大家都在講什麼、跟上次比有什麼變化
   - 影響：對我的產業、公司或客戶有什麼影響
   - 競品動態：對手做了什麼、我們要不要回應
   - 機會：可以拿來做什麼內容、提案或產品
   - 風險：法規、輿論、市場要注意的地方
   - 一句話結論：今天最重要的一件事
2. **要 AI 幫你下哪些判斷？（可複選）**
   - 重要性：每則標「高／中／低」，高的排前面
   - 要不要行動：標出「需要你處理」的新聞，附建議的下一步
   - 可信度：標出傳聞、單一來源、廠商業配或新聞稿
   - 跟我有沒有關：不相關的直接不放
   - 好消息還是壞消息：對我們是正面還是負面
3. **要不要在最上面放「今日三重點」？**（30 秒看完最重要的事）⭐ 要／不用

**用途 → 預設勾選對照表**（使用者說「你幫我決定」時用）

| 第 1 輪第 4 題的用途 | 預設洞察 | 預設判斷 |
|---|---|---|
| 開會或報告 | 一句話結論、影響 | 重要性 |
| 寫內容 | 趨勢、機會 | 跟我有沒有關 |
| 盯競爭對手 | 競品動態、影響 | 重要性、要不要行動 |
| 找客戶話題 | 影響、機會 | 跟我有沒有關 |
| 做決策 | 影響、風險 | 重要性、要不要行動、可信度 |
| 自己學習 | 趨勢 | 可信度 |

**固定規則：事實查證（不用問，一律套用，但要跟使用者說一聲）**

用一句話介紹：「每一份報告都會照查證規則跑，你不用設定，但要知道有這些規則。」完整規則在 `news-rules.md` 的「事實查證」一節，跟使用者講這五條就好：
1. 每則附媒體名稱、發布日期、原文連結，連結都實際打開過
2. 重要性高或要你行動的新聞，至少兩個獨立來源，或一個第一手來源
3. 數字回到原文核對，寫清楚基準（跟平日比、跟去年比）
4. 社群、論壇只當線索，找不到媒體或官方的報導就不收
5. AI 的推測一律標「推測」；找不到可靠來源就不放，絕不編造

---

#### 第 4 輪：報告長什麼樣

1. **想要哪種風格？**（有預覽圖工具就給他看三張範例；沒有就用一句話描述）

| 選項 | 感覺 | 適合 | 版型檔 |
|---|---|---|---|
| ⭐ 簡約版 | 白底、清楚分段，像工作文件 | 每天快速掃、轉給同事 | `assets/style-minimal.html` |
| 報紙風（Times 風格） | 襯線字、雙欄、頭版大標，黑白為主 | 喜歡讀報感、週報 | `assets/style-times.html` |
| 雜誌風 | 封面故事大標、色塊、卡片 | 想看起來精緻、給主管或客戶 | `assets/style-magazine.html` |
| 自訂 | 用公司品牌色或自己的喜好 | 要對外分享、要有品牌感 | 從三種版型選一個當底，再換色票 |

2. **選「自訂」才追問**（一次一題）：
   1. 主色和輔色？給色碼（如 `#00A0E9`）、描述（「深藍＋金色」），或上傳公司 logo 讓 AI 取色
   2. 要用哪一種版型當底？簡約／報紙／雜誌
   3. 字體偏好？黑體（現代）／明體（正式）／不指定
   4. 要放 logo 或固定的報告抬頭嗎？
   5. 有沒有喜歡的參考？附截圖，AI 只借風格、不抄內容

   自訂配色規則：主色用他給的；底色用主色極淡的版本或白色；文字用深灰；**文字和底色對比至少 4.5:1**，不夠就把文字加深，並跟他說一聲。

3. **一份報告想花多久看完？（份量）**
   - 3 分鐘：精選 5 則
   - ⭐ 5 分鐘：8 則，分類＋重點
   - 10 分鐘：深度版，3 則深度＋其他快訊
   - 自訂則數
4. **語氣？**
   - ⭐ 專業清楚
   - 輕鬆口語，像朋友跟你說
   - 精準簡短，像主管簡報
5. **幫新聞中心取個名字？**（例：「Amy 的晨間情報」「每週行銷雷達」）⭐ 預設「〔稱呼〕的新聞中心」

---

#### 第 5 輪：什麼時候、怎麼收到

1. **多久收一次？**
   - ⭐ 每個工作天
   - 每天（含週末）
   - 每週一次（選星期幾）
   - 我想看的時候再叫它（不排程）
2. **幾點收到？**
   - 上班前 07:30
   - ⭐ 上班時 09:00
   - 午休 12:00
   - 下班前 17:00
   - 自訂
   - 追問：時區（跨國工作的人要問，例：台灣時間或日本時間）
3. **每次涵蓋多長時間的新聞？** ⭐ 跟頻率一樣（每天就看過去 24 小時，星期一自動含週末；每週就看過去 7 天）／自訂
4. **怎麼收到？**（可複選，第一個選的是主要管道）
   - ⭐ 單頁網頁（HTML 檔，雙擊就能打開，也能分享）
   - Email（寄到自己的信箱）
   - PDF（好存檔、好轉寄、好列印）
   - 在 AI 對話裡直接看文字版
   - 自訂（例：存進 Notion、Google 文件；傳到 LINE、Slack、Teams）
   - 選 Email：問收件信箱。**只寄到使用者自己的信箱**；要寄給別人，到第 6 輪處理
   - 選自訂管道：說明要另外連接那個服務，做不到就先用 HTML 或 Email，不要硬答應
5. **你平常用哪個 AI 工具？**（排程從這裡設）⭐ Claude／ChatGPT／Gemini

> 工具做不到的事要老實說。例如沒有排程功能、沒有連接 Email 的工具，就改成「每天打開說一句『今天的新聞』就產出」。各工具做法見 `scheduling.md`。

---

#### 第 6 輪：要不要分享

1. **這份報告給誰看？**
   - ⭐ 只有我
   - 團隊同事（幾個人、什麼角色）
   - 主管
   - 對外給客戶或合作夥伴
2. **選了分享才追問：**
   - **每個人看的版本一樣嗎？**（例：主管只看三重點、團隊看完整版）
   - **怎麼給他們？**
     - ⭐ 我收到後自己轉寄或轉貼（最簡單，也最安全）
     - 把 HTML 或 PDF 放到共用雲端資料夾
     - 把我的新聞中心 skill 分享給同事，讓他們自己跑（每個人可以再改成自己的版本）
   - **有沒有不能外流的內容？**（例：內部數字、客戶名稱、未公開計畫）對外版本一律不放
3. **回頭確認**：分享對象會影響語氣和份量。對外或給主管時，問一句：「要不要把語氣改成『精準簡短』、份量改成 3 分鐘版？」

---

## 附錄 B：新聞搜尋與撰寫規則

### 新聞搜尋與撰寫規則

新聞中心只要被抓到一次假新聞或壞連結，使用者就不會再信它。以下每條都是底線。

#### 事實查證（每次都跑，不能跳過）

新聞中心的價值建立在「你可以相信它」。以下每一條都是底線，做不到的那則就不收。

##### 1. 來源分三層

| 層級 | 包含 | 規則 |
|---|---|---|
| 第一手 | 官方公告、政府機關、公司新聞稿與財報、法規原文、研究機構報告 | 事實以這層為準；有第一手就附上連結 |
| 媒體 | 主流媒體、產業專業媒體 | 主要新聞來源；同一事件優先選有採訪、有引述的報導 |
| 社群與意見 | 社群貼文、論壇、KOL、個人部落格、內容農場 | **只當線索**：要找到上兩層的報導才收，並以那篇為主連結 |

使用者在第 2 輪設定的「必看來源」優先搜；「排除來源」一律不收。

##### 2. 交叉查證
- **重要性「高」或標了「需要行動」的新聞**：至少兩個彼此獨立的來源（不是互相轉載），或一個第一手來源
- 只找到一個來源：可以收，但可信度標「單一來源」，不能標重要性「高」
- 多家媒體說法不同：寫出差異（「A 報導 30%，B 報導 25%」），不自己挑一個

##### 3. 數字
- 每個數字都回到原文核對，照原文寫，不換算、不推估、不四捨五入成更好看的數字
- 寫清楚**基準**：跟平日比、跟去年同期比、累計還是單月
- 平台或公司自己公布的數字，可信度標「自行公布」

##### 4. 日期
- 分清楚「發布日期」和「事件日期」；舊聞被轉載成新文章的，以最早的報導日期為準，超出範圍就不收
- 人物職稱、公司人事會變：照報導當時的寫法，必要時加上「（截至 X 月 X 日）」

##### 5. 連結
- **每個連結都要實際打開過**，確認內容、日期跟摘要一致；打不開的不收
- 付費牆只看得到開頭時，標示「付費內容，摘要依公開段落」
- 不用搜尋結果頁、短網址或轉址連結當原文連結

##### 6. 分清楚事實、引述和推論
- 事實：來源寫的事，照寫
- 引述：某人說的話，寫明是誰說的（「業者宣稱」「XX 表示」），不寫成事實
- 推論：AI 自己的判斷，一律標「推測」或用「可能」「可以留意」

##### 7. 查證說明（放在報告最下面）
每份報告頁尾加一行：「本期 N 則：交叉查證 N 則、第一手 N 則、單一來源 N 則；排除 N 則（原因）」。讓使用者一眼知道這份報告有多可靠。

---

#### 搜尋

1. **每個主題至少搜 2 次**：一次用中文關鍵字、一次用英文或換個說法（有國外新聞時英文必搜）。追蹤關鍵字各自單獨搜一次
2. 搜尋字串加上日期或「最新」「今天」，例如「LINE 官方帳號 2026年11月」
3. **照設定的廣度搜**：只看本業就不搜上下游；有「上下游」「相鄰產業」時，各至少搜 1 次
4. **照來源清單搜**：先搜使用者的必看來源（可以用「site:網域」限定），再一般搜尋；排除來源一律不收
5. 社群、論壇搜到的消息只當線索，要再搜一次找到媒體或第一手報導
6. 只看到搜尋摘要、打不開原文的，不收（見「事實查證 5. 連結」）

#### 篩選

1. **發布日期**必須在設定的時間範圍內；看不到日期的不收
2. **同一事件只收一則**，多家報導時挑最完整的當主連結，其他列為補充來源
3. 跟使用者主題無關的熱門新聞不要硬塞
4. 排序：追蹤關鍵字命中 ＞ 對使用者工作影響大 ＞ 新聞本身重要性
5. 搜到的不夠設定則數，**就少給**，在頁尾寫「今天〔主題〕相關新聞較少，共 N 則」

#### 撰寫

1. **只寫來源裡有的事實**。數字、人名、日期照原文，不推測、不補細節
   - **照設定的深度寫**：快訊＝一句話；重點＝3 點條列；深度＝背景、各方說法、數字、可能影響（約 200–300 字）；混合＝最重要 3 則深度、其他快訊
   - 設定要追第一手時，在該則下面附「第一手資料」連結（官方公告、財報、法規原文、研究報告），找不到就寫「未找到第一手資料」
2. 摘要用自己的話重寫，不整段複製；可以引用一句短原話，要加引號
3. 國外新聞依設定翻譯，專有名詞第一次出現附原文，例：「生成式引擎最佳化（GEO）」
4. 「💡 跟你有什麼關係」：一句話、具體、連到使用者的職業與目的；推論要寫成「可能」「可以留意」，不要講成定論
5. 不給投資買賣建議、不給醫療或法律個案建議
6. 標題用自己的話改寫成一句清楚的重點，不做聳動標題

#### 洞察與判斷（依設定卡的勾選項目寫，沒勾的不寫）

洞察是「解讀」，判斷是「篩選和下決定」。兩者都是 AI 的推論，所以都要守這三條：
1. **推論要能指回某一則新聞**。說不出根據哪則報導的洞察，就不寫
2. **推論一律標示**：句子用「可能」「可以留意」，或在標籤上寫「推測」。不把推論寫成事實
3. **一則一句話**。洞察寫成長篇分析，使用者就不看了

| 項目 | 怎麼寫 | 範例 |
|---|---|---|
| 趨勢 | 同一段時間 2 則以上報導指向同一件事才算趨勢；只有 1 則就不叫趨勢 | 「這週 3 則新聞都在講 AI 代理人接手廣告投放」 |
| 影響 | 連到使用者的產業、公司或客戶 | 「檔期業績集中，可能讓你明年的預算更往 11 月擠」 |
| 競品動態 | 只寫報導裡有的事實，再加一句「要不要回應」 | 「對手上線會員點數，可以留意客訴與轉換率」 |
| 機會 | 具體到可以做的一件事 | 「可以寫一篇『限動編輯功能』教學貼文」 |
| 風險 | 寫清楚是法規、輿論還是市場風險 | 「平台抓抄襲腳本，搬運型內容可能被降觸及」 |
| 一句話結論 | 放在報告最上面，跟三重點不重複 | 「這週最該做的事：把雙11檢討數字和獲利一起看」 |

| 判斷 | 標籤 | 規則 |
|---|---|---|
| 重要性 | 高／中／低 | 「高」最多佔三成；全部都是高，等於沒有排序 |
| 要不要行動 | 需要行動＋下一步 | 下一步要具體（「本週跟小編更新 SOP」），不寫「持續關注」 |
| 可信度 | 官方公告／多方報導／單一來源／傳聞／廠商業配或新聞稿 | 頁面寫「贊助」「新聞稿」「業配」就一定標出來 |
| 跟我有沒有關 | 不相關就不收 | 寧可少給，不湊數 |
| 好壞消息 | 正面／負面／中性 | 站在使用者公司的角度判斷，不是新聞本身的情緒 |

#### 使用者端的驗證（教使用者做，不是 AI 做）

AI 查證過不代表一定對。新聞中心交付時，提醒使用者養成三個習慣：
1. **抽查**：每份報告至少點開 2 則原文，看摘要和原文一不一致
2. **回報**：發現錯誤就直接說（「第 3 則數字跟原文不同」），AI 修正後要說明錯在哪
3. **更新規則**：同一個來源錯兩次，就加進排除名單（「幫我改新聞中心設定，排除 XX」）

#### 交付前自我檢查

- [ ] 每則都有媒體名稱、發布時間、可點的原文連結
- [ ] 沒有任何一則是憑記憶寫的
- [ ] 每個連結都打開過；重要性高、需要行動的都有兩個獨立來源或第一手來源
- [ ] 數字跟原文一致，基準有寫清楚
- [ ] 社群與論壇來源都已換成媒體或第一手報導
- [ ] 頁尾有查證說明
- [ ] 日期全部在範圍內
- [ ] 則數、分類、配色、語氣符合設定
- [ ] 「今日三重點」跟下方內容一致
- [ ] 每個洞察和判斷都能指回某一則新聞，推論都有標示
- [ ] 要分享給別人的版本，沒有使用者說不能外流的內容

---

## 附錄 C：安裝 skill、寄送方式與每日排程

### 安裝 skill、寄送方式與每日排程

依第 5 輪使用者選的工具和管道，只給他對應的那一段，不要全部貼出來。把 `{{…}}` 換成他的設定。

> **介面會改版。** 以下按鈕名稱是 2026 年 11 月查證的版本。使用者找不到時，請他截圖給你看，依畫面帶他找，不要堅持書面步驟。
> **做不到就說做不到。** 工具沒有排程或沒連 Email 時，退回「每天打開說一句『今天的新聞』」，不要假裝設好了。

---

#### 一、寄送方式

##### HTML 單頁（預設）
- 存成 `{{新聞中心名稱}}_YYYY-MM-DD.html`，雙擊就能用瀏覽器打開
- 版型已內建手機版排版與列印樣式，轉寄、放雲端都可以

##### PDF
- 能執行程式的環境：把 HTML 轉成 A4 PDF（版型的 `@media print` 已設好分頁與配色）
- 不能轉的環境：給 HTML，請使用者用瀏覽器打開 →「列印」→「另存為 PDF」
- 檔名同 HTML，副檔名改 `.pdf`

##### Email
- **只寄到使用者自己的信箱。** 要給同事，請使用者收到後轉寄（見第三節）
- 信件內文用「精簡版」：招呼語＋今日三重點＋每則一行（標題連到原文）＋完整版的說明（附件或存放位置）
- Email 軟體常把網頁樣式拿掉，所以內文不要依賴複雜排版；完整版用 HTML 或 PDF
- 各工具怎麼寄，見第二節

---

#### 二、各 AI 工具的做法

##### A. Claude

**安裝專屬 skill**
1. 先確認「設定 → 功能（Capabilities）」裡的「程式碼執行與檔案建立」已開啟（新版介面叫「建立與編輯檔案」）。沒開的話，skill 會是灰色或找不到
2. 到「自訂（Customize）→ Skills」
3. 按「＋」→「建立 skill」→「上傳 skill」，選 `{{skill名稱}}.zip`（不用解壓縮）
4. 開新對話說「今天的新聞」，能產出報告就成功了
5. Team／Enterprise 方案：由管理員在組織設定開啟 skill 功能

**設定排程（Cowork 的排程任務）**
1. 打開 Claude 桌面 App 的 Cowork，點左側「Scheduled（排程）」→ 右上「New task（新增任務）」
2. 選「Create with Claude」，直接貼上下面這段，Claude 會幫你填好名稱、頻率和指令，確認後按「Schedule」：

```
每{{平日／天／週X}} {{時間}}（{{時區}}）使用 {{skill名稱}} skill，產出最新一期的{{新聞中心名稱}}。
完成後{{寄送方式：把精簡版寄到我的 Gmail {{信箱}}／在對話裡回覆三重點和檔案}}。
```

3. 也可以選「Set up manually」自己填：名稱、指令、頻率（每小時／每天／每週／平日／手動）
4. 建好先按「立即執行」測一次

- 排程任務在雲端執行，**電腦睡眠或 App 關著也會跑**；但手動設定時選了本機資料夾，就只會在電腦開著、App 開著時跑
- 排程任務需要付費方案（Pro 以上）

**寄 Email（Gmail 連接器）**
1. 在 Claude 的連接器裡連上 Gmail
2. Gmail 連接器可以寄信、回信、轉寄；**預設每次寄信前都會先問你**
3. 排程任務沒人在旁邊按同意，信可能卡住。第一次先手動跑一次，確認寄得到；寄不出去就改成「存成草稿」或「在對話裡回覆」
4. 只寄給自己。要給同事，自己轉寄

##### B. ChatGPT

ChatGPT 沒有一樣的 skill，改用「**排程任務**」，把設定整段寫進指令。

1. 開一個新對話，貼上下面整段送出，ChatGPT 會建立排程
2. 左側欄「Scheduled（排程）」可以看到所有任務，能暫停、修改、刪除
3. 收通知：「設定 → 通知」選推播、Email 或兩者都要；手機推播要在手機 App 允許通知

```
請建立一個排程任務：{{平日／每天／每週X}} {{時間}}（{{時區}}）執行以下工作。

你是我的專屬新聞中心「{{新聞中心名稱}}」。
我是 {{稱呼}}，{{職稱與產業}}，看完新聞會拿去 {{用途}}。
目的：{{目的}}。請搜尋過去 {{時間範圍}} 以下主題的新聞：{{主題清單}}。
廣度：{{廣度}}；地區：{{地區}}，國外新聞{{呈現方式}}。
來源：先看 {{第一手與媒體來源}}；社群與論壇只當線索；排除 {{排除來源}}。
一定要收錄：{{追蹤關鍵字}}；不要：{{排除主題}}。
共 {{N}} 則，依主題分類。每則：改寫標題、{{深度}}、💡跟我有什麼關係（一句）、媒體名稱＋發布日期＋原文連結{{、第一手資料連結}}。
洞察：{{洞察項目}}。判斷：{{判斷項目}}。最上面放「今日三重點」。語氣{{語氣}}。
事實查證：每個連結都打開過；重要性高或要行動的新聞至少兩個獨立來源或一個第一手來源，否則標「單一來源」；數字照原文並寫基準；引述寫明誰說的；推論標「推測」；只寫真的搜到的報導，不編造，不夠就少給。頁尾寫查證說明（交叉查證、第一手、單一來源、排除各幾則）。
```

4. 想要 HTML 或 PDF 版：收到後在該對話回「做成 {{風格}} 的 HTML 檔」或「做成 PDF」

##### C. Gemini

Gemini 用「**排定的動作（Scheduled actions）**」。

1. 打開 gemini.google.com 或 Gemini App
2. 貼上 B 段同一份指令，第一行改成「請{{每天}} {{時間}}幫我執行：」
3. 到「設定與說明 → 排定的動作」查看、修改或刪除
4. 注意：最多同時 10 個排定的動作；實際執行時間可能比設定晚一小時內；需要 Google AI Pro 以上或符合資格的 Workspace 方案
5. 想要 HTML 版：在對話裡回「用 Canvas 做成 {{風格}} 的 HTML 網頁」

> 想把設定長期存著、隨時手動叫出來：ChatGPT 可以做成「GPT」、Gemini 可以做成「Gem」，用另一份《GPT 與 Gemini 版指令》。

---

#### 三、分享給同事

| 做法 | 適合 | 注意 |
|---|---|---|
| ⭐ 自己收到後轉寄或轉貼 | 大部分人 | 最簡單；轉寄前看一眼有沒有不能外流的內容 |
| HTML／PDF 放共用雲端資料夾 | 固定給一個團隊 | 排程存檔的資料夾設成共用，同事打開就看得到 |
| 把專屬 skill 分享給同事 | 同事也想要自己的版本 | Claude 的 skill 預設只有自己看得到；把 zip 傳給同事上傳，或用團隊方案的分享功能。同事可以再說「改新聞中心設定」改成他的版本 |
| 主管版／團隊版兩種 | 給不同層級 | 專屬 skill 裡設兩種輸出：主管版只有三重點＋一句話結論 |

**不要讓 AI 自動寄給別人。** 寄錯人、寄出內部資料都收不回來；每一封對外的信都由使用者自己按送出。

---

## 附錄 D1：簡約版版型

````html
<!doctype html>
<!--
  新聞中心版型：簡約版（給 AI 套版用，使用者看不到這段）
  套版步驟：
  1. 自訂配色：只改 :root 的色票（--primary 主色、--accent 強調、--bg 底色、--text 文字）。文字和底色對比至少 4.5:1
  2. 把所有 {{…}} 換成真的內容。範例只有一則新聞，照同樣結構複製 <article class="item">
  3. 每個主題一個 <section class="topic" id="t1">，id 依序 t1、t2…，上方 nav 的連結跟著改
  4. 每份都要留頁尾的「查證說明」（.verify），照 news-rules.md「事實查證 7.」填
  5. 使用者沒勾的項目整塊刪掉：一句話結論（.lead）、三重點（.brief）、洞察（.insights）、判斷總覽（.judge）、
     每則的重要性／可信度標籤、💡跟你有什麼關係（.why）、✅需要行動（.action）
  6. 洞察是推論成分高的，chip 加 class="guess"，文字後面加「・推測」
  7. 可信度出現「傳聞、單一來源、廠商新聞稿、業配、測試中、平台自行公布」時，chip 加 class="cred-warn"
  
  整份檔案不依賴網路，雙擊就能打開；用瀏覽器「列印 → 另存為 PDF」就是 A4 PDF。
-->
<html lang="zh-Hant">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>{{新聞中心名稱}}｜{{日期，例：2026 年 11 月 24 日 星期一}}</title>
<style>
  :root {
    --primary: #2450C8; --accent: #2450C8; --on-accent: #FFFFFF;
    --bg: #FFFFFF; --card: #FFFFFF; --text: #1C2230; --muted: #687083; --line: #E3E6EC;
    --tint: #EEF2FC; --why-bg: #F4F6FA; --act-bg: #FFF5E0; --warn-bg: #FCEBEA; --warn: #A6342B;
    --font-body: -apple-system, BlinkMacSystemFont, "PingFang TC", "Noto Sans TC", "Microsoft JhengHei", sans-serif;
    --font-head: var(--font-body); --max: 760px;
  }

  * { box-sizing: border-box; }
  html { -webkit-text-size-adjust: 100%; }
  body { margin: 0; background: var(--bg); color: var(--text); font-family: var(--font-body);
         font-size: 16px; line-height: 1.7; }
  a { color: var(--primary); }
  .wrap { max-width: var(--max); margin: 0 auto; padding: 0 16px 56px; }
  .chip { display: inline-block; font-size: 12.5px; line-height: 1.6; padding: 0 8px; border-radius: 6px;
          background: var(--tint); color: var(--primary); white-space: nowrap; }
  .chip.imp-high { background: var(--accent); color: var(--on-accent); font-weight: 700; }
  .chip.imp-mid  { background: var(--tint); color: var(--primary); }
  .chip.imp-low  { background: transparent; color: var(--muted); border: 1px solid var(--line); }
  .chip.cred-warn { background: var(--warn-bg); color: var(--warn); }
  .chip.guess { background: var(--warn-bg); color: var(--warn); }
  .labels { display: flex; flex-wrap: wrap; gap: 6px; margin: 0 0 8px; }
  .no { font-size: 12.5px; color: var(--muted); font-variant-numeric: tabular-nums; }
  .meta { font-size: 13px; color: var(--muted); margin: 0 0 10px; display: flex; flex-wrap: wrap; gap: 2px 12px; }
  .orig { font-size: 13px; color: var(--muted); margin: -2px 0 8px; font-style: italic; }
  .summary { margin: 0; padding-left: 1.2em; }
  .summary li { margin: 2px 0; }
  .why, .action { margin: 10px 0 0; padding: 9px 13px; border-radius: 8px; font-size: 15px; }
  .why { background: var(--why-bg); }
  .action { background: var(--act-bg); }
  .refs { color: var(--muted); font-size: 13px; white-space: nowrap; }
  .sources, .primary { margin: 8px 0 0; font-size: 14px; color: var(--muted); }
  .sources a, .primary a { color: inherit; }
  .verify { margin: 36px 0 0; padding: 12px 16px; border: 1px dashed var(--line); border-radius: 10px; font-size: 14px; color: var(--muted); }
  footer { margin-top: 40px; font-size: 13px; color: var(--muted); text-align: center; }
  @media print {
    @page { size: A4; margin: 14mm; }
    body { -webkit-print-color-adjust: exact; print-color-adjust: exact; font-size: 11pt; }
    nav.chips { display: none; }
    article.item, .box { break-inside: avoid; }
  }

  header.masthead { padding: 40px 0 8px; }
  .eyebrow { font-size: 13px; color: var(--muted); margin: 0 0 6px; letter-spacing: .04em; }
  h1 { font-size: clamp(26px, 5vw, 34px); line-height: 1.25; margin: 0 0 6px; }
  .greeting { margin: 0; color: var(--muted); }
  nav.chips { display: flex; flex-wrap: wrap; gap: 8px; margin: 18px 0 0; }
  nav.chips a { font-size: 14px; text-decoration: none; padding: 2px 12px; border: 1px solid var(--line); border-radius: 999px; color: var(--text); }
  .box { margin: 24px 0 0; padding: 18px 20px; border: 1px solid var(--line); border-radius: 12px; }
  .box h2 { font-size: 15px; margin: 0 0 8px; color: var(--muted); font-weight: 600; letter-spacing: .04em; }
  .lead { background: var(--tint); border: 0; }
  .lead p { margin: 0; font-size: 19px; font-weight: 700; line-height: 1.5; }
  .brief ol, .insights ul { margin: 0; padding-left: 1.3em; }
  .insights ul { list-style: none; padding: 0; }
  .insights li { margin: 6px 0; }
  .judge { display: flex; flex-wrap: wrap; gap: 8px 20px; font-size: 14px; color: var(--muted); }
  .judge b { color: var(--text); font-size: 18px; margin-right: 4px; }
  section.topic { margin-top: 36px; }
  section.topic > h2 { font-size: 20px; margin: 0 0 4px; }
  section.topic > h2 .count { font-size: 13px; font-weight: 400; color: var(--muted); margin-left: 8px; }
  article.item { padding: 18px 0; border-bottom: 1px solid var(--line); }
  article.item:last-child { border-bottom: 0; }
  .item h3 { font-size: 18px; line-height: 1.45; margin: 0 0 4px; }
  .item h3 a { color: var(--text); text-decoration: none; }
  .item h3 a:hover { color: var(--primary); text-decoration: underline; }
</style>
</head>
<body class="style-minimal">
<header class="masthead"><div class="wrap" style="padding-bottom:0">
<p class="eyebrow">{{日期，例：2026 年 11 月 24 日 星期一}} ・ {{期別或涵蓋期間，例：第 12 期・涵蓋 11/17–11/23}}</p>
<h1>{{新聞中心名稱}}</h1>
<p class="greeting">{{招呼語，例：早安，Amy！今天整理了 8 則，約 5 分鐘看完。}}</p>
<nav class="chips"><a href="#t1">{{主題 1}}</a></nav></div></header>
<main class="wrap">
<section class="box lead"><h2>一句話結論</h2><p>{{一句話結論：今天最重要的一件事}}</p></section>
<section class="box brief"><h2>今日三重點</h2><ol><li>{{重點 1，一句話}}</li><li>{{重點 2}}</li><li>{{重點 3}}</li></ol></section>
<section class="box insights"><h2>AI 幫你多想一步</h2><ul><li><span class="chip">{{洞察類型：趨勢／影響／競品動態／機會／風險}}</span> {{一句話洞察}} <span class="refs">依據：{{第幾則，例：No.1、No.2}}</span></li><li><span class="chip guess">{{推論成分高的洞察}}・推測</span> {{一句話，用「可能」「可以留意」}} <span class="refs">依據：{{No.3}}</span></li></ul></section>
<section class="judge"><span><b>{{N}}</b>則新聞</span><span><b>{{N}}</b>則重要性高</span><span><b>{{N}}</b>則需要行動</span><span><b>{{N}}</b>則要留意可信度</span></section>
<section class="topic" id="t1">
<h2>{{主題 1}}<span class="count">1 則</span></h2>
<div class="items">
<article class="item">
  <div class="labels"><span class="no">No.1</span><span class="chip imp-high">重要性 高</span><span class="chip cred-warn">{{可信度：官方公告／多方報導／單一來源／傳聞／廠商新聞稿}}</span></div>
  <h3><a href="{{原文連結}}" target="_blank" rel="noopener">{{用自己的話改寫的標題}}</a></h3>
  <p class="orig">{{國外新聞保留原文標題時才放}}</p>
  <p class="meta"><span>{{台灣／國際／國家}}</span><span>{{媒體名稱}}</span><span>{{發布日期，例：11/18}}</span></p>
  <ul class="summary"><li>{{重點 1}}</li><li>{{重點 2}}</li><li>{{重點 3}}</li></ul>
  <p class="why">💡 <strong>跟你有什麼關係：</strong>{{一句連到使用者職業與用途的話}}</p>
  <p class="action">✅ <strong>需要行動：</strong>{{具體的下一步；不需要行動就整行刪掉}}</p>
  <p class="sources">其他報導：<a href="{{連結}}" target="_blank" rel="noopener">{{其他報導的媒體＋日期；沒有就整行刪掉}}</a></p>
  <p class="primary">📄 第一手資料：<a href="{{連結}}" target="_blank" rel="noopener">{{第一手資料名稱，例：官方公告；設定不追第一手就整行刪掉}}</a></p>
</article>
</div>
</section>
<section class="verify">🔎 <strong>查證說明：</strong>本期 {{N}} 則：交叉查證 {{N}} 則、第一手 {{N}} 則、單一來源 {{N}} 則；排除 {{N}} 則（{{原因，例：只有社群貼文、原文打不開}}）</section>
<footer>{{新聞中心名稱}} ・ 由 AI 依你的設定整理，內容以各原文為準 ・ {{則數不足時：今天「主題」相關新聞較少，共 N 則}}</footer>
</main>
</body>
</html>
````

---

## 附錄 D2：報紙風版型

````html
<!doctype html>
<!--
  新聞中心版型：報紙風（給 AI 套版用，使用者看不到這段）
  套版步驟：
  1. 自訂配色：只改 :root 的色票（--primary 主色、--accent 強調、--bg 底色、--text 文字）。文字和底色對比至少 4.5:1
  2. 把所有 {{…}} 換成真的內容。範例只有一則新聞，照同樣結構複製 <article class="item">
  3. 每個主題一個 <section class="topic" id="t1">，id 依序 t1、t2…，上方 nav 的連結跟著改
  4. 每份都要留頁尾的「查證說明」（.verify），照 news-rules.md「事實查證 7.」填
  5. 使用者沒勾的項目整塊刪掉：一句話結論（.lead）、三重點（.brief）、洞察（.insights）、判斷總覽（.judge）、
     每則的重要性／可信度標籤、💡跟你有什麼關係（.why）、✅需要行動（.action）
  6. 洞察是推論成分高的，chip 加 class="guess"，文字後面加「・推測」
  7. 可信度出現「傳聞、單一來源、廠商新聞稿、業配、測試中、平台自行公布」時，chip 加 class="cred-warn"
  
  整份檔案不依賴網路，雙擊就能打開；用瀏覽器「列印 → 另存為 PDF」就是 A4 PDF。
-->
<html lang="zh-Hant">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>{{新聞中心名稱}}｜{{日期，例：2026 年 11 月 24 日 星期一}}</title>
<style>
  :root {
    --primary: #111111; --accent: #8E1B1B; --on-accent: #FFFFFF;
    --bg: #FBFAF7; --card: #FBFAF7; --text: #151515; --muted: #5E5A55; --line: #CFCAC2;
    --tint: #EEEBE5; --why-bg: #F1EEE8; --act-bg: #F6E9E4; --warn-bg: #F6E3E1; --warn: #8E1B1B;
    --font-body: "Noto Serif TC", "Songti TC", "PMingLiU", "Source Han Serif TC", Georgia, serif;
    --font-head: "Noto Serif TC", "Songti TC", "PMingLiU", Georgia, serif; --max: 980px;
  }

  * { box-sizing: border-box; }
  html { -webkit-text-size-adjust: 100%; }
  body { margin: 0; background: var(--bg); color: var(--text); font-family: var(--font-body);
         font-size: 16px; line-height: 1.7; }
  a { color: var(--primary); }
  .wrap { max-width: var(--max); margin: 0 auto; padding: 0 16px 56px; }
  .chip { display: inline-block; font-size: 12.5px; line-height: 1.6; padding: 0 8px; border-radius: 6px;
          background: var(--tint); color: var(--primary); white-space: nowrap; }
  .chip.imp-high { background: var(--accent); color: var(--on-accent); font-weight: 700; }
  .chip.imp-mid  { background: var(--tint); color: var(--primary); }
  .chip.imp-low  { background: transparent; color: var(--muted); border: 1px solid var(--line); }
  .chip.cred-warn { background: var(--warn-bg); color: var(--warn); }
  .chip.guess { background: var(--warn-bg); color: var(--warn); }
  .labels { display: flex; flex-wrap: wrap; gap: 6px; margin: 0 0 8px; }
  .no { font-size: 12.5px; color: var(--muted); font-variant-numeric: tabular-nums; }
  .meta { font-size: 13px; color: var(--muted); margin: 0 0 10px; display: flex; flex-wrap: wrap; gap: 2px 12px; }
  .orig { font-size: 13px; color: var(--muted); margin: -2px 0 8px; font-style: italic; }
  .summary { margin: 0; padding-left: 1.2em; }
  .summary li { margin: 2px 0; }
  .why, .action { margin: 10px 0 0; padding: 9px 13px; border-radius: 8px; font-size: 15px; }
  .why { background: var(--why-bg); }
  .action { background: var(--act-bg); }
  .refs { color: var(--muted); font-size: 13px; white-space: nowrap; }
  .sources, .primary { margin: 8px 0 0; font-size: 14px; color: var(--muted); }
  .sources a, .primary a { color: inherit; }
  .verify { margin: 36px 0 0; padding: 12px 16px; border: 1px dashed var(--line); border-radius: 10px; font-size: 14px; color: var(--muted); }
  footer { margin-top: 40px; font-size: 13px; color: var(--muted); text-align: center; }
  @media print {
    @page { size: A4; margin: 14mm; }
    body { -webkit-print-color-adjust: exact; print-color-adjust: exact; font-size: 11pt; }
    nav.chips { display: none; }
    article.item, .box { break-inside: avoid; }
  }

  body { line-height: 1.75; }
  a { color: var(--text); }
  header.masthead { text-align: center; padding: 28px 0 0; }
  header.masthead .rule { border-top: 3px double var(--text); border-bottom: 1px solid var(--text); padding: 8px 0 12px; margin-top: 10px; }
  .eyebrow { display: flex; justify-content: space-between; font-size: 13px; color: var(--muted); margin: 0; border-bottom: 1px solid var(--text); padding-bottom: 6px; }
  h1 { font-family: var(--font-head); font-size: clamp(34px, 7vw, 58px); line-height: 1.1; margin: 0; font-weight: 900; letter-spacing: .02em; }
  .greeting { margin: 6px 0 0; font-style: italic; color: var(--muted); }
  nav.chips { display: flex; justify-content: center; flex-wrap: wrap; gap: 4px 18px; margin: 8px 0 0; padding: 6px 0; border-bottom: 1px solid var(--text); font-size: 14px; }
  nav.chips a { text-decoration: none; letter-spacing: .08em; }
  .front { display: grid; grid-template-columns: 1.4fr 1fr; gap: 0 28px; margin-top: 22px; }
  .box { break-inside: avoid; }
  .box h2 { font-size: 13px; letter-spacing: .18em; margin: 0 0 6px; color: var(--accent); }
  .lead p { margin: 0; font-family: var(--font-head); font-size: clamp(22px, 3.4vw, 30px); line-height: 1.35; font-weight: 900; }
  .brief { border-left: 1px solid var(--line); padding-left: 24px; }
  .brief ol { margin: 0; padding-left: 1.2em; }
  .insights { margin-top: 22px; padding: 14px 0; border-top: 1px solid var(--text); border-bottom: 1px solid var(--text); }
  .insights ul { margin: 0; padding: 0; list-style: none; columns: 3 220px; column-gap: 28px; }
  .insights li { break-inside: avoid; margin: 0 0 8px; }
  .judge { margin-top: 10px; font-size: 13px; color: var(--muted); display: flex; justify-content: center; flex-wrap: wrap; gap: 4px 22px; letter-spacing: .06em; }
  .judge b { color: var(--text); margin-right: 3px; }
  section.topic { margin-top: 30px; }
  section.topic > h2 { font-family: var(--font-head); font-size: 14px; letter-spacing: .22em; margin: 0 0 14px; padding: 4px 0; border-top: 2px solid var(--text); border-bottom: 1px solid var(--text); }
  section.topic > h2 .count { font-weight: 400; color: var(--muted); margin-left: 8px; letter-spacing: 0; }
  .items { columns: 2 300px; column-gap: 30px; column-rule: 1px solid var(--line); }
  article.item { break-inside: avoid; margin: 0 0 22px; }
  .item h3 { font-family: var(--font-head); font-size: 21px; line-height: 1.35; margin: 0 0 6px; font-weight: 900; }
  .item h3 a { text-decoration: none; }
  .item h3 a:hover { text-decoration: underline; }
  .chip { border-radius: 0; }
  .why, .action { border-radius: 0; }
  @media (max-width: 720px) { .front { grid-template-columns: 1fr; } .brief { border-left: 0; padding-left: 0; margin-top: 16px; border-top: 1px solid var(--line); padding-top: 12px; } }
</style>
</head>
<body class="style-times">
<header class="masthead"><div class="wrap" style="padding-bottom:0">
<p class="eyebrow"><span>{{日期，例：2026 年 11 月 24 日 星期一}}</span><span>{{期別或涵蓋期間，例：第 12 期・涵蓋 11/17–11/23}}</span></p>
<div class="rule"><h1>{{新聞中心名稱}}</h1><p class="greeting">{{招呼語，例：早安，Amy！今天整理了 8 則，約 5 分鐘看完。}}</p></div>
<nav class="chips"><a href="#t1">{{主題 1}}</a></nav></div></header>
<main class="wrap">
<div class="front">
<section class="box lead"><h2>一句話結論</h2><p>{{一句話結論：今天最重要的一件事}}</p></section>
<section class="box brief"><h2>今日三重點</h2><ol><li>{{重點 1，一句話}}</li><li>{{重點 2}}</li><li>{{重點 3}}</li></ol></section>
</div>
<section class="box insights"><h2>AI 幫你多想一步</h2><ul><li><span class="chip">{{洞察類型：趨勢／影響／競品動態／機會／風險}}</span> {{一句話洞察}} <span class="refs">依據：{{第幾則，例：No.1、No.2}}</span></li><li><span class="chip guess">{{推論成分高的洞察}}・推測</span> {{一句話，用「可能」「可以留意」}} <span class="refs">依據：{{No.3}}</span></li></ul></section>
<section class="judge"><span><b>{{N}}</b>則新聞</span><span><b>{{N}}</b>則重要性高</span><span><b>{{N}}</b>則需要行動</span><span><b>{{N}}</b>則要留意可信度</span></section>
<section class="topic" id="t1">
<h2>{{主題 1}}<span class="count">1 則</span></h2>
<div class="items">
<article class="item">
  <div class="labels"><span class="no">No.1</span><span class="chip imp-high">重要性 高</span><span class="chip cred-warn">{{可信度：官方公告／多方報導／單一來源／傳聞／廠商新聞稿}}</span></div>
  <h3><a href="{{原文連結}}" target="_blank" rel="noopener">{{用自己的話改寫的標題}}</a></h3>
  <p class="orig">{{國外新聞保留原文標題時才放}}</p>
  <p class="meta"><span>{{台灣／國際／國家}}</span><span>{{媒體名稱}}</span><span>{{發布日期，例：11/18}}</span></p>
  <ul class="summary"><li>{{重點 1}}</li><li>{{重點 2}}</li><li>{{重點 3}}</li></ul>
  <p class="why">💡 <strong>跟你有什麼關係：</strong>{{一句連到使用者職業與用途的話}}</p>
  <p class="action">✅ <strong>需要行動：</strong>{{具體的下一步；不需要行動就整行刪掉}}</p>
  <p class="sources">其他報導：<a href="{{連結}}" target="_blank" rel="noopener">{{其他報導的媒體＋日期；沒有就整行刪掉}}</a></p>
  <p class="primary">📄 第一手資料：<a href="{{連結}}" target="_blank" rel="noopener">{{第一手資料名稱，例：官方公告；設定不追第一手就整行刪掉}}</a></p>
</article>
</div>
</section>
<section class="verify">🔎 <strong>查證說明：</strong>本期 {{N}} 則：交叉查證 {{N}} 則、第一手 {{N}} 則、單一來源 {{N}} 則；排除 {{N}} 則（{{原因，例：只有社群貼文、原文打不開}}）</section>
<footer>{{新聞中心名稱}} ・ 由 AI 依你的設定整理，內容以各原文為準 ・ {{則數不足時：今天「主題」相關新聞較少，共 N 則}}</footer>
</main>
</body>
</html>
````

---

## 附錄 D3：雜誌風版型

````html
<!doctype html>
<!--
  新聞中心版型：雜誌風（給 AI 套版用，使用者看不到這段）
  套版步驟：
  1. 自訂配色：只改 :root 的色票（--primary 主色、--accent 強調、--bg 底色、--text 文字）。文字和底色對比至少 4.5:1
  2. 把所有 {{…}} 換成真的內容。範例只有一則新聞，照同樣結構複製 <article class="item">
  3. 每個主題一個 <section class="topic" id="t1">，id 依序 t1、t2…，上方 nav 的連結跟著改
  4. 每份都要留頁尾的「查證說明」（.verify），照 news-rules.md「事實查證 7.」填
  5. 使用者沒勾的項目整塊刪掉：一句話結論（.lead）、三重點（.brief）、洞察（.insights）、判斷總覽（.judge）、
     每則的重要性／可信度標籤、💡跟你有什麼關係（.why）、✅需要行動（.action）
  6. 洞察是推論成分高的，chip 加 class="guess"，文字後面加「・推測」
  7. 可信度出現「傳聞、單一來源、廠商新聞稿、業配、測試中、平台自行公布」時，chip 加 class="cred-warn"
  8. 雜誌風：第一個主題的第一則加 class="cover"，會變成封面故事
  整份檔案不依賴網路，雙擊就能打開；用瀏覽器「列印 → 另存為 PDF」就是 A4 PDF。
-->
<html lang="zh-Hant">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>{{新聞中心名稱}}｜{{日期，例：2026 年 11 月 24 日 星期一}}</title>
<style>
  :root {
    --primary: #5B3FD1; --accent: #FF6B4A; --on-accent: #FFFFFF;
    --bg: #F3F1EC; --card: #FFFFFF; --text: #1B1830; --muted: #6B6880; --line: #E4E1EA;
    --tint: #ECE8FB; --why-bg: #F5F3FC; --act-bg: #FFF0EB; --warn-bg: #FFE8E3; --warn: #B3361C;
    --cover: #1B1830; --on-cover: #FFFFFF;
    --font-body: -apple-system, BlinkMacSystemFont, "PingFang TC", "Noto Sans TC", "Microsoft JhengHei", sans-serif;
    --font-head: var(--font-body); --max: 1040px;
  }

  * { box-sizing: border-box; }
  html { -webkit-text-size-adjust: 100%; }
  body { margin: 0; background: var(--bg); color: var(--text); font-family: var(--font-body);
         font-size: 16px; line-height: 1.7; }
  a { color: var(--primary); }
  .wrap { max-width: var(--max); margin: 0 auto; padding: 0 16px 56px; }
  .chip { display: inline-block; font-size: 12.5px; line-height: 1.6; padding: 0 8px; border-radius: 6px;
          background: var(--tint); color: var(--primary); white-space: nowrap; }
  .chip.imp-high { background: var(--accent); color: var(--on-accent); font-weight: 700; }
  .chip.imp-mid  { background: var(--tint); color: var(--primary); }
  .chip.imp-low  { background: transparent; color: var(--muted); border: 1px solid var(--line); }
  .chip.cred-warn { background: var(--warn-bg); color: var(--warn); }
  .chip.guess { background: var(--warn-bg); color: var(--warn); }
  .labels { display: flex; flex-wrap: wrap; gap: 6px; margin: 0 0 8px; }
  .no { font-size: 12.5px; color: var(--muted); font-variant-numeric: tabular-nums; }
  .meta { font-size: 13px; color: var(--muted); margin: 0 0 10px; display: flex; flex-wrap: wrap; gap: 2px 12px; }
  .orig { font-size: 13px; color: var(--muted); margin: -2px 0 8px; font-style: italic; }
  .summary { margin: 0; padding-left: 1.2em; }
  .summary li { margin: 2px 0; }
  .why, .action { margin: 10px 0 0; padding: 9px 13px; border-radius: 8px; font-size: 15px; }
  .why { background: var(--why-bg); }
  .action { background: var(--act-bg); }
  .refs { color: var(--muted); font-size: 13px; white-space: nowrap; }
  .sources, .primary { margin: 8px 0 0; font-size: 14px; color: var(--muted); }
  .sources a, .primary a { color: inherit; }
  .verify { margin: 36px 0 0; padding: 12px 16px; border: 1px dashed var(--line); border-radius: 10px; font-size: 14px; color: var(--muted); }
  footer { margin-top: 40px; font-size: 13px; color: var(--muted); text-align: center; }
  @media print {
    @page { size: A4; margin: 14mm; }
    body { -webkit-print-color-adjust: exact; print-color-adjust: exact; font-size: 11pt; }
    nav.chips { display: none; }
    article.item, .box { break-inside: avoid; }
  }

  header.masthead { padding: 34px 0 10px; }
  .eyebrow { font-size: 13px; font-weight: 700; color: var(--accent); letter-spacing: .14em; margin: 0 0 4px; }
  h1 { font-size: clamp(34px, 7vw, 64px); line-height: 1.02; margin: 0; font-weight: 900; letter-spacing: -.01em; }
  .greeting { margin: 10px 0 0; color: var(--muted); font-size: 17px; }
  nav.chips { display: flex; flex-wrap: wrap; gap: 8px; margin: 18px 0 0; }
  nav.chips a { font-size: 14px; font-weight: 700; text-decoration: none; padding: 4px 14px; border-radius: 999px; background: var(--card); color: var(--text); }
  .front { display: grid; grid-template-columns: 1.3fr 1fr; gap: 16px; margin-top: 22px; }
  .box { border-radius: 20px; padding: 22px 24px; background: var(--card); }
  .box h2 { font-size: 13px; letter-spacing: .14em; margin: 0 0 10px; color: var(--accent); }
  .lead { background: var(--cover); color: var(--on-cover); }
  .lead p { margin: 0; font-size: clamp(22px, 3.2vw, 32px); line-height: 1.3; font-weight: 900; }
  .brief ol { margin: 0; padding-left: 1.2em; font-weight: 600; }
  .brief li { margin: 6px 0; }
  .insights { margin-top: 16px; }
  .insights ul { list-style: none; margin: 0; padding: 0; display: grid; grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); gap: 12px; }
  .insights li { background: var(--why-bg); border-radius: 14px; padding: 12px 14px; }
  .judge { margin-top: 16px; display: flex; flex-wrap: wrap; gap: 10px; }
  .judge span { background: var(--card); border-radius: 999px; padding: 4px 14px; font-size: 14px; color: var(--muted); }
  .judge b { color: var(--text); margin-right: 4px; }
  section.topic { margin-top: 40px; }
  section.topic > h2 { font-size: 26px; font-weight: 900; margin: 0 0 14px; }
  section.topic > h2 .count { font-size: 14px; font-weight: 700; color: var(--accent); margin-left: 10px; }
  .items { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
  article.item { background: var(--card); border-radius: 18px; padding: 20px 22px; }
  article.item.cover ~ article.item:last-child:nth-child(even) { grid-column: 1 / -1; }
  article.item.cover { grid-column: 1 / -1; background: var(--primary); color: #fff; padding: 28px 30px; }
  article.item.cover h3 { font-size: clamp(22px, 3.4vw, 30px); }
  article.item.cover h3 a, article.item.cover .meta, article.item.cover .no { color: #fff; }
  article.item.cover .meta { opacity: .85; }
  article.item.cover .why, article.item.cover .action { background: rgba(255,255,255,.14); color: #fff; }
  article.item.cover .chip.imp-high { background: var(--accent); }
  article.item.cover .chip:not(.imp-high) { background: rgba(255,255,255,.18); color: #fff; border-color: transparent; }
  .item h3 { font-size: 19px; line-height: 1.4; margin: 0 0 6px; font-weight: 800; }
  .item h3 a { color: inherit; text-decoration: none; }
  .item h3 a:hover { text-decoration: underline; }
  @media (max-width: 720px) { .front, .items { grid-template-columns: 1fr; } }
</style>
</head>
<body class="style-magazine">
<header class="masthead"><div class="wrap" style="padding-bottom:0">
<p class="eyebrow">{{日期，例：2026 年 11 月 24 日 星期一}} ・ {{期別或涵蓋期間，例：第 12 期・涵蓋 11/17–11/23}}</p>
<h1>{{新聞中心名稱}}</h1>
<p class="greeting">{{招呼語，例：早安，Amy！今天整理了 8 則，約 5 分鐘看完。}}</p>
<nav class="chips"><a href="#t1">{{主題 1}}</a></nav></div></header>
<main class="wrap">
<div class="front">
<section class="box lead"><h2>一句話結論</h2><p>{{一句話結論：今天最重要的一件事}}</p></section>
<section class="box brief"><h2>今日三重點</h2><ol><li>{{重點 1，一句話}}</li><li>{{重點 2}}</li><li>{{重點 3}}</li></ol></section>
</div>
<section class="box insights"><h2>AI 幫你多想一步</h2><ul><li><span class="chip">{{洞察類型：趨勢／影響／競品動態／機會／風險}}</span> {{一句話洞察}} <span class="refs">依據：{{第幾則，例：No.1、No.2}}</span></li><li><span class="chip guess">{{推論成分高的洞察}}・推測</span> {{一句話，用「可能」「可以留意」}} <span class="refs">依據：{{No.3}}</span></li></ul></section>
<section class="judge"><span><b>{{N}}</b>則新聞</span><span><b>{{N}}</b>則重要性高</span><span><b>{{N}}</b>則需要行動</span><span><b>{{N}}</b>則要留意可信度</span></section>
<section class="topic" id="t1">
<h2>{{主題 1}}<span class="count">1 則</span></h2>
<div class="items">
<article class="item cover">
  <div class="labels"><span class="no">No.1</span><span class="chip imp-high">重要性 高</span><span class="chip cred-warn">{{可信度：官方公告／多方報導／單一來源／傳聞／廠商新聞稿}}</span></div>
  <h3><a href="{{原文連結}}" target="_blank" rel="noopener">{{用自己的話改寫的標題}}</a></h3>
  <p class="orig">{{國外新聞保留原文標題時才放}}</p>
  <p class="meta"><span>{{台灣／國際／國家}}</span><span>{{媒體名稱}}</span><span>{{發布日期，例：11/18}}</span></p>
  <ul class="summary"><li>{{重點 1}}</li><li>{{重點 2}}</li><li>{{重點 3}}</li></ul>
  <p class="why">💡 <strong>跟你有什麼關係：</strong>{{一句連到使用者職業與用途的話}}</p>
  <p class="action">✅ <strong>需要行動：</strong>{{具體的下一步；不需要行動就整行刪掉}}</p>
  <p class="sources">其他報導：<a href="{{連結}}" target="_blank" rel="noopener">{{其他報導的媒體＋日期；沒有就整行刪掉}}</a></p>
  <p class="primary">📄 第一手資料：<a href="{{連結}}" target="_blank" rel="noopener">{{第一手資料名稱，例：官方公告；設定不追第一手就整行刪掉}}</a></p>
</article>
</div>
</section>
<section class="verify">🔎 <strong>查證說明：</strong>本期 {{N}} 則：交叉查證 {{N}} 則、第一手 {{N}} 則、單一來源 {{N}} 則；排除 {{N}} 則（{{原因，例：只有社群貼文、原文打不開}}）</section>
<footer>{{新聞中心名稱}} ・ 由 AI 依你的設定整理，內容以各原文為準 ・ {{則數不足時：今天「主題」相關新聞較少，共 N 則}}</footer>
</main>
</body>
</html>
````

---

## 附錄 E：使用者專屬 skill 範本

````markdown
---
name: {{skill名稱，例：amy-news-center}}
description: {{稱呼}}的專屬新聞中心「{{新聞中心名稱}}」。依固定設定搜尋{{主題清單}}的最新新聞，整理成重點摘要與出處，附上洞察與判斷，輸出成{{風格}}的新聞報告（{{收件方式}}）。當使用者說「今天的新聞」「我的新聞中心」「{{新聞中心名稱}}」「每日新聞」「幫我看今天新聞」，或排程任務每天自動執行時使用。使用者說「改新聞中心設定」時，依指示更新這份設定。
---

# {{新聞中心名稱}}

## 我的設定

| 項目 | 設定 |
|---|---|
| 稱呼 | {{稱呼}} |
| 職稱・產業 | {{職稱與產業}} |
| 負責市場 | {{市場或地區，沒有就寫「未填」}} |
| 平常在忙 | {{一句話工作描述，沒有就寫「未填」}} |
| 看完拿去 | {{用途}} |
| 目的 | {{跟上趨勢／盯特定對象／找素材／支援決策／風險預警}} |
| 主題 | {{① 主題 1 ② 主題 2 ③ 主題 3 …}} |
| 特別追蹤 | {{關鍵字，出現就一定收錄；沒有寫「無」}} |
| 不要看 | {{排除主題；沒有寫「無」}} |
| 廣度 | {{只看本業／本業＋上下游／本業＋相鄰產業／跨產業大趨勢}} |
| 地區 | {{台灣／國際／指定國家}} |
| 國外新聞 | {{翻成繁中摘要、標題保留原文／全中文／保留英文}} |
| 深度 | {{快訊／重點／深度／混合}}；第一手資料：{{要追／不用}} |
| 來源清單 | 第一手：{{…}}；媒體：{{…}}；社群與論壇只當線索；排除：{{…}} |
| 份量 | {{N}} 則，約 {{N}} 分鐘看完{{；深度版：3 則深度分析＋快訊}} |
| 時間範圍 | {{過去 24 小時（星期一自動涵蓋週末）／過去 7 天}} |
| 洞察 | {{趨勢／影響／競品動態／機會／風險／一句話結論，沒選寫「不用」}} |
| 判斷 | {{重要性／要不要行動／可信度／跟我有沒有關／好壞消息，沒選寫「不用」}} |
| 跟我有什麼關係 | {{要／不用}} |
| 今日三重點 | {{要／不用}} |
| 出處格式 | {{媒體名稱＋發布時間＋原文連結}} |
| 語氣 | {{專業清楚／輕鬆口語／精準簡短}} |
| 風格 | {{簡約／報紙／雜誌／自訂}}{{，自訂色票：主色 #…、強調 #…}} |
| 收報 | {{工具}}，{{平日／每天／每週X}} {{時間}}（{{時區}}） |
| 收件方式 | {{網頁 HTML／Email 寄到 {{信箱}}／PDF}} |
| 分享 | {{只有我／給誰、什麼版本}}；不能外流：{{內容，沒有寫「無」}} |

## 每次執行的步驟

1. **搜尋**：照「廣度」決定搜哪些範圍；先搜「來源清單」的第一手與媒體來源（可用 site:網域），再一般搜尋。每個主題至少搜 2 次（中文一次、英文或換說法一次）；「特別追蹤」的關鍵字各自單獨搜。搜尋字串加上今天的年月。社群、論壇搜到的只當線索，要再找到媒體或第一手報導。
2. **篩選**：只收時間範圍內、看得到發布日期的報導；同一事件只收一則；跳過「不要看」的主題、排除來源、內容農場、無日期頁面。排序：追蹤關鍵字 ＞ 對我工作影響大 ＞ 新聞重要性。
3. **事實查證**（每一則都要過，做不到就不收）：
   - 連結都實際打開過，內容和日期跟摘要一致；付費牆標「付費內容，摘要依公開段落」
   - 重要性「高」或「需要行動」的新聞：至少兩個獨立來源，或一個第一手來源；只有一個來源就標「單一來源」、不能標高
   - 數字回到原文核對，照原文寫，寫清楚基準（跟平日比、跟去年比）；公司自己公布的標「自行公布」
   - 分清楚事實、引述（「XX 表示」「業者宣稱」）和推論（標「推測」）
   - 舊聞轉載以最早的報導日期為準；人物職稱照報導當時的寫法
4. **撰寫**：照「深度」寫（快訊一句話／重點 3 點／深度含背景與各方說法／混合），用自己的話寫，不整段複製原文。「跟我有什麼關係」寫一句具體的話，連到我的職業、用途與目的。設定要追第一手時，附第一手資料連結。
   **洞察與判斷**：只做設定表勾選的項目。每個洞察寫明依據第幾則；重要性「高」最多三成；可信度遇到傳聞、單一來源、廠商新聞稿或業配一定標出來。
   **查證說明**：頁尾寫「本期 N 則：交叉查證 N 則、第一手 N 則、單一來源 N 則；排除 N 則（原因）」。
5. **套版**：使用同資料夾的 `template.html`，風格與配色已經設定好，不要改顏色。照檔案開頭註解填入內容。
6. **存檔**：檔名 `{{新聞中心名稱}}_YYYY-MM-DD.html`。能存到電腦時，存到 `{{存放資料夾，例：文件/我的新聞中心}}`；不能存檔就直接給我下載。
7. **寄送**：{{收件方式的做法：存成 HTML／轉成 PDF（不能轉就請我用瀏覽器列印另存 PDF）／把精簡版寄到我的信箱 {{信箱}}}}。只寄給我自己。
8. **通知訊息**：在對話裡回覆：
   ```
   ☀️ {{稱呼}}，今天的{{新聞中心名稱}}來了（N 則）
   ⚡ 今日三重點
   1. …
   2. …
   3. …
   👉 完整新聞：〔HTML 檔〕
   ```

## 底線規則（每次都要遵守）

- **只寫這次真的搜尋到的報導**，每則都要有媒體名稱、發布時間、可點的原文連結。絕不憑記憶寫新聞，絕不編造數字或連結。
- 搜到的不夠 {{N}} 則就少給，並在頁尾說明哪個主題今天新聞較少。
- 洞察與判斷是推論，一律標示，不寫成事實。
- 不自動寄信給我以外的人。
- 不給投資買賣建議、不給醫療或法律個案建議。
- 交付前檢查：連結都打開過、日期都在範圍內、數字跟原文一致、三重點和下方內容一致、頁尾有查證說明。
- 我回報錯誤時，先修正、再說明錯在哪；同一個來源錯兩次，問我要不要加進排除名單。

## 修改設定

我說「改新聞中心設定」或「加一個主題」「換顏色」時：
1. 問清楚要改哪一項，改完把新的「我的設定」表給我確認
2. 換顏色時，同時改 `template.html` 開頭 `:root` 的色票；換風格時，請我重新跑一次 news-center-builder 的第 4 輪
3. 改完重新打包這個資料夾成 zip 給我，提醒我重新上傳覆蓋舊版

````
