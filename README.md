# 📰 新聞中心建造師

用問答的方式，20 分鐘打造**專屬你的每日新聞中心**：

- 依你的職業和關注主題，每天整理新聞重點
- 每則都附原文出處，還有「💡 跟你有什麼關係」
- 整理成一頁網頁（HTML），隨時打開
- 每天固定時間，透過 Claude、ChatGPT 或 Gemini 通知你

👉 [看範例成品](https://htmlpreview.github.io/?https://github.com/linlin-botbot/news-center-builder/blob/main/example/sample-news.html)

---

## 方法一：安裝成 Claude Skill（推薦）

1. **[⬇️ 點這裡下載 news-center-builder.zip](https://github.com/linlin-botbot/news-center-builder/releases/latest/download/news-center-builder.zip)**（下載後**不要解壓縮**）
2. 打開 Claude → 左下角頭像 →「設定」→「功能」
3. 確認「程式碼執行與檔案建立」已開啟
4. 在「Skills」按「上傳 skill」，選剛剛的 zip
5. 開新對話，輸入：

```
幫我建新聞中心
```

## 方法二：不安裝，直接貼網址

上傳卡住、或想先試試看，把下面整段複製貼到 Claude 的新對話：

```
請讀這個網址的完整內容，然後照著裡面的流程帶我建立新聞中心：
https://raw.githubusercontent.com/linlin-botbot/news-center-builder/main/all-in-one.md
```

> 需要 Claude 能存取網頁。這個方法只在這次對話有效，但最後一樣會幫你打包出自己的專屬 skill。

## 方法三：用 ChatGPT 或 Gemini

複製 [gpt-gemini-instructions.md](gpt-gemini-instructions.md) 裡的指令，貼到 Custom GPT 或 Gem 的「指示」欄位。

---

## 做完你會得到

| 成果 | 說明 |
|---|---|
| 今天的新聞網頁 | 依你的主題分類、配色與版面 |
| 你的專屬 skill | 例如 `amy-news-center`，以後說「今天的新聞」就會產出 |
| 每日通知 | 每天固定時間自動整理新聞 |

想改主題、加關鍵字、換顏色，隨時跟 AI 說「幫我改新聞中心設定」。

## 遇到問題

| 狀況 | 試試看 |
|---|---|
| 上傳失敗 | 確認是 zip、沒解壓縮過、「程式碼執行與檔案建立」有開 |
| AI 沒開始問問題 | 說「使用 news-center-builder skill，幫我建新聞中心」 |
| 新聞太少 | 主題放寬一點，或時間範圍改成過去 3 天 |
| 連結打不開 | 跟 AI 說「第 X 則連結打不開，換一則」 |
| 早上沒收到通知 | Claude 桌面 App 要開著、電腦不能關機 |

## 檔案說明

```
news-center-builder/        Claude Skill 原始檔（Releases 裡的 zip 就是這個資料夾）
all-in-one.md               單一檔案版，給「方法二」貼網址用
gpt-gemini-instructions.md  ChatGPT／Gemini 版指令
example/sample-news.html    範例成品
```
