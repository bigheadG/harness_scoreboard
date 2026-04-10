# SKILLS.md

## Project Skills
Harness Scoreboard / Badminton Web Project

Repository guide source:
- `bigheadG/harness_scoreboard`
- Follow the repo markdown files as the scoreboard project working handbook. citeturn765848view0

---

## 1. Flask web app maintenance
Scope:
- `web_scoreboard_control.py`
- `web_scoreboard_viewer.py`

Skills:
- single-file Flask app maintenance
- `render_template_string` page updates
- JSON API design
- file upload handling
- simple persistent file storage

---

## 2. MQTT sync
Scope:
- score sync
- color sync
- reset sync
- viewer meta refresh

Skills:
- paho-mqtt integration
- reconnect handling
- control-to-viewer sync
- viewer refresh triggers

---

## 3. Responsive web design
Scope:
- control page
- viewer page

Skills:
- phone / tablet / desktop layout
- fullscreen display layout
- safe-area support
- touch-friendly controls
- portrait / landscape adaptation

---

## 4. iPhone Safari compatibility
Skills:
- disable double-tap zoom
- `touch-action: manipulation`
- `viewport-fit=cover`
- safe-area padding
- prevent accidental gesture interference

---

## 5. Viewer display UI
Skills:
- left / right corner team name positioning
- large score-first layout
- marquee display
- logo display and hide/show
- fullscreen topbar behavior
- live logo refresh support

---

## 6. Control UI
Skills:
- Team 1 / Team 2 score controls
- local swap / both swap
- sync / set / reset
- team name / color editing
- member management
- next game editing
- marquee editing
- logo upload
- viewer display toggles

---

## 7. File-based persistence
Recommended location:

```text
static/branding/
```

File types:
- `viewer_marquee.txt`
- `viewer_ui.json`
- `members.json`
- `next_game.json`
- `viewer.*`

---

## 8. Member / Team / Next Game data structures
Recommended `members.json` structure:

```json
[
  {
    "name": "王力宏",
    "category": "正式會員"
  },
  {
    "name": "阿妹",
    "category": "來賓"
  },
  {
    "name": "伊朗",
    "category": "臨打"
  }
]
```

Recommended category values:
- `來賓`
- `正式會員`
- `臨打`

Compatibility note:
- older name-only arrays may exist
- future code should normalize both old and new formats safely

---

## 9. Member category features
Skills:
- store member category in file
- render category in member page
- use category for filtering
- use category for grouped dropdown display
- use category badge in UI
- use category counts / stats later if needed

Future category UI ideas:
- dropdown grouped by category
- badge colors:
  - 來賓
  - 正式會員
  - 臨打
- filter toggle by category
- category summary cards

---

## 10. Polling overwrite mitigation
This is a critical project skill.

Problem source:
- control polls `/api/state`
- next game polls `/api/next_game`
- new UI selections can be overwritten before backend state catches up

Mitigation skills:
- optimistic update
- pending state cache
- temporary polling suspension
- only clear pending state after backend data matches

---

## 11. Logo upload
Skills:
- `request.files`
- extension validation
- replace old `viewer.*`
- return logo meta data
- trigger viewer refresh

---

## 12. systemd deployment
Services:
- `scoreboard_control.service`
- `scoreboard_viewer.service`

Skills:
- back up before deploy
- restart / status checks
- keep deploy flow consistent
- avoid mixing experimental files into production baseline

---

## 13. Project maintenance principles
- use the user-confirmed version as baseline
- prefer targeted fixes over broad rewrites
- fix control issues in control only
- fix viewer issues in viewer only
- keep markdown guides updated when project rules change
