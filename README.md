# 📰 新聞中心建造師

> **給讀到這頁的 AI：** 使用者把這個 GitHub 連結給你，是想用問答建立自己的新聞中心。請讀 https://raw.githubusercontent.com/linlin-botbot/news-center-builder/main/all-in-one.md 的完整內容（含附錄），照裡面的流程從「步驟 0：開場」開始帶他做，不要先總結這一頁。

用問答的方式，30 分鐘打造**專屬你的新聞中心**：

- 依你的職業和關注主題，每天整理新聞重點
- 每則都附原文出處，還有「💡 跟你有什麼關係」
- AI 幫你多想一步：趨勢、影響、機會、風險，標出重要性和可信度
- 三種風格任選：簡約版、報紙風、雜誌風，或用你的品牌色自訂
- 用網頁、Email 或 PDF 收到，每天或每週固定時間自動整理

👉 看範例成品：[簡約版](https://htmlpreview.github.io/?https://github.com/linlin-botbot/news-center-builder/blob/main/example/sample-minimal.html)・[報紙風](https://htmlpreview.github.io/?https://github.com/linlin-botbot/news-center-builder/blob/main/example/sample-times.html)・[雜誌風](https://htmlpreview.github.io/?https://github.com/linlin-botbot/news-center-builder/blob/main/example/sample-magazine.html)

---

## 方法一：把連結給 AI（最快，推薦）

打開 Claude，開新對話，把下面這段貼上送出：

```
請讀這個連結，照裡面的流程帶我建立新聞中心：
https://github.com/linlin-botbot/news-center-builder
```

AI 會直接開始問你問題。如果它說讀不到，改貼這段（直接指到說明檔）：

```
請讀這個網址的完整內容，然後照著裡面的流程帶我建立新聞中心：
https://raw.githubusercontent.com/linlin-botbot/news-center-builder/main/all-in-one.md
```

> 需要 AI 能讀網頁（Claude 要開啟網路搜尋）。做完一樣會幫你打包出自己的專屬 skill。
> 用 Claude Code 或 Cowork 這類能存檔的環境，AI 會先問你要不要把建造師安裝起來，以後直接說「幫我建新聞中心」就好。

## 方法二：下載 zip 安裝成 Claude Skill

想長期留著建造師、之後幫同事或其他主題再建一個，用這個方法。

1. **[⬇️ 點這裡下載 news-center-builder.zip](https://github.com/linlin-botbot/news-center-builder/releases/latest/download/news-center-builder.zip)**（下載後**不要解壓縮**）
2. 打開 Claude →「設定」→「功能」，確認「程式碼執行與檔案建立」已開啟（新版介面叫「建立與編輯檔案」）
3. 到「自訂」→「Skills」
4. 按「＋」→「建立 skill」→「上傳 skill」，選剛剛的 zip
5. 開新對話，輸入：

```
幫我建新聞中心
```

## 方法三：用 ChatGPT 或 Gemini

複製 [gpt-gemini-instructions.md](gpt-gemini-instructions.md) 裡的指令，貼到 Custom GPT 或 Gem 的「指示」欄位。

---

## 做完你會得到

| 成果 | 說明 |
|---|---|
| 第一份新聞報告 | 依你的主題分類，用你選的風格 |
| 你的專屬 skill | 例如 `amy-news-center`，以後說「今天的新聞」就會產出 |
| 自動收報 | 每天或每週固定時間，用網頁、Email 或 PDF 收到 |

想改主題、加關鍵字、換風格，隨時跟 AI 說「幫我改新聞中心設定」。

## 遇到問題

| 狀況 | 試試看 |
|---|---|
| 上傳失敗 | 確認是 zip、沒解壓縮過、「程式碼執行與檔案建立」有開 |
| AI 沒開始問問題 | 說「使用 news-center-builder skill，幫我建新聞中心」 |
| 新聞太少 | 主題放寬一點，或時間範圍改成過去 3 天 |
| 連結打不開 | 跟 AI 說「第 X 則連結打不開，換一則」 |
| 沒收到報告 | 到排程頁面看執行紀錄；設定時選了電腦裡的資料夾，電腦和 App 要開著 |

## 檔案說明

```
news-center-builder/        Claude Skill 原始檔（Releases 裡的 zip 就是這個資料夾）
all-in-one.md               單一檔案版，給「方法一」把連結給 AI 用
gpt-gemini-instructions.md  ChatGPT／Gemini 版指令
example/                    三種風格的範例報告（簡約、報紙、雜誌）
```
