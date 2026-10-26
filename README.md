Carbonite 3.3.5a — Grimfall Fork

This is a fork of DevScarabyte/Carbonite-3.3.5a-Remastered, adapted for the Grimfall private server (WotLK 3.3.5a, classless, with backported Cataclysm-era zones: Uldum, Tol Barad, Tol Barad Peninsula, plus custom Goblin/Kezan and Worgen/Gilneas starting continents).

The original Carbonite relies on hardcoded assumptions about stock WotLK continent/zone data. Grimfall's custom zones caused a cascade of Lua errors and UI bugs as a result — this fork fixes those so Carbonite runs stably on Grimfall.

What this fork fixes
Crashes on custom zones — added nil-guards to coordinate decoding (Nx.Que:ULR) so unknown/missing byte-packed data no longer throws an arithmetic error.
"table index is nil" error when registering unknown map names — the code now skips (instead of crashing) when a Grimfall-specific zone name isn't in Carbonite's internal lookup table.
"attempt to compare nil with number" error in the continent-boundary table (Map.MaI2) — added a metatable that returns a safe placeholder for unknown continents (e.g. Kezan, Gilneas) instead of crashing.
"Unknown map name" chat spam — silenced.
Version nag messages ("a newer version is available", "this version is pretty old") — silenced.
Native world map (Alt+M) became completely unclickable with Carbonite loaded — root cause was a background timer (Nx.Que:SBQDT) firing every 0.9 seconds to scan quest data across zones, which kept changing the global map state (SetMapZoom) regardless of whether the native Blizzard map was open. This collided with the native map's own update cycle and randomly broke POI button clicks/tooltips. Fix: this background scan now pauses while the native map is open, and resumes where it left off once you close it.
WorldMapTooltip getting stuck on the wrong parent frame — after Carbonite's own map-embedding (AWM/DWM), the native tooltip now always gets reset to the correct WorldMapFrame parent and TOOLTIP strata whenever the native map opens.
Caverns of Time (Tanaris) map — all three layers (entrance, portal room, inside) showed the real Tanaris terrain or the Kalimdor overview instead of the drawn Caverns of Time map, and quest distances jumped between layers. Fix: the portal room and the inside layer each got their own map entry (1026 / 1027) with geometry taken from WorldMapArea.dbc, an explicit zone-index binding (Grimfall's classless hero-class start zone shares the "Caverns of Time" name), the continent-overview and terrain-tile layers are suppressed there, and both entries are flagged as city maps so the composite map tiles render.
Tol Barad was missing from Carbonite entirely — Tol Barad Peninsula and Tol Barad are now real Carbonite zones (map scale from the client's WorldMapArea.dbc), so the zone map, your position and quest markers show there. On Carbonite's world map they sit in open sea west of the Eastern Kingdoms. 
Gilneas (Worgen start zone) — Gilneas City and Gilneas are now Carbonite zones. The client puts them on no continent, so Carbonite recognizes them by name; their map shows while you are there (like an instance). On the world view, Tol Barad, Tol Barad Peninsula and Gilneas are always drawn in the sea west of the Eastern Kingdoms, so they are visible without hovering.
Cataclysm portals — Horde: Orgrimmar to Tol Barad Peninsula, Deepholm and Blasted Lands, and the portals back to Orgrimmar from Tol Barad Peninsula, Deepholm and Hellfire Peninsula, with in-game measured positions. They appear in the Guide's Travel list and are used for routing. Portals without their own icon now show a portal icon instead of a question mark. Alliance side: Stormwind to Tol Barad Peninsula, Deepholm and Blasted Lands, and back to Stormwind from Tol Barad Peninsula, Deepholm and Hellfire Peninsula.
Caverns of Time map hover — the CoT layers are flagged as city maps (needed for the drawn map), which also made them win the map's mouse-over zone selection: inside the cave the map jumped between layers depending on mouse position, and on the Tanaris map hovering the cave switched to the CoT city view and darkened the continent. Hovering now never selects the CoT layers, and inside the cave the map always shows the layer you are in.
Caverns of Time quest NPCs — Caverns of Time has two Andormu NPCs (adult and child), but Carbonite's quest database had the adult's position for every Andormu quest. All Andormu quests (The Caverns of Time, To The Master's Lair, Old Hillsbrad, The Black Morass, Return to Andormu, Hero of the Brood) now point to the child Andormu. Erozion and Sa'at stand inside instances that are closed on Grimfall and had no position at all; they now point to the Escape from Durnholde and Black Morass portals.
Feedback: please use GitHub issues.
Quest map point error — a Lua error ("attempt to perform arithmetic on local 'x'", Nx.Que:SBQDZ) when the game returned no map point for a quest during Carbonite's background quest scan (seen e.g. on joining a party in Tol Barad). Such quests are now skipped.
Data Issues window — cases where Carbonite skips missing game data are recorded (once each, kept between sessions). Right click the Carbonite minimap button > Show Data Issues to see them (copyable text, Clear button). A red ! on the minimap button, the menu entry and the button's tooltip show when there are new entries.
Installation

Option A — download the whole repo:

Click the green Code → Download ZIP button on this page.
Unzip it, and copy the Carbonite folder into Interface/AddOns/.
Fully log out/in (not just /reload) so the addon reloads cleanly.

Option B — update just the addon files:

Download Carbonite.lua, Carbonite.xml, and Localization.lua.
Copy them into Interface/AddOns/Carbonite/ (overwriting the existing files).
Fully log out/in (not just /reload) so the addon reloads cleanly.
Credits

Based on: DevScarabyte/Carbonite-3.3.5a-Remastered Original Carbonite: Carbon Based Creations, LLC (2007-2010)

This fork is unofficial and not affiliated with Blizzard or the original Carbonite developers.
