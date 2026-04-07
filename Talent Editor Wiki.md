# Talent Editor

The Talent Editor is a module in [IMET (Icarus Mod Editor Tool)](README.md).  
It lets you create, edit, and manage talent trees visually on a node graph canvas — no hand-editing JSON required.

---

## What are Talents?

ICARUS uses several interconnected data tables for its talent system:

| Table | Purpose |
|---|---|
| `D_Talents` | Individual talent nodes — stats, requirements, display name, icon, position, etc. |
| `D_TalentTrees` | Groups talents into named trees (e.g. "Blueprint_T1_Player", "Combat_Solo") |
| `D_TalentArchetypes` | Categories that contain trees (e.g. "Survival", "Combat", "Blueprint") |
| `D_TalentViews` | Which archetype pages are shown on each interface view |
| `D_TalentModelViews` | Visual themes — node textures, accent colours, layout rules |

Inside a mod file, the editor looks for sections where `CurrentFile` matches `D_Talents.json`, `D_TalentTrees.json`, or `D_TalentArchetypes.json` and surfaces every entry for editing.

### Core Talent Fields

| Field | Description |
|---|---|
| `Name` | Unique row name identifying this talent |
| `DisplayName` | NSLOCTEXT-formatted display label shown in-game |
| `Description` | NSLOCTEXT-formatted description text |
| `TalentType` | Node type: `Talent`, `Reroute`, or `MutuallyExclusive` |
| `DrawMethod` | How connection lines are routed: `YThenX`, `XThenY`, `ShortestDistance`, or `Unspecified` |
| `Position` | `{X, Y}` coordinates on the talent page canvas |
| `NodeSize` | `{X, Y}` width and height of the node |
| `Icon` | Game asset path to the talent's icon texture |
| `RequiredTalents` | Array of talent row names this talent depends on (drawn as connection lines) |
| `RequiredLevel` | Minimum character level to unlock |
| `bDefaultUnlocked` | Whether the talent starts unlocked (always true for reroute nodes) |
| `RequiredRank` | Rank gate from `D_TalentRanks` |
| `ExtraData` | Links the talent to a row in another data table (e.g. `D_Itemable`, `D_ProspectList`) |
| `RequiredFlags` | Account / character / DLC flags the player must have |
| `ForbiddenFlags` | Session flags that block unlocking |
| `Ranks` | Array of reward tiers — each tier can grant stats and/or character flags |
| `Metadata.RequiredFeatureLevel` | Development feature-level gate |

---

## Editor Layout

The Talent Editor has two view modes, toggled by the **Editor** / **Page Editor** buttons at the top left.

### Editor Mode

```
┌─────────────────────────────────────────────────────────────────────────┐
│  [Editor] [Page Editor]     Category Pills        [⚙ Settings] [↓ Icons]│
│  ── Tree Tab Bar (scrollable) ──────────────────────────────────────────│
├──────────────┬───────────────────────────────────┬──────────────────────┤
│              │                                   │                      │
│  Talent List │         Graph Canvas              │    Properties Panel  │
│  (left)      │         (center)                  │    (right)           │
│              │                                   │                      │
│              │                        ┌────────┐ │                      │
│              │                        │Controls│ │                      │
└──────────────┴────────────────────────┴────────┘─┴──────────────────────┘
```

### Page Editor Mode

```
┌─────────────────────────────────────────────────────────────────────────┐
│  [Editor] [Page Editor]                                                 │
├──────────────────────────────────┬──────────────────────────────────────┤
│                                  │                                      │
│  Archetypes Section              │  Trees Section                       │
│  (list + editor)                 │  (list + editor)                     │
│                                  │                                      │
└──────────────────────────────────┴──────────────────────────────────────┘
```

---

## Opening a File

When no file is open, the module shows an empty-state screen.

1. Use the toolbar **Open** button to load a `.json` or `.exmod` mod file.
2. The editor parses the file and extracts talent, tree, and archetype sections automatically.

### Supported file formats

Both `.json` and `.exmod` files use the same ICARUS JSON structure. The editor handles both transparently.

---

## Category Pills & Tree Tab Bar

### Category pills

A row of coloured pills at the top filters which tree tabs are shown:

| Pill | Behaviour |
|---|---|
| **All** | Shows all visible trees |
| **Blueprint**, **Player**, **Creature**, etc. | Shows only trees in that category |
| **[Dev]** | Trees with missing archetype entries (shown in gold) |

