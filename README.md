# NadeMenu (CounterStrikeSharp Plugin)

NadeMenu is a CounterStrikeSharp plugin for **CS2** that lets you:

- Browse nade lineups per map and filter them.
- Save your own lineups directly in-game (position + aim).
- Instantly **teleport** to any saved lineup to practice it.
- Browse and manage your own lineups (“My lineups”).
- Admin-manage all lineups on the server (edit/move/delete).
- Optionally integrate with **MatchZy**-style `savednades.json`.
- Optionally auto-equip / auto-give the correct grenade when teleporting to a lineup.

Author: `gabeazar`  
Plugin name: `NadeMenu`  
Current plugin version: `1.2.1`

---

## Features

- **Menu-based UI** using Center HTML menus.
- **Number-key navigation** via helper `css_0`–`css_9` commands.
- **Save flow** that captures side, part, From/To, instructions, POS & ANG (`EyeAngles`).
- **Filter & browse** by nade type, side, and map part, including “My lineups” views.
- **Manage lineups**: rename/edit text, update position, delete (owner or admin).
- **Auto-equip/give grenade on teleport** (configurable per lineup type & team).
- **MatchZy-compatible JSON** with creator ownership & timestamps.

---

## Requirements

- **CounterStrikeSharp** (latest stable).
- **CS2 dedicated server** (latest update).

---

## Installation

1. **Place plugin**

   Place `NadeMenu.dll` into:

       csgo/addons/counterstrikesharp/plugins/NadeMenu/

   Example final path:

       csgo/addons/counterstrikesharp/plugins/NadeMenu/NadeMenu.dll

2. **Configuration file**

   On first run, CounterStrikeSharp will generate:

       csgo/addons/counterstrikesharp/configs/plugins/NadeMenu/NadeMenu.json

   Edit this file to change options (see **Configuration**).

3. **Restart or reload**

   Restart the server or reload CounterStrikeSharp for changes to apply.

---

## Configuration

**Default config (example):**

```json
{
  "Version": 2,
  "UseMatchZy": true,
  "RegisterNadeAlias": true,
  "PrimaryPageSize": 4,
  "AutoEquipSavedGrenade": true,
  "GiveMissingGrenade": true
}
```

**Options:**

- `Version` (int, default: 2)  
  - Internal config version used by the plugin; you normally should not change this.

- `UseMatchZy` (bool, default: true)  
  - `true` → Use MatchZy-style path:  
    `csgo/cfg/MatchZy/savednades.json`  
  - `false` → Use plugin-local JSON:  
    `addons/counterstrikesharp/plugins/NadeMenu/savednades.json`

- `RegisterNadeAlias` (bool, default: true)  
  - When `true`, both `nades` and `nade` (or `!nades` / `!nade` in chat) open the menu.  
  - When `false`, only `nades` is registered.

- `PrimaryPageSize` (int, default: 4)  
  - Number of primary menu items per page (e.g. 4 options + Prev/Next/Close).

- `AutoEquipSavedGrenade` (bool, default: true)  
  - When teleporting to a lineup, automatically switches to the grenade type saved with that lineup (Smoke/Flash/Molly/HE) if you have it in your inventory.

- `GiveMissingGrenade` (bool, default: true)  
  - Only used when `AutoEquipSavedGrenade` is `true`.  
  - `true` → If you don’t have the saved grenade type, NadeMenu gives you that grenade and then auto-equips it.  
  - `false` → If you don’t have the grenade, NadeMenu does not give or equip anything (teleport only).

---

## Usage

### Commands

- `nades` – Open the NadeMenu main menu.  
- `nade` – Same as `nades` (if `RegisterNadeAlias = true`).  
- `nmadmin` – Open the admin manager directly (requires `@css/root`).

> CounterStrikeSharp also exposes these as chat commands, so `!nades`, `!nade`, and `!nmadmin` work as expected.

### Recommended Keybinds

When you join the server, NadeMenu prints a one-liner you can paste into your console, for example:

```cfg
bind 1 "slot1; css_1"; bind 2 "slot2; css_2"; bind 3 "slot3; css_3"; bind 4 "slot4; css_4"; bind 5 "slot5; css_5"; bind 6 "slot6; css_6"; bind 7 "slot7; css_7"; bind 8 "slot8; css_8"; bind 9 "slot9; css_9"; bind 0 "slot10; css_0"
```

