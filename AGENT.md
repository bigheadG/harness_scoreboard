# AGENT.md

## Project
ScoreBoard / Homecare web stack on Linux with Flask + MQTT + systemd.

This file is the working guide for future edits to the **scoreboard** side of the project.

---

## Current scope

Focus on the **scoreboard badminton** project, not homecare.

Project root (current convention):

```bash
/home/user123/Desktop/scoreboard_badminton
```

Main web services:

- `scoreboard_control.service`
- `scoreboard_viewer.service`

Typical ports:

- control: `5084`
- viewer: `5083`

---

## Current blessed baseline

### Control
Use this file as the current control baseline:

```text
web_scoreboard_control_rwd_with_members_next_game_team_select_auto_apply_fixed_polling_v3.py
```

Reason:
- RWD control page
- member list
- next game entry
- team1 / team2 can be selected from member list
- next game can be applied to current team names
- includes polling race-condition mitigation for team-name overwrite

### Viewer
Use the latest viewer that is currently accepted by the user in the conversation as the live baseline in the project directory.

If a newer experimental viewer is created, do **not** replace the production baseline unless the user explicitly confirms it.

---

## Explicitly avoid

Do **not** continue from this experimental file unless the user explicitly asks:

```text
web_scoreboard_control_rwd_team_cards_colored.py
```

Reason:
- user said this version has many issues in next team entry
- treat it as abandoned / experimental

---

## Functional requirements already discussed

### Viewer
Viewer should support:

- strong RWD behavior
- fullscreen mode
- logo can remain visible in fullscreen
- marquee text can be large
- right team name fixed on the right side of right card
- left team name fixed on the left side of left card
- logo and marquee can be shown / hidden from control
- live logo refresh after upload if supported by current viewer baseline

### Control
Control should support:

- large touch-friendly RWD layout
- no double-tap zoom issue on iPhone Safari
- scoreboard actions:
  - `+1`
  - `-1`
  - sync
  - set
  - reset score
  - reset all
  - local swap
  - both swap
- team name / color editing
- member page
- next game entry
- team1 / team2 name can come from:
  - manual input
  - member list
  - next game left / right side
- avoid poll loop overwriting newly selected team names

---

## Persistent data files

Under:

```bash
static/branding/
```

Current data conventions:

- `viewer_marquee.txt`
  - marquee text
- `viewer_ui.json`
  - viewer UI switches
  - example:
    ```json
    {
      "show_logo": true,
      "show_marquee": true
    }
    ```
- `members.json`
  - member list
- `next_game.json`
  - next game players
- `viewer.png` / `viewer.svg` / `viewer.jpg` / `viewer.jpeg` / `viewer.webp`
  - viewer logo image

Rule:
- prefer storing simple editable project data as JSON or TXT in `static/branding/`

---

## Deployment pattern

### Control deploy
```bash
cp /home/user123/Desktop/scoreboard_badminton/web_scoreboard_control.py    /home/user123/Desktop/scoreboard_badminton/web_scoreboard_control.py.bak

cp <new_file>.py    /home/user123/Desktop/scoreboard_badminton/web_scoreboard_control.py

sudo systemctl restart scoreboard_control.service
sudo systemctl status scoreboard_control.service --no-pager
```

### Viewer deploy
```bash
cp /home/user123/Desktop/scoreboard_badminton/web_scoreboard_viewer.py    /home/user123/Desktop/scoreboard_badminton/web_scoreboard_viewer.py.bak

cp <new_file>.py    /home/user123/Desktop/scoreboard_badminton/web_scoreboard_viewer.py

sudo systemctl restart scoreboard_viewer.service
sudo systemctl status scoreboard_viewer.service --no-pager
```

---

## Important behavior rules for future edits

### 1. Do not break accepted behavior
If the user says a file/version is working, preserve these accepted behaviors and patch on top of that baseline.

### 2. Keep viewer and control changes separate
If a problem is only in control, do not unnecessarily modify viewer.

### 3. Prefer small targeted fixes
Avoid broad UI rewrites unless the user explicitly asks for a redesign.

### 4. Watch out for polling overwrite issues
If control uses polling from `/api/state`, any UI-side temporary edits can be overwritten.
For team-name selection flows:
- either persist immediately
- or temporarily suppress overwrite until backend state catches up

### 5. iPhone Safari matters
Always consider:
- double-tap zoom suppression
- safe-area padding
- touch-friendly buttons
- portrait and landscape layout

### 6. Uploaded logo replacement
When replacing viewer logo:
- store as `viewer.*` in `static/branding/`
- remove old `viewer.*` files first if deterministic behavior is needed

---

## Recommended quick test checklist

After each control update:

```bash
sudo systemctl restart scoreboard_control.service
sudo systemctl status scoreboard_control.service --no-pager
```

Check in browser:

- control page loads
- language switch works
- team names do not revert unexpectedly
- member dropdown shows data
- next game entry works
- marquee save works
- viewer UI toggle works
- logo upload works if present

After each viewer update:

```bash
sudo systemctl restart scoreboard_viewer.service
sudo systemctl status scoreboard_viewer.service --no-pager
```

Check in browser:

- viewer loads
- fullscreen works
- team name alignment is correct
- marquee size is correct
- logo visibility is correct
- live updates still work

---

## Suggested next-step philosophy

When adding new features:
1. patch from the current blessed baseline
2. keep file naming explicit
3. avoid mixing unrelated experiments into production baseline
4. preserve working next-game and team-name flows first

---

## Current instruction for future work

When asked to continue scoreboard control work, start from:

```text
web_scoreboard_control_rwd_with_members_next_game_team_select_auto_apply_fixed_polling_v3.py
```

and ignore:

```text
web_scoreboard_control_rwd_team_cards_colored.py
```

unless the user explicitly asks otherwise.
