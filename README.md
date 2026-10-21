# PPT SVG Code Paste

PowerPoint 工作窗格增益集：貼上 SVG (XML) 程式碼，直接以向量圖插入目前的投影片，不用先存檔再匯入。

- 即時預覽，自動去掉聊天複製夾帶的 ``` 標記、補上 `xmlns`
- 可設定插入寬度 (pt)，Ctrl+Enter 插入
- 使用 Office.js `setSelectedDataAsync` 搭配 `CoercionType.XmlSvg`，插入結果為 SVG 圖形，可在 PPT 裡「轉換為圖形」後改色

## 安裝 (Windows 桌面版 PowerPoint，側載)

1. 下載 `manifest.xml` 存到本機固定位置
2. 在登錄檔 `HKCU\Software\Microsoft\Office\16.0\WEF\Developer` 新增字串值，名稱填 manifest 的 `<Id>` (`4fa1aa32-29a9-4519-82dd-e7d9b2a5e3cc`)，資料填 manifest 的完整路徑 (建議用純英文路徑)
3. 重開 PowerPoint，「常用」索引標籤會出現「SVG 插入」按鈕

移除時刪掉該登錄值即可。

窗格頁面：<https://ag-0814.github.io/ppt-svg-code-paste/taskpane.html>
