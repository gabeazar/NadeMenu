# NadeMenu — Advanced Lineup & Nade Practice Menu for Counter-Strike 2

**NadeMenu** is a lightweight, high-performance server-side plugin for Counter-Strike 2 that gives players an intuitive in-game menu to browse, filter, search, and teleport to saved grenade lineups — with full integration for **MatchZy** users.

It was built from the ground up using **CounterStrikeSharp v1.0.347+** and works perfectly alongside MatchZy, Get5, or in regular practice servers.

![NadeMenu in action](https://i.imgur.com/example-screenshot.png)  


---

### Features

- Beautiful chat-based menu with pagination, colour-coded nade types and live search
- Full **MatchZy compatibility** – automatically loads `savednades.json` from `csgo/cfg/MatchZy/` when enabled
- Save new lineups in seconds with guided flow (auto-detects held grenade, team side, map part)
- Powerful filtering: by nade type (Smoke/Flash/Molly/HE), side (T/CT), bombsite (A/Mid/B), and free-text search
- Instant teleport to lineup position + view angle
- Zero lag – intelligent caching, tick-rate friendly, cleans up player state on disconnect/round start
- Easy to extend – all nades stored in a single human-readable JSON file

---

### How It Complements MatchZy

MatchZy already includes a basic nade-saving system and stores lineups in `cfg/MatchZy/savednades.json`.  
**NadeMenu enhances that experience dramatically** by turning a raw JSON file into a fully featured, searchable, filterable menu that players can use during practice without ever leaving the game.

You keep using MatchZy exactly as before — NadeMenu simply reads the same file (or its own if you disable MatchZy mode) and gives your community a professional-grade lineup browser.

---

### Installation

1. Make sure **CounterStrikeSharp** is installed and up to date (`css_plugins list` should show version 347 or higher).
2. Download the latest release (`NadeMenu.dll`) from the [Releases](https://github.com/gabeazar/NadeMenu/releases) page.
3. Place the DLL into:  
   `csgo/addons/counterstrikesharp/plugins/NadeMenu/`
4. (Optional) Create the folder structure if it doesn’t exist.
5. Restart the server or run `css_plugins load NadeMenu`

**That’s it!** The plugin auto-detects whether you’re running MatchZy and switches JSON path accordingly.

---

### Configuration

A `config.json` file is created on first load inside the plugin folder:

```json
{
  "Version": 1,
  "UseMatchZy": true,
  "PageSize": 6
}