Each pill shows a count badge with the number of trees in that category. The pill's accent colour is derived from the first tree in the category.

### Tree tabs

Below the pills, a horizontal scrollable row of tabs — one per visible talent tree.

| Element | Description |
|---|---|
| **Tab label** | Tree display name (disambiguated if duplicates exist) |
| **Mod dot** | Small coloured dot if the tree is modified or added by the mod |
| **DEV badge** | Gold label if the tree has a missing archetype |
| **Scroll** | Use the mouse scroll wheel on the tab bar to scroll horizontally |

Click a tab to switch the graph canvas and talent list to that tree.

### Settings button (⚙)

Opens the **Tab Visibility Settings** modal to configure which trees appear in the tab bar.

---

## Tab Visibility Settings

A modal dialog for choosing which talent trees are visible.

### Layout

| Section | Description |
|---|---|
| **Search** | Filter trees by name |
| **Left column** | Category buttons with count badges — click to filter the right column |
| **Right column** | Checkbox list of trees, each showing the tree label, mod dot, DEV badge, and raw row name |
| **Select All / Clear All** | Bulk toggle for the current filtered view |
| **Counter** | Shows `X / Y` selected out of total |

### Footer buttons

| Button | Effect |
|---|---|
| **Default** | Resets visibility to the default set (Blueprint tiers + any mod-added trees) |
| **Cancel** | Closes without applying changes |
| **Apply** | Saves the new visibility set and closes. If the active tab was hidden, switches to the first visible tab. |

The visibility selection is persisted to `localStorage` and restored on next load.

---

## Talent Icons

### Icon sources (in priority order)

1. **User custom icon** — assigned per-talent via right-click → "Set Custom Icon"
2. **GitHub cached icons** — fetched from the talent icons repository
3. **Bundled fallback** — shipped with the app
4. **Initials** — auto-generated from the talent's display name (1–3 uppercase letters)

### Toolbar icon controls

| Button | Description |
|---|---|
| **↓ Icons** | Fetches the latest talent icons from GitHub. Progress shows `Fetching… X / Y`. Pulsing orange border when an update is available. |
| **↻ Reload** | Forces a reload of icons from the local cache |

After a fetch completes, a status message appears briefly: `✓ X updated`, `✓ Up to date`, or `⚠ Fetch failed`.

### Custom icon assignment

1. Right-click a node on the graph canvas.
2. Select **Set Custom Icon**.
3. Choose a `.png` file from the file picker.
4. The icon is copied to the `user_talent_icons/` directory and saved in the user config.

Custom icons persist across sessions and are independent of the mod file.

---

## Talent List (Left Panel)

### Header controls

| Control | Description |
|---|---|
| **A–Z toggle** | Sorts talents alphabetically when on. When off, talents appear in base-game order with added talents listed separately below. Persisted to `localStorage`. |
| **Count badge** | Shows the total number of talents in the active tree |
| **RAW toggle** | When on, shows the raw row name below each talent's display label. Persisted to `localStorage`. |

### List layout

When **A–Z is off** (default), the list is split into two sections:

1. **Base-game & overridden talents** — in their original order. Modified talents show an orange dot.
2. **Divider**: "custom talents"
3. **Added talents** — new talents created in this mod. Each has a drag handle (⠿) for reordering. Gold dot indicates a new talent.

When **A–Z is on**, all talents are shown in a single flat alphabetical list with no drag handles.

### List item elements

| Element | Description |
|---|---|
| **Display label** | Resolved from DisplayName → ExtraData lookup → row name fallback |
| **Raw name** | Monospace row name (only shown when RAW toggle is on) |
| **Mod status dot** | Orange = modified base-game talent, Gold = newly added talent |
| **Drag handle (⠿)** | Visible on added talents when A–Z is off — drag to reorder |

### Selection

| Action | Result |
|---|---|
| **Click** | Select that talent (deselects others). The graph canvas centres on it and the properties panel shows its fields. |
| **Shift+Click** | Toggle that talent in/out of the multi-selection set |
| **Click empty area** | Deselects all talents |

### Drag-to-reorder (added talents only)

1. Grab the **⠿** handle on an added talent.
2. Drag vertically — a dashed placeholder shows the drop position.
3. A floating ghost of the item follows your cursor with a tilt effect.
4. Release to drop. The reorder is reflected in the save output.
5. Auto-scrolling activates when dragging near the top or bottom edge of the list.

