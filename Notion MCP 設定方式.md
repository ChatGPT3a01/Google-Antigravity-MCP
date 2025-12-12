到 https://www.notion.so/profile/integrations  
🎯 以下是新版 Notion API + MCP 的正確設定（最終版）
🥇 第 1 步：建立 Notion Integration（支援 2025-09-03 版）

進入：
https://www.notion.so/profile/integrations

按：

➤ Create new integration

設定：

欄位	建議
Name	Antigravity MCP
Capabilities	✔ Read content
✔（可選）Insert content
✔（可選）Update content
API version	2025-09-03（新版 API）
Token	自動生成（ntn_xxxxxxx）

抄下 Token（等等要貼到 Antigravity）

🥇 第 2 步：把 Integration 加到頁面／資料庫（新版 Notion 強制要求）

每個你想讓 AI 讀的頁面 → 點右上角 ... →

➤ Connect to integration

選你剛建立的 Integration。

❗ 新版 Notion API 有強制規則：
如果 integration 沒加入某資料庫 → AI 連 data_source 都看不到

🥇 第 3 步：打開 Antigravity → Tools → MCP → Notion → Install

填入：

NOTION_TOKEN = ntn_xxxxxxxxxxxxxxxxxxxxx


然後按 Install。

Antigravity 會自動建構 config.json，內容會像：

{
  "mcpServers": {
    "notion": {
      "command": "npx",
      "args": ["-y", "@notionhq/notion-mcp-server"],
      "env": {
        "NOTION_TOKEN": "ntn_xxxxxx"
      }
    }
  }
}


不用你自己更動。

🥇 第 4 步：驗證 MCP 是否成功支援新版 API

在 Antigravity 對 AI 輸入：

notion.list_data_sources


或：

列出我的 Notion 資料庫


如果看到：

✔ object = "data_source"
✔ 包含 data_source_id
✔ 包含 database_parent

→ 表示 Antigravity 的 MCP 已成功接上 Notion 新 API。

🥇 第 5 步：MCP 如何自動處理 data_source_id（你不用處理）

你提供的新 API 文件寫得很清楚：

「所有 database_xxx API 已經改由 data_source_xxx 取代」

MCP Server 已經內建：

動作	MCP 會自動做什麼
讀資料庫	先查 database → 再取 data_sources → 回傳給 AI
查詢資料庫	自動呼叫 /v1/data_sources/:id/query
建立頁面	自動把 parent 改為 { data_source_id: xxx }
搜尋頁面	自動將回傳的 data_source 解析成頁面 ID

👉 你不需要寫任何 code、完全不需要自己管理 data_source_id。

🔧（進階）如果你真的想自己驗證 data_source_id：

可以對 AI 輸入：

取得名稱包含「課程」的資料庫的 data_source_id


AI 會回覆像：

database_id: 6c42...
data_source_id: a42a6...


這證明 MCP 確實使用新版 API。

🎉 帥氣的亮，你現在要我幫你做哪一件？

我可以幫你：

檢查你現在的 MCP 設定是否正確

測試你的 Notion MCP 是否成功接到新版 API

教你如何用語音 / 文字讓 AI 操作 Notion

協助你建立 Notion 資料庫自動化工作流

如果出現 error，我可以直接解出是哪個 data_source 出問題

你要我先做哪一個？

你說：
這個要填入什麼?
ChatGPT 說：

帥氣的亮：
你現在看到的這個欄位 「OAuth 網域與 URI → 重新導向 URI（Redirect URI）」
👉 在你做 MCP（Antigravity）整合時完全不用填！

因為：

✅ 你現在建立的是「Internal Integration（內部整合）」不是 OAuth App

也就是：

MCP（Antigravity）、OpenAI、CLI、自動化工具

全部只需要「Internal Integration Token」（ntn_xxx）

不需要 OAuth Login

不需要 Redirect URI

➡️ 所以這一格保持空白即可。什麼都不要填。

⭐ 什麼時候會需要填 Redirect URI？

只有當你做：

✔ OAuth Login
✔ 第三方網站要讓使用者登入 Notion
✔ Web app 需要 Notion 授權 Flow

例如：

要讓學生登入 Notion

做 Notion 連結社團 app

需要 Notion 自動回呼 callback URL

這些情境跟你現在做的 MCP（模型工具協定）完全無關。

❗ 你現在的目的 → 連接 MCP → 只要 Token，不需要 OAuth

你需要的是：

✔ Internal integration token（在上面 "機密金鑰" 那裡）

格式像：

ntn_xxxxxx_xxxxxxxxxxxxxx


複製後貼到：

Antigravity → Tool → MCP → Notion → NOTION_TOKEN

就完成了。

🔥 確保以下 3 個步驟就可以正常使用 MCP + Notion
① 建立 Integration（你已經做了）
② 把 Integration 加到 Notion 頁面 / 資料庫

（右上角「連接整合」）

③ 把 Token 填入 Antigravity MCP 設定

你不需要 Redirect URI。

✅ 你現在需要做的就是「把 Notion 頁面授權給 Integration」

下面我給你 一步一步最清楚操作，照做就會成功：

🧩 步驟 1：找出你剛建立的 Integration 名稱

在 Notion → 左側 → 整合（Integrations）
你剛才建立的 Integration 名稱會在此出現，例如：

ChatGPT-MCP

Antigravity

3a01chatgpt

或你自己設定的名稱

記住它的名字。

🧩 步驟 2：打開你要給 MCP 使用的 Notion 頁面／資料庫

例如：

一個資料庫（表格）

一個頁面（Page）

甚至整本筆記本

🧩 步驟 3：點右上角 ⋯（三個點）

會看到這個畫面：

⋯（更多）
Share（分享）
Connections（連線）

🧩 步驟 4：找到「Connections（連線）」

點進去後：

你會看到按鈕：

👉 Add connections（新增連線）

🧩 步驟 5：把你的 Integration 加進來

選擇 你剛建立的 Integration 名稱
按下：

✔ Allow Access（允許存取）

🧩 步驟 6：完成授權後回來跟我說：「授權完成」

我就可以幫你重新叫 MCP 搜尋資料庫，產生 data_source_id，並開始使用了。

🎉 你授權成功後，MCP 就可以做到：

讀取你的資料庫 schema

查詢資料（query）

新增頁面（create page）

建立自動化 pipeline

幫你更新資料庫

幫你在 Notion 做 AI 操作（搜集資料、整理、生成內容）

如果你願意，我可以 幫你檢查你授權是否正確

只要你說：

👉 「我已授權完成，可以搜尋 Notion」

我就會幫你測試 MCP 的連線狀態，確認是否有 data_source_id 返回，代表 MCP 安裝成功。