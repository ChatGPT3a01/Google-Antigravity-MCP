✅ 申請 Netlify Token（步驟）
方法 A：用 CLI 取得 Token（最快方式）

適合：你已安裝 Node.js、已能使用終端機（CMD / PowerShell / VSCode Terminal）

Step 1：安裝 Netlify CLI
npm install -g netlify-cli

Step 2：執行登入指令取得 Token
netlify login

接下來：

你的瀏覽器會自動開啟一個頁面

系統會叫你登入 Netlify

你必須按下「Authorize」

完成後 CLI 會自動存 Token

或者

Netlify CLI 回應：

Already logged in via netlify config on your machine

意思是：

🔹 你之前已經登入過 Netlify（可能是以前操作過）
🔹 CLI 已經有 Token，不需要再登入
🔹 Token 已經存在本機的 config.json 裡
🔹 Netlify CLI 現在處於「已登入」狀態

Step 3：Token 會自動存到電腦

在 Windows 系統，它會存這裡：

C:\Users\你的使用者名稱\AppData\Roaming\netlify\Config\config.json


你可以開啟看到類似：

{
  "accessToken": "YOUR_TOKEN_HERE"
}


這就是 Netlify Token！
CLI 之後會自動使用，不用自己手動貼。


也可以查看你目前登入的帳號資訊

可以執行：

netlify status


會顯示：

目前登入的 Netlify 帳號

Token 來源

正在連動的 site 資訊（如果有）

PS D:\> netlify link
✅ 你看到的選單
? How do you want to link this folder to a project?
> Search by full or partial project name
  Choose from a list of your recently updated projects
  Enter a project ID

🔍 選項 1：Search by full or partial project name

（搜尋專案名稱）

適合：你知道 Netlify 專案的名稱
例： vibecoding-site、ollama-chatbot-demo

選這個後，你會輸入專案名稱的部分字，它就會搜尋符合的專案，讓你選。

🔍 選項 2：Choose from a list of your recently updated projects

（從最近更新的專案清單中選）

適合：你不確定名字，但你知道是哪個專案

它會列出你 Netlify 帳號下最近部署過的網站
你從清單選一個即可。

✔ 最常用
✔ 最簡單
✔ 新手最方便

🔍 選項 3：Enter a project ID

（手動輸入專案 ID）

適合：你知道 Site ID / Project ID
（例如在 Netlify 後台 General → Project information 裡）

格式像：

d23k1d9e-19ab-455b-a2e5-xxxxx


只有你需要直接連結特定專案時才會用到。

✨ 小結：你應該選哪個？

👉 如果你知道專案名稱 → 選第一個
👉 如果想從清單挑選 → 選第二個（最方便）
👉 如果你要連結特定 ID → 用第三個

大多數人會選：

✔ 「Choose from a list of your recently updated projects」

最安全、最不會選錯。

❗ 連結完成後會發生什麼？

Netlify 會在你的專案資料夾裡新增：

.netlify/state.json


裡面會寫入：

{
  "siteId": "xxxxxx-xxxx-xxxx-xxxx"
}


從此之後你就可以直接：

netlify deploy


或：

netlify deploy --prod


完全不用再設定。

從電腦檔案直接讀出 Token（最直接、最準確）

Netlify CLI 會把 Token 存在你的電腦裡：

📍 Windows Token 位置：
C:\Users\你的使用者名稱\AppData\Roaming\netlify\Config\config.json


例如你的帳號，是：

C:\Users\user\AppData\Roaming\netlify\Config\config.json

🔍 打開方式：

Press Win + R

輸入：

%APPDATA%\netlify\Config


Enter

打開 config.json

你會看到內容像：

{
  "accessToken": "YOUR_NETLIFY_TOKEN"
}


這一串 "YOUR_NETLIFY_TOKEN" 就是你的 Token。

開啟antigravity 的MAC server 中的 Netlify 後 按下 install
填入NETLIFY_TOKEN
畫面顯示：

「Saving to config and reloading MCP server list.」

這代表：

✔ 你的 Netlify MCP Server 已成功安裝

✔ 你的 Token 已成功寫入 MCP config

✔ antigravity（或 Cursor / Windsurf / Claude Desktop）已經開始重新載入 MCP servers

✔ AI 現在已具備 “控制 Netlify 的能力”

你剛剛做的完全正確，並且操作是成功的！

🧪 Step 1：確認 MCP 是否成功啟動

在 antigravity 的聊天框輸入：

mcp list


或：

list mcp servers


你應該會看到：
The list_resources capability seems to be unavailable or not supported by the current Netlify MCP server connection. However, I have access to the following Netlify tools which indicate the server is active:

Available Netlify MCP Tools:

mcp_netlify_netlify-coding-rules: Check coding rules for serverless/edge functions.
mcp_netlify_netlify-deploy-services-reader: Read deploy information.
mcp_netlify_netlify-deploy-services-updater: Deploy sites.
mcp_netlify_netlify-extension-services-reader: Read extension details.
mcp_netlify_netlify-extension-services-updater: Manage extensions.
mcp_netlify_netlify-project-services-reader: detailed project/form info.
mcp_netlify_netlify-project-services-updater: Update project settings/env vars.
mcp_netlify_netlify-team-services-reader: Read team info.
mcp_netlify_netlify-user-services-reader: Read user info.
If you were trying to run a specific mcp CLI command, please let me know, otherwise, I am ready to use these tools to help you with Netlify tasks.
✅ Netlify MCP Server 已成功啟動，而且 Token 已成功連線！

右側 AI 回應中列出了所有可用工具：

像是：

mcp_netlify_netlify-coding-rules

mcp_netlify_netlify-deploy-services-reader

mcp_netlify_netlify-deploy-services-updater

mcp_netlify_netlify-project-services-reader

mcp_netlify_netlify-project-services-updater

mcp_netlify_netlify-team-services-reader

mcp_netlify_netlify-user-services-reader

這些就是 Netlify MCP 的 API 介面。

🎉 看到這些代表：你已經完成整個串接，成功！
✅ 接下來你可以開始用的真正 Netlify MCP 指令

你要知道：
在 MCP 裡不是自己打 command，而是對 AI 說出任務，AI 會替你呼叫工具。

例如：

🎯 示範 1：列出你的 Netlify 專案

在聊天輸入：

列出我所有 Netlify 專案


AI 就會使用：

netlify-project-services-reader

netlify-deploy-services-reader

來取得你的網站資訊。

🎯 示範 2：查某個網站的 Deploy 記錄
查看我某個 Netlify 網站最近 5 次部署紀錄

🎯 示範 3：部署新版本（若你有 repo）
幫我部署 Netlify 上的 [網站名稱] 最新版本

🎯 示範 4：佈署網站

1.將 ZIP 檔案放到 d:\Anti\@@@@ 資料夾中。
2.告訴Antigravity該 ZIP 檔案的名稱。
3.告訴Antigravity要建立新網站還是更新現有網站（如果是更新，請提供網站名稱）。