> Drag-to-reorder is only available when A–Z sort is off, since reordering a sorted list would be contradictory.

---

## Graph Canvas (Centre Panel)

The graph canvas is the core of the Talent Editor. It displays talent nodes as a visual tree with connection lines showing dependencies.

### Visual elements

| Element | Description |
|---|---|
| **Dot grid** | Repeating dot pattern (25 px spacing) that moves with pan/zoom |
| **Origin guide lines** | Dashed lines through (0, 0) — horizontal and vertical |
| **Blueprint boundary** | Horizontal line at Y = 1350 for Blueprint-model trees |
| **Nodes** | Rectangular talent cards or diamond reroute points |
| **Connection lines** | Lines between nodes showing RequiredTalents dependencies |
| **Selection box** | Dashed rectangle when rubber-band selecting |
| **Wire preview** | Dashed green line when shift-dragging to create a connection |

### Node appearance

#### Regular nodes (rectangular)

Each node renders with:

- **Background texture** — model-specific artwork (Player, Solo, Creature, Blueprint, Prospect, Outpost, Workshop, Great Hunt)
- **Tint overlay** — orange if modified by the mod, or the tree's accent colour for base-game nodes
- **Icon** — from the icon system (see [Talent Icons](#talent-icons)), or auto-generated initials
- **Label** — below the icon, auto-sized text with a darker background bar. Orange for modded, accent for base-game.
- **Recipe count badge** — small diamond overlay if the talent is referenced by 2+ crafting recipes, showing the count

#### Reroute nodes (diamond)

- Small 20×20 px diamond shape
- No label, no icon
- Used as routing waypoints for connection lines
- Always have `bDefaultUnlocked = true` (enforced automatically)

### Node visual states

| State | Appearance |
|---|---|
| **Default** | Normal texture, standard opacity |
| **Selected** | Hovered texture, higher opacity |
| **Hovered** | Hovered texture version |
| **Modded** | Orange tint overlay |
| **Wiring source** | Crosshair cursor, green preview line extending from node |
| **Wire target** | Bright highlight when hovered during wiring |

### Connection lines

Lines are drawn between nodes based on the `RequiredTalents` array.

| Property | Description |
|---|---|
| **Routing** | Controlled by the source node's `DrawMethod` — `YThenX` (vertical then horizontal), `XThenY` (horizontal then vertical), `ShortestDistance` (direct diagonal), or `Unspecified` (defaults to XThenY) |
| **Colour** | Orange if either connected node is modded, otherwise the tree's accent colour |
| **Hover** | Full opacity + thicker stroke (2.5 px) on hover |
| **Hit area** | 12 px invisible zone for easy clicking |

#### Right-click a connection line

Right-clicking on a connection line removes that dependency (cuts the wire). The source node's `RequiredTalents` array is updated immediately.

---

## Canvas Controls

### Panning

| Input | Behaviour |
|---|---|
| **Middle mouse + drag** | Pan the canvas. Cursor disappears (pointer lock) — you can pan infinitely without hitting screen edges. |
| **Ctrl + left-click + drag** | Same as middle mouse panning |
| **Release** | Cursor reappears at its locked position |
| **Escape** | Exits panning if pointer lock is active |

### Zooming

| Input | Behaviour |
|---|---|
| **Scroll wheel** | Zoom toward/away from the cursor position. 5% steps, range 5%–500%. |
| **− button** (controls bar) | Zoom out one step |
| **+ button** (controls bar) | Zoom in one step |
| **Zoom %** (controls bar) | Shows current zoom level |

### Snap grid

The snap grid dropdown (controls bar, bottom-right) controls position snapping when dragging nodes:

| Value | Effect |
|---|---|
| **5 px** | Fine grid |
| **10 px** | Small grid |
| **25 px** | Medium grid |
| **50 px** | Default grid |
| **100 px** | Large grid |

When snapping is active, node positions snap according to the **Round Snap** mode (see below).

### Round Snap

The **Round Snap** button (in the controls bar, next to the snap grid dropdown) controls *how* snapping is applied when dragging nodes.

| State | Behaviour |
|---|---|
| **On (green)** | Dragged nodes snap to the nearest absolute grid multiple — e.g. a node dragged to 63 px snaps to 50 px or 100 px. This is the default. |
| **Off** | Dragged nodes snap in increments of the grid size *from their original position* — e.g. a node at 13 px moves to 63 px, 113 px, etc., preserving its offset. |

Use **Round Snap off** when your nodes are intentionally offset from the grid and you want to keep that offset while still moving in clean steps. The setting is persisted across sessions.

### Fit all nodes

Click the **grid icon** button (⊞) in the controls bar to auto-zoom and pan so all nodes in the current tree fit within the canvas viewport.

### Controls bar

Located at the bottom-right corner of the canvas. Glass-styled floating bar containing:

```
[ Snap Grid ▾ ] [ Round Snap ] │ [ − ] 100% [ + ] │ [ ⊞ ]
```

---

## Node Interactions

### Selecting nodes

| Action | Result |
|---|---|
| **Click a node** | Select it (deselects others). Opens its properties in the right panel. |
| **Shift+Click a node** | Toggle it in/out of the multi-selection set |
| **Rubber-band selection** | Click and drag on empty canvas to draw a selection box. All nodes overlapping the box are selected. |
| **Click empty canvas** | Deselects all nodes |

### Dragging nodes

- **Requirement**: the node must be modded (exist in `pendingOverrides`). Base-game nodes that haven't been modded yet cannot be moved.
- **Single drag**: click and drag a modded node to reposition it.
- **Multi drag**: if multiple nodes are selected and the dragged node is among them, all selected modded nodes move together.
- **Snap**: positions snap to the active grid (see [Snap Grid](#snap-grid)).

### Wiring (Shift+Drag)

Create dependency connections between nodes:

1. **Shift + hold left-click** on a modded node to start wiring.
2. A dashed green preview line extends from the source node to your cursor.
3. Drag over another node — it highlights as a valid target.
4. Release on the target to add it to the source's `RequiredTalents` array.
5. Duplicate connections are automatically prevented.

> Shift+drag is only available on modded nodes (nodes in `pendingOverrides`).

### Node context menu (right-click)

Right-click a node to open the context menu:

| Option | Behaviour |
|---|---|
| **Duplicate** | Creates a copy of the node with all fields, offset by 50 px. RequiredTalents are cleared on the duplicate. |
| **Copy ▸** | Submenu with field-group copy options (see below) |
| **Paste** | Applies the previously copied fields to this node (greyed out if clipboard is empty) |
| **Set Custom Icon** | Opens a file picker to assign a custom `.png` icon to this node |
| **Delete** | Opens a confirmation dialog, then removes the node |

#### Copy submenu

| Group | Fields copied |
|---|---|
| **All Fields** | Every editable field |
| **Position** | X, Y coordinates |
| **Size** | Width, Height |
| **Display Name** | DisplayName (NSLOCTEXT) |
| **Description** | Description (NSLOCTEXT) |
| **Icon** | Icon asset path |
| **Type & Draw Method** | TalentType, DrawMethod |
| **Extra Data** | ExtraData row reference |
| **Conditions** | RequiredLevel, bDefaultUnlocked, RequiredRank, RequiredFlags, ForbiddenFlags, Metadata |
| **Dependencies** | RequiredTalents array |
| **Rewards** | Ranks array (stats and flags) |

The clipboard persists across node selections within the session.

### Canvas context menu (right-click empty canvas)

| Option | Behaviour |
|---|---|
| **Add Node** | Creates a new regular talent node at the click position |
| **Add Route** | Creates a new reroute diamond at the click position |

New nodes are added to `pendingOverrides` immediately and appear on the canvas.

### Tooltip

Hovering over a node shows a small tooltip with the node's row name. The tooltip is clamped inside the canvas bounds to prevent overflow.

---

## Properties Panel (Right Panel)

When a talent is selected, the right panel shows all of its editable fields.

### Header

| Element | Description |
|---|---|
| **Title** | "Properties" |
| **Talent name** | Monospace label showing the selected talent's row name |
| **+ Mod** | Appears for base-game talents not yet modded — click to add the talent to the mod file for editing |
| **⟳ Defaults** | Opens the defaults viewer overlay (base-game talents only) — compare current values to base-game originals |
| **✕ Revert** | Removes all overrides, reverting the talent to its base-game state |
| **✕ Delete** | Removes a custom/added talent from the mod |

> Fields are disabled (read-only) until the talent is modded (added to `pendingOverrides`).

### Identity

| Field | Type | Notes |
|---|---|---|
| **Name** | Text input | Editable row name. Spaces auto-convert to underscores. Press Enter or click away to commit the rename. |
| **Type** | Dropdown | `Talent`, `Reroute`, or `MutuallyExclusive`. Changing to Reroute forces `bDefaultUnlocked = true`. |
| **Talent Tree** | Picker | Click to open a tree picker overlay. Shows all available trees. |

### Display

| Field | Type | Notes |
|---|---|---|
| **Display Name** | Text input | The in-game display label. Stored as NSLOCTEXT format internally. |
| **Description** | Textarea (3 rows) | In-game description text. Also NSLOCTEXT format. |
| **Icon** | Text + Browse | Game asset path. Browse button opens an asset browser overlay. |

### Layout

| Field | Type | Notes |
|---|---|---|
| **Position** | Dual input (X, Y) | Node centre coordinates on the canvas. Also updated by dragging. |
| **Size** | Dual input (W, H) | Node width and height. Reroute nodes use 0×0; regular nodes default to 184×184. |
| **Draw Method** | Dropdown | `Unspecified`, `YThenX`, `XThenY`, `ShortestDistance`. Controls how connection lines route from this node. |

### Extra Data

Links the talent to a row in another data table. When no Display Name or icon is set on the talent, the game uses the referenced item's name and icon instead.

| Field | Type | Notes |
|---|---|---|
| **Row Name** | Picker | Click to open the ExtraData picker overlay — browse rows from D_Itemable, D_ProspectList, D_WorkshopItems, etc. |
| **Data Table** | Read-only | Shows the data table the selected row belongs to |

### Conditions

| Field | Type | Notes |
|---|---|---|
| **Req. Level** | Number input | Minimum character level. Default 0. |
| **Default On** | Checkbox | `bDefaultUnlocked` — whether the talent starts unlocked. Always true for reroute nodes. |
| **Req. Rank** | Picker | Opens a picker of D_TalentRanks entries |

### Advanced

| Field | Type | Notes |
|---|---|---|
| **Feature Level** | Picker | Development feature-level gate. Nested in `Metadata.RequiredFeatureLevel`. |

### Dependencies

| Field | Type | Notes |
|---|---|---|
| **Required Talents** | Tag list + Edit | Shows a chip for each dependency talent. Click × to remove one. Click **Edit** to open a multi-select talent picker. |
| **Required Flags** | Tag list + Edit | Account, character, and DLC flags. Each chip shows the row name + data table badge. Click **Edit** to open a categorised multi-select picker (D_DLCPackageData, D_AccountFlags, D_CharacterFlags). |
| **Forbidden Flags** | Tag list + Edit | Session flags that block unlocking. Only D_SessionFlags entries. |

### Rewards

Rewards are organised into **ranks** (tiers). A talent can have one or more ranks.

#### Single rank (1 tier)

| Section | Controls |
|---|---|
| **Granted Stats** | Table of stat → value pairs. Click **Edit** to open a stat picker (multi-select from D_Stats). Each stat has a number input for the value and a × button to remove. |
| **Granted Flags** | Character flags granted on unlock. Click **Edit** to open a character flags picker. Shows chips with × to remove. |

#### Multiple ranks (2+ tiers)

Each rank appears in a labelled container ("Rank 1", "Rank 2", etc.) with the same Stats and Flags controls. A × button on each rank header removes that tier.

A **Ranks** control lets you set the number of tiers (1–5 choices via picker).

### Field override indicators

For base-game talents, fields that have been overridden show a "modified" indicator. Individual field revert buttons (×) allow reverting single fields without affecting others.

---

## Defaults Viewer

Click **⟳ Defaults** in the properties header to open the defaults viewer overlay.

### For base-game talents

Shows all field groups side by side — base-game values vs. current overrides.

| Element | Description |
|---|---|
| **Field group** | Each property section (Identity, Display, Layout, etc.) |
| **"modified" tag** | Appears if any field in the group has been overridden |
| **Restore button** | Per-group — deletes the override, reverting to the base-game value |
| **Restore All** | Reverts every override on the talent, returning it fully to base-game state |

### For custom talents

Shows all current values (no base-game comparison available, since the talent doesn't exist in the base game).

---

## Changes Dialog

Click the **Changes** button in the toolbar (visible when the file has unsaved changes) to open the changes dialog.

### Layout

Lists all modified, added, and deleted talents with their field-level diffs.

| Element | Description |
|---|---|
| **Entry header** | Shows `[MODIFIED]`, `[ADDED]`, or `[DELETED]` label + talent row name + Revert button |
| **Field diffs** | For each changed field: old value (red, − prefix) and new value (green, + prefix) |
| **Per-field Revert** | Reverts a single field to its saved state |
| **Per-entry Revert** | Reverts all changes for that talent |
| **Close** | Closes the dialog |

If nothing has changed, the dialog shows an empty state.

---

## Delete Confirmation

When deleting node(s) — via the context menu, Properties panel, or keyboard — a confirmation dialog appears.

| Element | Description |
|---|---|
| **Title** | "Delete Node?" or "Delete N Nodes?" |
| **Message** | Explains that base-game nodes will reappear when the mod is unloaded, while mod-only nodes are permanently removed |
| **Cancel** | Closes without deleting |
| **Delete** (red) | Confirms deletion |

---

## Page Editor Mode

Switch to Page Editor mode using the **Page Editor** button at the top left (next to **Editor**).

This mode manages the structural containers for talents: **Archetypes** (categories) and **Trees** (groups within archetypes).

### Archetypes Section (left half)

#### List

- **Header**: "Archetypes" + **+ New** button
- **Mod entries** listed first (draggable by ⠿ handle to reorder)
- **Base-game entries** listed below (not draggable)
- Each item shows:
  - Display label (from DisplayName or row name)
  - Mod status dot: gold = new, orange = edited
- Click to select and view in the editor panel

#### Editor

When an archetype is selected:

| Field | Type | Notes |
|---|---|---|
| **Row Name** | Text input | Editable only for new archetypes |
| **Display Name** | Text input | Stored as NSLOCTEXT |
| **Icon** | Text + Browse | Game asset path for the archetype's icon |
| **Model View** | Picker | Selects from D_TalentModelViews — controls the visual theme for all trees under this archetype |
| **Required Level** | Number input | Optional minimum level to see this archetype page |
| **Metadata** | Picker | Feature level gate |

#### Buttons (context-dependent)

| Button | When shown | Effect |
|---|---|---|
| **Mod This Page** | Base-game archetype, file open | Copies the base entry into the mod draft for editing |
| **Remove Override** | Modded base-game archetype | Reverts to the base-game entry |
| **Delete** | New archetype | Permanently removes it |

#### Adding a new archetype

Click **+ New** to create a new archetype. The name is auto-generated (`New_Archetype_1`, `New_Archetype_2`, etc.) and can be renamed in the editor panel.

### Trees Section (right half)

Identical layout to the Archetypes section, with these fields:

| Field | Type | Notes |
|---|---|---|
| **Row Name** | Text input | Editable only for new trees |
| **Display Name** | Text input | Stored as NSLOCTEXT |
| **Icon** | Text + Browse | Game asset path |
| **Archetype** | Picker | Selects which archetype this tree belongs to |
| **Metadata** | Picker | Feature level gate |

The same **+ New**, **Mod This Page**, **Remove Override**, and **Delete** buttons apply.

### Drag-to-reorder

In both sections, mod-created entries can be reordered by dragging the ⠿ handle. Base-game entries always stay in their original order below the mod entries.

---

## Saving

Changes are not written to disk until you explicitly save.

| Action | Effect |
|---|---|
| **Ctrl+S** | Saves the file (keyboard shortcut handled by the app) |
| **Toolbar Save button** | Same as Ctrl+S |

### What happens on save

1. The mod file is re-read from disk to catch any external changes.
2. Talent rows are rebuilt:
   - Modified base-game talents come first
   - Added talents follow in the order set by the talent list (drag-to-reorder)
   - Fields matching base-game defaults are omitted to keep the mod file minimal
3. Archetype and tree rows are rebuilt with the same omission logic.
4. Sections are spliced back into the file's `Rows` array.
5. JSON is written to disk with 4-space indentation.
6. The dirty indicator clears.

### Dirty tracking

The file is marked dirty when any of these change relative to the last save:

- Talent field overrides
- Added/deleted talents
- Talent list order (drag-to-reorder)
- Archetype or tree drafts (Page Editor)

An unsaved-changes marker appears on the module tab.

---

## Reroute Nodes

Reroute nodes are routing waypoints for connection lines. They appear as small diamonds on the canvas.

### Properties

- **Type**: Always `Reroute`
- **bDefaultUnlocked**: Always `true` (enforced automatically on save)
- **Size**: 0×0 (no visible rectangular area)
- **No label, no icon**: Just a diamond shape

### Creating a reroute

- Right-click empty canvas → **Add Route**
- Or change an existing node's Type to `Reroute` in the properties panel

### Usage

Wire talents through reroute nodes to create cleaner line layouts. Connection lines pass through the reroute's position, creating bend points in the visual tree.

---

## Keyboard Shortcuts

| Key | Context | Action |
|---|---|---|
| **Delete** / **Backspace** | Canvas focused, node(s) selected | Opens delete confirmation for selected modded nodes |
| **Escape** | During panning | Exits pointer lock and stops panning |
| **Enter** | Name field focused | Commits the rename |
| **Space** (in Name field) | Name field focused | Converted to underscore automatically |
| **Scroll wheel** | Over tab bar | Scrolls tabs horizontally |
| **Scroll wheel** | Over canvas | Zooms in/out |

---

## Picker Overlays

Various fields open picker overlays for selecting values from game data tables.

### Row Picker

A simple searchable list. Used for: Feature Levels, Talent Ranks, Talent Trees, stat/rank count selection.

- Search box filters the list
- Click to select, **Clear** to unselect
- Highlighted row shows the current selection

### Talent Picker (multi-select)

Used for editing RequiredTalents. Shows all talents grouped by tree.

- Checkboxes for each talent
- Search filters across all trees
- **Commit** button applies the selection

### Category Picker (multi-select)

Used for RequiredFlags and ForbiddenFlags. Shows entries grouped by data table category:

- D_DLCPackageData
- D_AccountFlags
- D_CharacterFlags
- D_SessionFlags

Each item has a checkbox. Search filters across all categories.

### Stat Picker (multi-select)

Used for Granted Stats in the Rewards section. Shows all entries from D_Stats with checkboxes.

### ExtraData Picker

Two-panel layout:

- **Left**: Data table selector (D_Itemable, D_ProspectList, D_WorkshopItems, etc.)
- **Right**: Rows from the selected table, searchable
- Click a row to link the talent to that data entry

### Asset Browser

File/folder tree browser for selecting game asset paths (icons, textures).

- Search box
- Click a file to select its game path
- Used for Icon field browsing

---

## Copy & Paste

The node context menu supports field-level copy/paste between talents.

### Workflow

1. Right-click a source node → **Copy** → choose a field group
2. Right-click a target node → **Paste**
3. The copied fields are applied to the target node

The clipboard stores the last copied field group with a label (e.g. "Position", "All Fields") and persists within the session.

### Paste availability

The **Paste** option appears in the context menu only when the clipboard has content. It shows the label of the last copy operation (e.g. "Paste — Position").

---

## Tips

- **Use "+ Mod" first** — base-game talents are read-only until you click **+ Mod** in the properties panel to add them to your mod file. This copies the talent's current values and enables editing.
- **Shift+Drag to wire** — hold Shift and drag from one node to another to quickly add dependency connections, rather than editing the RequiredTalents list manually.
- **Right-click connections to cut** — instead of editing RequiredTalents, just right-click a connection line on the canvas to remove that dependency.
- **Use reroute nodes** — add reroute diamonds to create clean bend points in your connection lines, especially when lines cross or overlap.
- **Snap grid for alignment** — use 50 px or 100 px grid snap to keep nodes aligned. Switch to 5 px or 10 px for fine adjustments. Toggle **Round Snap** off if your nodes sit at offsets from the grid and you want to preserve that offset while still moving in even steps.
- **Fit all nodes** — click the grid (⊞) button in the controls bar to instantly zoom and pan to see all nodes in the current tree.
- **Copy fields across nodes** — use the Copy submenu (right-click) to copy a field group from one node, then Paste onto another. Great for duplicating rewards or conditions.
- **Page Editor for structure** — use the Page Editor mode to add new archetypes and trees before populating them with talent nodes in Editor mode.
- **Reorder added talents via drag** — the order of added talents in the list is the order they appear in the saved mod file. Drag them to control output order.
- **Infinite panning** — middle-mouse panning locks the cursor, so you can pan the canvas as far as you want without running into screen edges.
- **Keyboard delete** — select nodes on the canvas and press Delete or Backspace to quickly remove them (with confirmation).
- **Download icons once** — click ↓ Icons to fetch community-contributed talent icons from GitHub. They're cached locally and update only when changes are detected.
