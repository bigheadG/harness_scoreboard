# SKILLS.md

## Project Skills
ScoreBoard / Badminton Web Project

---

## 1. Flask Web App 維護
適用範圍：
- `web_scoreboard_control.py`
- `web_scoreboard_viewer.py`

技能內容：
- 單檔 Flask app 維護
- `render_template_string` / API route 調整
- 表單、JSON API、檔案上傳
- 靜態檔案與 `static/branding/` 資料管理

---

## 2. MQTT 同步技能
適用範圍：
- score sync
- color sync
- reset sync
- viewer meta refresh

技能內容：
- MQTT publish / reconnect
- topic-based scoreboard state sync
- paho-mqtt 整合
- 控制端與 viewer 間的即時資料同步

---

## 3. RWD 前端技能
適用範圍：
- control 頁
- viewer 頁

技能內容：
- 手機 / 平板 / 桌機版面適配
- iPhone Safari safe-area 處理
- 大按鈕 UI
- 觸控互動優化
- 橫式 / 直式版面切換
- Fullscreen 展示型 UI

---

## 4. iPhone Safari 相容技能
重點：
- 禁止 double-tap zoom
- `touch-action: manipulation`
- `viewport-fit=cover`
- `user-scalable=no`
- safe-area padding
- 避免按鈕誤觸與放大

---

## 5. Viewer 展示 UI 技能
重點：
- 左右隊名角落定位
- 超大比分顯示
- 跑馬燈展示
- Logo 展示與隱藏
- Fullscreen 狀態下的 topbar 控制
- RWD for TV / laptop / tablet / phone

---

## 6. Control 操作 UI 技能
重點：
- Team 1 / Team 2 控制
- local swap / both swap
- sync / reset / set
- color / name editing
- member list 選擇
- next game 編輯
- marquee 編輯
- logo 上傳
- viewer 顯示控制

---

## 7. 檔案型資料持久化技能
建議存放路徑：

```bash
static/branding/
```

技能內容：
- `.json` 儲存設定資料
- `.txt` 儲存 marquee 文字
- 圖片檔儲存 logo
- 簡單、可人工編輯、易部署

資料類型：
- `viewer_marquee.txt`
- `viewer_ui.json`
- `members.json`
- `next_game.json`
- `viewer.*`

---

## 8. Team / Member / Next Game 流程技能
技能內容：
- 從 member list 填入 team 名稱
- 從 next game 左右邊帶入 team 名稱
- 單打 / 雙打模式名稱組裝
- 自動儲存與 optimistic update
- 避免輪詢覆蓋新資料

---

## 9. 輪詢覆蓋問題處理技能
這是本專案的重要技能點。

問題來源：
- control 頁會週期性呼叫 `/api/state`
- next game 區塊會週期性呼叫 `/api/next_game`
- 使用者剛修改時，後端尚未完全同步，舊資料可能被輪詢帶回

解法技能：
- optimistic UI update
- pending state 暫存
- 暫停 polling 一小段時間
- 等後端 state 追上後再恢復正常 render

---

## 10. 檔案上傳技能
目前用途：
- viewer logo upload

技能內容：
- `request.files`
- 副檔名驗證
- 取代舊 `viewer.*`
- 回傳新 logo meta 資訊
- viewer live refresh

---

## 11. systemd 部署技能
服務：
- `scoreboard_control.service`
- `scoreboard_viewer.service`

技能內容：
- 正式檔覆蓋前備份
- restart / status 檢查
- 維持 deploy 流程一致
- 避免錯誤版本混入正式環境

---

## 12. 專案維護原則
- 以使用者確認可用的版本為基線
- 優先小修，不做大改
- control 問題就修 control，不亂動 viewer
- viewer 問題就修 viewer，不亂動 control
- 實驗版不要直接當正式版
- 每次修改都要可直接部署

---

## 13. 建議後續可擴充技能
- 多 court / 多場 next game
- 排隊賽程表
- 成員分類（來賓 / 正式會員 / 臨打）
- 比賽歷史紀錄
- 控制頁管理帳號 / 權限
- 匯入 / 匯出 member 名單
