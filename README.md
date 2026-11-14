# minecraft_server_manager
A minecraft server manager programed by claude. Even new players can host a server by using a easily controled localhost website.

# Minecraft 伺服器網頁管理器

在一台電腦上用瀏覽器管理**多台** Minecraft Java 版伺服器，區網內的手機或電腦也能連進來操作。

## 需求
- **Python 3.8 以上**（不用安裝任何套件）
- 不需要自己裝 Java：程式會依照 Minecraft 版本自動下載需要的 Java（8 / 17 / 21 / 25…）

## 啟動
- Windows：雙擊 `start.bat`
- macOS / Linux：`./start.sh`（或 `python3 manager.py`）

畫面會顯示網址，例如：
```
本機：  http://localhost:8080
區網：  http://192.168.1.23:8080   ← 同一個 Wi-Fi 的手機/電腦用這個
```
> Windows 第一次執行時防火牆會跳出詢問，請允許「私人網路」。
> 朋友要連進 Minecraft 伺服器，用的是 `電腦IP:伺服器連接埠`（例如 192.168.1.23:25565）。

## 關閉
- 網頁右上角「⏻ 關閉管理程式」：所有伺服器會先執行 `stop`（自動存檔）再結束
- 或在黑色視窗按 `Ctrl+C`

## 功能
| 功能 | 說明 |
|---|---|
| 建立伺服器 | 選擇運行器（Vanilla / Paper / Purpur / Fabric / Forge / NeoForge）與版本，自動從官方下載 |
| 獨立 Java | 下載到 `data/runtimes/java-XX/`，只用完整路徑啟動，**不修改 PATH、JAVA_HOME 或系統設定** |
| 多開 | 每台伺服器各自的資料夾、連接埠、記憶體、Java 版本，各自有啟動/關閉/存檔按鈕 |
| 主控台 | 即時記錄；直接打字 = `say 文字`，`/` 開頭 = 指令；Tab 自動補全、↑↓ 歷史紀錄 |
| 玩家 | 在線/離線玩家 ID、UUID、最後上線時間，可**備註名稱**；一鍵踢出 / OP / 封鎖 |
| 地圖 | 讀取存檔畫出俯視地形，顯示在線玩家即時位置與離線玩家最後位置（1.13 以上） |
| 檔案 | 拖曳上傳模組/插件 .jar、地圖 .zip、資料包；模組可停用/啟用；世界可切換、備份下載 |
| server.properties | 網頁表單編輯（含中文說明） |
| Discord | 每台伺服器可填 Webhook 網址，玩家加入/離開/聊天/開關機會推送到頻道 |

## 設定檔 `data/config.json`
```json
{
  "host": "0.0.0.0",
  "port": 8080,
  "password": "",
  "user_agent": "MCWebManager/1.0 (self-hosted Minecraft server manager)"
}
```
- `password`：設定後開網頁會要求登入（帳號隨便填，密碼填這個）。**區網有其他人時建議設定。**
- `port`：管理網頁的連接埠。
- 修改後重新啟動管理程式生效。

## 資料夾結構
```
data/
  servers/<id>/      每台伺服器的完整檔案（世界、mods、plugins…）
    mcm.json         管理器設定（名稱、記憶體、Java…）
    mcm_players.json 玩家備註
  runtimes/java-21/  獨立的 Java
  config.json
```
要搬家或備份，複製整個資料夾即可。

## 注意事項
- 地形來自存檔檔案：新探索的區域按「存檔並更新地形」才會出現。
- 即時玩家座標使用 `data get entity` 指令（1.13 以上），這些查詢不會出現在主控台。
- Forge / NeoForge 安裝時會執行官方安裝程式，需要幾分鐘。
- Fabric 第一次啟動時會再下載原版核心，請保持網路連線。
- 不要把管理網頁的連接埠開放到網際網路（路由器不要轉發 8080）。

## 未來擴充 Discord Bot
`mcm/instance.py` 裡的 `Instance.discord()` 是對外推播的入口，`Instance.send()` 可送指令、
`Instance.subscribe()` 可訂閱主控台。要做雙向（Discord 打字 → 遊戲內）時，寫一個 bot 呼叫
`POST /api/servers/<id>/command`（內容 `{"command": "文字或/指令"}`）即可。