This lets you control the menu via your number keys:

- **1–4** – First four menu options.
- **8** – Prev page / Back.
- **9** – Next page (or Close if there is no next page).
- **0** – Close menu.

You can also use chat shortcuts while the menu is open:

- `!1`, `!2`, `!3`, `!4` – Activate options 1–4 in the current menu.
- `!8` – Prev.
- `!9` – Next (or Close if no next page).
- `!0` – Close.

When NadeMenu is asking for text input (From/To/Instructions), anything you type in chat is **captured** and not shown to other players. You can optionally prefix it with `!` or `/` (for example, `!xbox` or `/jumpthrow`); NadeMenu strips those prefixes.

---

## Menu Flow

### Main Menu

Opening `nades` / `!nades` shows the main menu:

- **Browse all nades**  
  Shows every saved lineup for the current map (no extra filters).

- **Filter / My lineups**  
  Opens a sub-menu for filtered browsing and “My lineups” (see below).

- **Save new lineup**  
  Starts the save flow (see **Saving a New Lineup**).

- **Manage lineups (edit/remove)**  
  Opens the management UI (My lineups, or All lineups for admins).

---

### Filter / My Lineups

From **Filter / My lineups** you get:

1. **Browse all nades**  
   Quick path: show all lineups on the current map (no filters).

2. **Filter by type/side/part**  
   Step-by-step filter:

   - Nade Type: Smoke / Flash / Molly / HE / ALL  
   - Side: CT / T / Both  
   - Map Part: A / Mid / B / All  

   After choosing these, you see a filtered browse list.

3. **Browse my lineups**  
   Shows only lineups **you created** on the current map (teleport-only browse).

Filtering only changes what is displayed; it does not modify or delete any lineups.

---

## Saving a New Lineup

1. Open the menu: `!nades` (or `nades` in console).
2. Choose **Save new lineup**.
3. Select:
   - **Side**: CT or T.
   - **Map Part**: A / Mid / B.
   - **Nade Type**:
     - If you are holding a grenade, NadeMenu tries to auto-detect the type (Smoke/Flash/Molly/HE).
     - Otherwise, you pick it manually.

4. Provide **text inputs via chat** when prompted:
   - **From** – Short name for your current position (e.g. `tspawn`, `long`, `coffins`).
   - **To** – Short name for landing spot (e.g. `xbox`, `pit`, `coffins`).
   - **Instructions** – How to throw it (e.g. `jumpthrow`, `walk 2 steps then throw`).

   While NadeMenu is expecting these, your chat messages are captured and not printed. You can start them with `!` or `/` if you prefer.

5. **Confirm**  
   NadeMenu prints a summary in chat and shows a confirmation menu. Choose **Yes (save)** to finalise the lineup.

### What Is Saved

Each lineup stores:

- **POS** – Your exact world position (`AbsOrigin`) at save time.
- **ANG** – Your exact view angles (`EyeAngles`) at save time.
- **Map** – Map name (`de_dust2`, etc.).
- **Type** – Nade type: Smoke / Flash / Molly / HE.
- **Short names** – Side (CT/T), map part (A/Mid/B), From, and To.
- **Instructions** – Free-text “how to throw” description.
- **Ownership & timestamps**:
  - `CreatorSteamId`
  - `CreatorName`
  - `CreatedAtUtc`
  - `LastEditedAtUtc`

The lineup ID is built as:

```text
<side>_<part>_<to>_from_<from>
```

For example:

```text
ct_a_xbox_from_tspawn
```

POS and ANG are parsed into `Vector` + `QAngle` and reused exactly when teleporting, so your aim is identical every time.

---

## Browsing & Teleporting

You can browse lineups through:

- **Browse all nades** (no filters).
- **Filter by type/side/part** (filtered list).
- **Browse my lineups** (only your own).

Each list entry shows the lineup ID, for example:

```text
ct_a_xbox_from_tspawn
```

Selecting a lineup will:

- Teleport you to the saved **position**.
- Set your view to the saved **angles**.
- Optionally **auto-equip the correct grenade** (and give it if missing), depending on your config:
  - If `AutoEquipSavedGrenade = true` and you already have that grenade, NadeMenu sends a `use weapon_x` command so it’s in your hand immediately.
  - If `AutoEquipSavedGrenade = true` and `GiveMissingGrenade = true`, NadeMenu gives you the grenade first, then equips it.
