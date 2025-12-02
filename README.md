# NadeMenu (CounterStrikeSharp Plugin)

NadeMenu is a CounterStrikeSharp plugin for **CS2** that lets you:

- Browse nade lineups per map.
- Filter lineups by **type**, **side**, and **map part**.
- Save your own lineups directly in-game (position + view angle).
- Instantly **teleport** to any saved lineup to practice it.
- Optionally integrate with **MatchZy**-style `savednades.json`.

Author: `gabeazar`  
Plugin name: `NadeMenu`

---

## Features

- **Menu-based UI** using Center HTML menus.
- **Navigation via number keys** with helper `css_0`–`css_9` commands.
- **Save flow** that captures:
  - Team side (CT/T)
  - Map part (A/Mid/B)
  - From/To short names
  - Method/instructions text
  - **Precise position (POS) and aim angle (ANG)** using `EyeAngles`
- **Filter system**:
  - By nade type: Smoke / Flash / Molly / HE / ALL
  - By side: CT / T / Both
  - By map part: A / Mid / B / All
- **MatchZy compatibility**:
  - Can read lineups from `csgo/cfg/MatchZy/savednades.json`.
  - Also saves its own lineups into that file (or a local file) in a compatible structure.

---

## Requirements

- **CounterStrikeSharp** (latest stable)
- **CS2 Server**
---

## Installation

1. **Copy plugin**:
   - Place the `NadeMenu.dll` into:
     ```text
     csgo/addons/counterstrikesharp/plugins/NadeMenu/
     ```
   - Final path example:
     ```text
     csgo/addons/counterstrikesharp/plugins/NadeMenu/NadeMenu.dll
     ```

2. **Configuration file**:
   - On first run, CounterStrikeSharp will generate:
     ```text
     csgo/addons/counterstrikesharp/configs/plugins/NadeMenu/NadeMenu.json
     ```
   - Edit this file to change options (see **Configuration** below).

3. **Restart the server** (or reload CounterStrikeSharp) for changes to apply.

---

## Configuration

Default config (example):

```jsonc
{
  "Version": 1,
  "UseMatchZy": true,
  "RegisterNadeAlias": true,
  "PrimaryPageSize": 4
}
Options
UseMatchZy (bool, default: true)

true: Use MatchZy‑style path csgo/cfg/MatchZy/savednades.json
false: Use plugin‑local JSON: addons/counterstrikesharp/plugins/NadeMenu/savednades.json
RegisterNadeAlias (bool, default: true)

When true, both nades and nade commands open the menu.
When false, only nades is registered.
PrimaryPageSize (int, default: 4)

Number of primary menu items per page (e.g. 4 options + Prev/Next/Close).
Usage
Commands
nades
Open the NadeMenu main menu.

nade (if RegisterNadeAlias = true)
Same as nades.

Recommended keybinds
When you join the server, you’ll see a chat message with a bind one‑liner to copy into your console.
It looks like this:

bind 1 "slot1; css_1"; bind 2 "slot2; css_2"; ...; bind 0 "slot10; css_0"
This lets you control the menu with your number keys:

1–4 = first four menu options
8 = Prev page / Back
9 = Next page
0 = Close menu
You can also use chat shortcuts like:

!1, !2, !3, !4 – activate option 1–4 in the current menu
!8 – Prev
!9 – Next
!0 – Close
(Only while the menu is open.)

Menu Flow
Main Menu
Browse all nades
Shows every nade for the current map (no filters).

Filter nades
Step‑by‑step filter:

Nade Type (Smoke / Flash / Molly / HE / ALL)
Side (CT / T / Both)
Map Part (A / Mid / B / All) Then shows the filtered list.
Save new lineup
Starts the save flow (see below).

Saving a New Lineup
Open the menu:

nades
Choose “Save new lineup”.

Select:

Side: CT or T
Map Part: A / Mid / B
Nade Type:
If you hold a grenade, NadeMenu will auto‑detect type (Smoke/Flash/Molly/HE).
Otherwise, pick it manually.
Provide text inputs via chat:

From: Short name for your current position (e.g. tspawn, long, coffins)
To: Short name for landing spot (e.g. xbox, pit, coffins)
Instructions: How to throw it (e.g. jumpthrow, walk 2 steps then throw, etc.)
Confirm:

NadeMenu prints a summary in chat.
Confirm Yes (save) to save the lineup.
What is Saved
For each lineup, NadeMenu saves:

POS — your exact world position (AbsOrigin)
ANG — your exact view angles at save time (EyeAngles)
Map name
Nade type
Short names and instructions
An ID generated like:
<side>_<part>_<to>_from_<from>
# example:
ct_a_xbox_from_tspawn
Both POS and ANG are then parsed into a Vector + QAngle pair and reused exactly when teleporting.

Browsing & Previewing
Use “Browse all nades” or “Filter nades” to see lineups for the current map.
Each menu entry is the nade ID (e.g. ct_a_xbox_from_tspawn).
Selecting a lineup will:
Teleport you to the saved POS.
Set your view to the saved ANG (same aim direction as when saved).
Print the description/instructions in chat.
You can repeat this as many times as you want to practice the lineup.

JSON / MatchZy Compatibility
If UseMatchZy = true, NadeMenu reads and writes:
csgo/cfg/MatchZy/savednades.json
It supports:
A "default" group containing id → NadeEntry mappings.
Per‑map groups (MatchZy style) where the group key is the map name.
When loading, NadeMenu:
Flattens all groups.
Uses either entry.Map or the group key as the map name.
Only lineups matching the current map are shown.
When saving, NadeMenu always writes to the "default" group (safe and predictable).
Building from Source
Clone the repository or copy the .cs file(s) into a project targeting the same TargetFramework as your CounterStrikeSharp install (e.g. net7.0).
Restore and build:
dotnet restore
dotnet build -c Release
Copy the resulting NadeMenu.dll into:
csgo/addons/counterstrikesharp/plugins/NadeMenu/
Troubleshooting
Menu won’t close

Use 0 (or !0 in chat).
On map change and round start, all menus are force‑closed by the plugin.
Saved nades not appearing

Make sure you are on the same map you saved them on.
Check UseMatchZy setting and correct JSON path.
Verify the JSON at savednades.json is valid (no syntax errors).
Teleport is wrong angle

Make sure you are using the latest version: NadeMenu now saves EyeAngles, not entity rotation.
Re‑save the lineup; new saves will use your exact aim direction.
