# PPT SVG Code Paste

繁體中文 | [English](README.en.md)

PowerPoint 工作窗格增益集：貼上 SVG (XML) 程式碼，直接以向量圖插入目前的投影片，不用先存成 .svg 檔再匯入。

適合把 AI 生成或從圖示庫複製來的 SVG 程式碼，快速放進簡報，而不用再透過增益集的 Claude for PowerPoint 的助理插入圖案。

本人之主要目的就是跳過等待 Claude 插入XML代碼的思考與執行時間，因為這對我來說是一個很常用的功能，但逐一下載 SVG 圖檔再拖入對應的簡報頁面又很麻煩。

![SVG Code Paste 在 PowerPoint 中的畫面：功能區的「SVG 插入」按鈕、右側窗格貼上程式碼與預覽，插入後的向量圖示位於投影片中央](docs/screenshot.png)

示例圖中的圖示 (耳機、階梯) 取自 [Phosphor Icons](https://phosphoricons.com/)，採 [MIT 授權](https://github.com/phosphor-icons/core/blob/main/LICENSE)。

## 功能

- 右側窗格貼上程式碼，即時預覽 (棋盤格背景可看出透明區)
- 自動去掉從聊天複製時夾帶的 ``` 標記，缺少 `xmlns` 時自動補上，XML 格式錯誤會提示
- 可設定插入寬度 (pt，0 = 原尺寸)，Ctrl+Enter 插入，可勾選插入後清空
- 插入結果是 SVG 圖形 (向量)，可在 PPT 裡右鍵「轉換為圖形」後改色、拆解
- 技術上使用 Office.js `setSelectedDataAsync` 搭配 `Office.CoercionType.XmlSvg`
- 介面語言 (窗格與功能區按鈕) 跟隨 Office 顯示語言，中文顯示繁中，其他語系一律顯示英文

## 系統需求

- Windows 版 PowerPoint (Microsoft 365 / 2019 以後，需支援 SVG)
- 窗格頁面由 GitHub Pages 提供，使用時需要連網

## 安裝 (共用資料夾目錄)

這個增益集沒有上架 Office 市集，要用微軟的「共用資料夾目錄」(Shared Folder Catalog) 方式安裝：在信任中心登記一個放 manifest 的網路共用路徑，PowerPoint 每次啟動都會從那裡載入，重開後也會保留。

1. **下載 manifest**：建立一個專放增益集 manifest 的資料夾，例如 `D:\OfficeAddins\ppt-svg-code-paste\`，把本倉庫的 [`manifest.xml`](manifest.xml) 存進去。這個資料夾之後不要搬動。

2. **取得網路共用路徑**：目錄只接受網路路徑 (`\\電腦名稱\共用名稱\...`)，不接受 `D:\...` 這種本機路徑。二選一：
   - **用系統內建的管理共用 (不用另外設定)**：如果你的帳號是系統管理員群組成員，Windows 預設會把每顆磁碟共用成 `磁碟代號$`。上例的路徑就是 `\\localhost\D$\OfficeAddins\ppt-svg-code-paste`。可以先在檔案總管網址列貼上這串路徑，能打開就代表可用。
   - **自己開共用資料夾**：在資料夾上按右鍵「內容 > 共用 > 共用」，把自己加入並給讀取權限，完成後視窗會顯示網路路徑 (例如 `\\你的電腦名稱\ppt-svg-code-paste`)。

3. **登記到信任中心**：PowerPoint「檔案 > 選項 > 信任中心 > 信任中心設定 > 受信任的增益集目錄」，在「目錄 URL」貼上第 2 步的網路路徑，按「新增目錄」，勾選該列的「顯示於功能表中」，按確定。

4. **重開 PowerPoint 並加入**：完全關閉 PowerPoint 再開啟，到「常用 > 增益集」(或「插入 > 我的增益集」) 的「共用資料夾」分頁，選 SVG Code Paste 按「新增」。

5. **之後使用**：「常用」索引標籤最右邊會出現「SVG 插入」按鈕，每份簡報都能用，按下去開啟右側窗格。

### 移除

在信任中心的「受信任的增益集目錄」清單選取這個路徑，按「移除」，再重開 PowerPoint。

## 疑難排解

| 狀況 | 原因與解法 |
| --- | --- |
| 跳出「此增益集已停用：在開發期間插入的增益集只能在從 Visual Studio 偵錯時使用」 | 這個增益集是用開發者側載 (`npx office-addin-dev-settings sideload` 或登錄檔 `WEF\Developer`) 裝的。開發者側載只在當次有效，PowerPoint 重開後會被停用。請先執行 `npx office-addin-dev-settings unregister <manifest 路徑>` (或刪除 `HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\WEF\Developer` 底下名稱為 `4fa1aa32-29a9-4519-82dd-e7d9b2a5e3cc` 的值)，再改用上方的共用資料夾目錄安裝。 |
| 「共用資料夾」分頁看不到增益集 | 確認信任中心該路徑有勾「顯示於功能表中」、PowerPoint 有完全重開，且路徑底下直接就是 `manifest.xml`。 |
| 窗格空白或顯示載入失敗 | 確認可以連到 <https://ag-0814.github.io/ppt-svg-code-paste/taskpane.html>。 |
| 插入按鈕是灰色、顯示「沒有連上 PowerPoint」 | 頁面是在一般瀏覽器裡開的。只有在 PowerPoint 的窗格裡才能插入。 |
| 介面語言不對 | 在頁面網址後加 `?lang=zh` 或 `?lang=en` 可強制指定語言 (方便在瀏覽器裡測試)。 |

如果仍有其他問題歡迎提 Issue 或 Pull Request。

## 檔案說明

| 檔案 | 用途 |
| --- | --- |
| `manifest.xml` | 增益集設定：窗格網址、功能區按鈕、圖示 |
| `taskpane.html` | 窗格本體 (單一 HTML，無建置步驟) |
| `assets/` | 功能區與增益集清單用的圖示 |
| `docs/` | README 用的示意截圖 |

## 授權

本專案以 MIT License 釋出，詳見 [LICENSE](LICENSE)。
