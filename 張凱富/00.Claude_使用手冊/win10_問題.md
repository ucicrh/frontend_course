C:\Users\USER\.local\bin
一、 安裝 Claude Desktop 桌面版失敗的解決方法

如果您下載的是 Claude Desktop（.exe / .msix 安裝檔）點擊後毫無反應或無法啟動：

1. 確認 Windows 10 系統版本：
   • Claude Desktop 要求 Windows 10（建議版本 1809 或更高版本）。請按 Win + R 輸入 winver 檢查您的 Windows 10 是否過舊，若版本太低需先透過 Windows Update 更新系統。 [1] (https://kevin.voyage/claude-code-install/)
2. 啟用 Windows「開發人員模式」：
   • 部分使用者在安裝 MSIX 封裝時會被阻擋。請前往 設定 > 隱私權與安全性（或更新與安全性）> 開發人員專用，將「開發人員模式」開啟，再重新點擊安裝檔。 [1] (https://www.reddit.com/r/ClaudeAI/comments/1rrm489/claude_desktop_cannot_install_on_windows/?tl=zh-hant)
3. 清除背景殘留程序：
   • 按 Ctrl + Shift + Esc 打開「工作管理員」，搜尋是否有背景殘留的 claude 相關行程，請將其「結束工作」再重新安裝。
4. 完整重新下載安裝檔：
   • 請至官方頁面 Anthropic Help Center 重新取得最新的安裝套件，避免下載到不完整或損壞的檔案。 [1] (https://support.claude.com/zh-TW/articles/10065433-%E5%AE%89%E8%A3%9D-claude-desktop)

二、 安裝 Claude Code（命令列工具）失敗的解決方法

如果您是透過 PowerShell 或終端機安裝 Claude Code 遇到問題：

1. Windows PowerShell 安裝指令：
   • 請用系統管理員身分開啟 PowerShell，並輸入官方建議的安裝指令：powershell
   irm https://claude.ai/install.ps1 | iex
   請謹慎使用程式碼。
   • 安裝完成後，請務必關閉 PowerShell 視窗並重新開一個，再輸入 claude --version 測試。 [1] (https://www.coolkidlab.com/newbie-pitfalls/claude-code-windows-setup.html)
2. 出現「無法辨識的詞彙」錯誤：
   • 代表 Node.js 或路徑（PATH）環境變數尚未重新載入，務必確實關閉終端機並重新啟動。 [1] (https://www.coolkidlab.com/newbie-pitfalls/claude-code-windows-setup.html)

常見問題摘要比對表

安裝類型 常見無法安裝原因 建議排查與解決步驟
Claude Desktop Windows 版本過舊 / MSIX 阻擋 / 程式卡住 檢查 winver、開啟「開發人員模式」、工作管理員砍背景程序
Claude Code 終端機未重啟 / 權限不足 / PowerShell 執行原則阻擋 系統管理員開 PowerShell 執行 irm 指令，重啟終端機

如果畫面上有跳出特定的錯誤代碼（例如 0x8007... 或類似訊息），或你是安裝 Claude Desktop 還是 Claude Code 遇到困難，可以補充跟我說，我幫你精準排查！
