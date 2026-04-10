# TASKS.md

## Project
Harness Scoreboard / Badminton Web System

Repository guide source:
- `bigheadG/harness_scoreboard`
- Use the repo markdown guides as the ongoing scoreboard project reference. citeturn765848view0

---

## Current working baselines

### Control
```text
web_scoreboard_control_rwd_with_members_next_game_team_select_auto_apply_fixed_polling_v4_nextgame.py
```

### Viewer
```text
web_scoreboard_viewer_rwd_teamname_bigger_logo_marquee_toggle_live_logo.py
```

---

## Task list

### T01. Maintain working baseline
- patch from current accepted baseline
- do not overwrite working production files with unconfirmed experiments
- back up deployed files before replacement

### T02. Control RWD stability
- keep phone / tablet / desktop usable
- keep iPhone Safari double-tap zoom suppression
- maintain large touch-friendly controls
- avoid UI race conditions and value snap-back

### T03. Team name sync stability
- Team 1 / Team 2 can come from:
  - manual input
  - member list
  - next game left / right side
- after selection, names must not be overwritten by polling
- maintain optimistic update / polling suppression if needed

### T04. Next Game stability
- next game players come from member list
- support singles / doubles
- viewer marquee should reflect next game
- next game selections must not be overwritten by polling

### T05. Member management
- member list can add / edit / save
- persist to file
- Team 1 / Team 2 / Next Game share same member source
- member entries should support category

Required member categories:
- `來賓`
- `正式會員`
- `臨打`

### T06. Member category support
- `members.json` should support:

```json
[
  {"name": "王力宏", "category": "正式會員"},
  {"name": "阿妹", "category": "來賓"},
  {"name": "伊朗", "category": "臨打"}
]
```

- future UI should support:
  - category filter
  - grouped dropdowns
  - badge display
  - category statistics

### T07. Viewer marquee
- marquee text editable from control
- next game can append to marquee
- large readable display
- control can hide / show marquee

### T08. Viewer logo
- logo upload from control
- store as `viewer.*`
- remove old `viewer.*` when needed
- control can hide / show logo
- viewer should refresh logo quickly

### T09. Viewer display behavior
- fullscreen works
- logo can remain visible in fullscreen
- left team name fixed left
- right team name fixed right
- score remains largest visual priority

### T10. Persistent file conventions
Under `static/branding/`:
- `viewer_marquee.txt`
- `viewer_ui.json`
- `members.json`
- `next_game.json`
- `viewer.*`

### T11. systemd deployment
Control:
```bash
cp /home/user123/Desktop/scoreboard_badminton/web_scoreboard_control.py \
   /home/user123/Desktop/scoreboard_badminton/web_scoreboard_control.py.bak

cp <new_file>.py \
   /home/user123/Desktop/scoreboard_badminton/web_scoreboard_control.py

sudo systemctl restart scoreboard_control.service
sudo systemctl status scoreboard_control.service --no-pager
```

Viewer:
```bash
cp /home/user123/Desktop/scoreboard_badminton/web_scoreboard_viewer.py \
   /home/user123/Desktop/scoreboard_badminton/web_scoreboard_viewer.py.bak

cp <new_file>.py \
   /home/user123/Desktop/scoreboard_badminton/web_scoreboard_viewer.py

sudo systemctl restart scoreboard_viewer.service
sudo systemctl status scoreboard_viewer.service --no-pager
```

---

## Minimum test checklist

### Control
- page loads
- language switch works
- Team 1 / Team 2 names save correctly
- member dropdown shows data
- next game saves correctly
- next game is not overwritten by polling
- marquee save works
- viewer UI toggle works
- logo upload works
- member category saves and reloads correctly
- category label / filter / grouping features behave correctly when implemented

### Viewer
- page loads
- fullscreen works
- team name alignment is correct
- logo visibility works
- marquee visibility works
- next game appears correctly in marquee
