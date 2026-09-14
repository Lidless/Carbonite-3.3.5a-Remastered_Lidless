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
