# TASKS.md

## 專案
ScoreBoard / Badminton Web System

專案根目錄慣例：

```bash
/home/user123/Desktop/scoreboard_badminton
```

---

## 目前建議基線

### Control
目前 control 工作基線：

```text
web_scoreboard_control_rwd_with_members_next_game_team_select_auto_apply_fixed_polling_v4_nextgame.py
```

### Viewer
Viewer 目前工作基線：

```text
web_scoreboard_viewer_rwd_teamname_bigger_logo_marquee_toggle_live_logo.py
```

---

## 主要任務清單

### T01. 維持目前可用基線
- 不要用實驗版覆蓋已確認可用版本
- 所有新功能都應該基於目前 working baseline patch 上去
- 修改前先備份正式檔

### T02. Control RWD 穩定化
- 保持手機 / 平板 / 桌機可用
- iPhone Safari 不可雙擊放大
- 維持大按鈕、高可讀性
- 避免 UI 更新造成資料回跳

### T03. Team Name 同步穩定
- Team 1 / Team 2 可手動輸入
- 可從 Member 名單帶入
- 可從 Next Game 左右兩側帶入
- 避免被 `/api/state` 輪詢蓋回舊值

### T04. Next Game 流程穩定
- Next Game 可從 Member 名單選擇球員
- 支援單打 / 雙打
- 修改後應立即反映在 viewer 跑馬燈
- 避免 Next Game 區塊被輪詢蓋回舊值

### T05. Member 管理
- Member 名單可新增 / 編輯 / 儲存
- 資料落地到檔案
- Team 1 / Team 2 / Next Game 都共用同一份 member source
- 成員需支援分類：
  - 來賓
  - 正式會員
  - 臨打
- 未來可依分類做篩選、下拉選單分組、顯示標籤與統計

### T06. Viewer 跑馬燈
- 跑馬燈文字可由 control 編輯
- Next Game 可附加在跑馬燈中
- 字體可維持大字展示
- 可從 control 控制顯示 / 隱藏

### T07. Viewer Logo 管理
- Viewer logo 可由 control 頁上傳
- 新 logo 應能自動替換舊 viewer.* 檔
- viewer 應支援即時更新或快速刷新
- control 可控制 logo 顯示 / 隱藏

### T08. Viewer 畫面維持展示邏輯
- Fullscreen 時 logo 不消失
- 右側隊名固定在右側 card 右上角
- 左側隊名固定在左側 card 左上角
- 分數仍保持最大展示空間
- 跑馬燈字體保持醒目

### T09. Persistent Data 規範
資料建議存放於：

```bash
static/branding/
```

目前已知資料：

- `viewer_marquee.txt`
- `viewer_ui.json`
- `members.json`
  - 建議每位成員至少包含：
    - `name`
    - `category`（來賓 / 正式會員 / 臨打）
- `next_game.json`
- `viewer.png` / `viewer.svg` / `viewer.jpg` / `viewer.jpeg` / `viewer.webp`

### T10. systemd 部署流程維持一致
Control：

```bash
cp /home/user123/Desktop/scoreboard_badminton/web_scoreboard_control.py    /home/user123/Desktop/scoreboard_badminton/web_scoreboard_control.py.bak

cp <new_file>.py    /home/user123/Desktop/scoreboard_badminton/web_scoreboard_control.py

sudo systemctl restart scoreboard_control.service
sudo systemctl status scoreboard_control.service --no-pager
```

Viewer：

```bash
cp /home/user123/Desktop/scoreboard_badminton/web_scoreboard_viewer.py    /home/user123/Desktop/scoreboard_badminton/web_scoreboard_viewer.py.bak

cp <new_file>.py    /home/user123/Desktop/scoreboard_badminton/web_scoreboard_viewer.py

sudo systemctl restart scoreboard_viewer.service
sudo systemctl status scoreboard_viewer.service --no-pager
```

---

## 每次修改後最少測試清單

### Control
- 頁面可開啟
- 語言切換正常
- Team 1 / Team 2 名稱可儲存
- Member 下拉可看到資料
- Member 分類可正確儲存與顯示
- 不同分類成員未來可做分組篩選
- Next Game 可正常儲存
- 跑馬燈儲存正常
- Viewer UI toggle 正常
- Logo upload 正常

### Viewer
- 頁面可開啟
- Fullscreen 正常
- 左右隊名位置正確
- 比分正常更新
- 跑馬燈正常顯示
- Logo 顯示 / 隱藏正常
- Next Game 跑馬燈正常附加

---

## 已知注意事項

- 任何會從 `/api/state` 或 `/api/next_game` 輪詢更新 UI 的地方，都要小心 race condition
- 若使用者已明確說某個版本可用，就以那版為正式基線
- 不要把實驗功能直接混入正式基線，除非使用者明確同意

---

## 不建議繼續的版本

```text
web_scoreboard_control_rwd_team_cards_colored.py
```

原因：
- user 已明確表示這版有 next team entry 問題
- 視為實驗版，非正式基線