- Print the **instructions/description** in chat.

After teleporting, NadeMenu reopens the browse list so you can quickly choose another lineup.

---

## Managing & Editing Lineups

From the main menu, select **Manage lineups (edit/remove)**:

- **My lineups (edit/remove)**  
  Shows only lineups **you own** on the current map.

- **[Admin] All lineups**  
  Shows **all** lineups on the current map. Only available if you have `@css/root`.

In the manage list, selecting a lineup opens **Manage • Lineup actions**:

- **Teleport to lineup**  
  Teleport to this lineup’s POS/ANG (same behavior as browsing).

- **Rename / edit text**  
  Change side, part, From, To, type (if needed), and instructions.

- **Update position from where I stand**  
  Keep ID and text, but overwrite POS and ANG with your current position and aim.

- **Delete lineup**  
  Permanently delete the lineup after a confirmation step.

When you open a lineup in manage mode, NadeMenu prints detailed info in chat:

- ID and map.
- Type, side, part, From, To.
- Owner (creator name or SteamID).
- Created time and last edited time.
- Notes/description.

### Rename / Edit Flow

When you choose **Rename / edit text**:

- NadeMenu pre-fills side, part, type, From, To, and instructions from the existing lineup.
- You walk through:
  - Side (CT/T).
  - Map part (A/Mid/B).
  - Nade type (if needed).
  - New From, To, and Instructions via chat inputs.
- A confirmation view shows:
  - Old ID → New ID.
  - New side/part/From/To/instructions.

On confirm, NadeMenu:

- Renames the lineup (changing the dictionary key if the ID changes).
- Updates type and description.
- Sets `LastEditedAtUtc` to now.

### Permissions

- **Edit / update position / delete** is allowed if:
  - You are the **creator** (SteamID match), or
  - You have **admin** permissions (`@css/root`).
- **Teleporting** is allowed for all players, even if they are not the owner.
- The admin manager (`[Admin] All lineups` and `nmadmin`) always requires `@css/root`.

---

## JSON / MatchZy Compatibility

If `UseMatchZy = true`, NadeMenu reads and writes:

```text
csgo/cfg/MatchZy/savednades.json
```

It supports:

- A `"default"` group containing `id → NadeEntry` mappings.
- Optional per-map groups (MatchZy style), where the group key is the map name.

When loading, NadeMenu:

- Deserialises all groups in the JSON.
- Uses `entry.Map` if present, otherwise falls back to the group key as the map name.
- Only exposes lineups for the **current map**.

When saving or editing, NadeMenu:

- Ensures there is at least a `"default"` group.
- Writes new and updated lineups into the `"default"` group.
- Adds or updates ownership and timestamp metadata.

**Example single-entry structure (formatted for readability):**

```json
{
  "default": {
    "ct_a_xbox_from_tspawn": {
      "LineupPos": "123.00000 456.00000 789.00000",
      "LineupAng": "10.00000 20.00000 0.00000",
      "Desc": "Regular standing jumpthrow.",
      "Map": "de_dust2",
      "Type": "Smoke",
      "CreatorSteamId": "7656119xxxxxxxxxx",
      "CreatorName": "PlayerName",
      "CreatedAtUtc": "2025-12-02T12:34:56Z",
      "LastEditedAtUtc": "2025-12-02T12:34:56Z"
    }
  }
}
```

If `UseMatchZy = false`, the same structure is used in a plugin-local file:

```text
addons/counterstrikesharp/plugins/NadeMenu/savednades.json
```

---

## Troubleshooting

- **Menu won’t close**  
  Use key **0**, `!0` in chat, or press **ESC**. Menus also auto-close on death, disconnect, round start, map change, and when moving to spectator.

- **Saved nades not appearing**  
  Ensure you are on the same map you saved them on. Check the `UseMatchZy` setting and that the JSON path is correct. Validate that `savednades.json` is valid JSON.

- **Teleport is wrong angle**  
  Make sure you’re running a version where NadeMenu saves `EyeAngles` (1.1.0+). Re-save the lineup; new saves will use your exact aim direction.

- **Cannot edit/delete a lineup**  
  Only the **creator** of a lineup or an admin with `@css/root` may edit or delete it. Teleporting remains available to everyone.

---

Enjoy practicing and organising your CS2 nade lineups with NadeMenu!
