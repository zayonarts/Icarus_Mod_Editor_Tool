# Recipe Editor

The Recipe Editor is a module in [IMET (Icarus Mod Editor Tool)](README.md).  
It lets you create, edit, and manage `D_ProcessorRecipes` entries inside `.json` or `.exmod` mod files — no hand-editing JSON required.

---

## What is D_ProcessorRecipes?

`D_ProcessorRecipes.json` is the data table ICARUS uses to define every crafting recipe — what items are needed as inputs, what is produced as outputs, which bench/processor is required, any talent gate, energy cost, and more.

Inside a mod file, the editor looks for entries where `CurrentFile === "Crafting-D_ProcessorRecipes.json"` and surfaces every item in the nested `File_Items[]` array as an individual recipe.

### Core Recipe Fields

| Field | Description |
|---|---|
| `Name` | The unique string key identifying this recipe |
| `RequiredMillijoules` | Energy cost (millijoules) the processor must supply |
| `Requirement` | Talent / blueprint that must be researched to unlock this recipe |
| `RecipeSets` | Which crafting bench(es) (recipe set rows) can execute this recipe |
| `Inputs` | Array of `{ RowName, Count }` items the crafting consumes |
| `Outputs` | Array of `{ RowName, Count }` items the crafting produces |
| `Audio` | Crafting audio cue row |
| `ResourceInputs` | Resource-type costs (energy, water, fuel, etc.) consumed during crafting |
| `ResourceOutputs` | Resource-type outputs produced by crafting |
| `Metadata.RequiredFeatureLevel` | Feature-level gate (e.g. early-access gating) |
| `CharacterRequirement` | Character-flag required on the player |
| `SessionRequirement` | Session/world-flag required |
| `ExperienceMultiplier` | XP multiplier relative to the recipe set's base XP |
| `Refundable` | Override the refundability of this recipe (default inherits from recipe set) |
| `bForceDisableRecipe` | Hard-disables the recipe even if all requirements are met |
| `bContainsContainer` | Marks the recipe as producing a container item |
| `bSelectOutputItemRandomly` | One of the output items is chosen at random instead of all being given |

---

## Editor Layout

```
┌──────────────────────────────────────────────────────────────────┐
│  File Tab Bar  (one tab per open file, Ctrl+Click for multi)     │
├──────────────────────┬───────────────────────────────────────────┤
│                      │                                           │
│   Recipes List       │           Recipe Editor                   │
│   (left column)      │           (right column)                  │
│                      │                                           │
│   ← draggable →      │                                           │
└──────────────────────┴───────────────────────────────────────────┘
```

The divider between the two columns is **drag-resizable** — see [Resizable Divider](#resizable-divider).

---

## Opening a File

When no file is open, the module shows an empty-state screen.

1. Use the **toolbar Open button** (or however your app surfaces file opening) to load a `.json` or `.exmod` mod file.
2. Alternatively, click the **+** button at the right end of the tab bar to open additional files on top of an already-open one.

### Supported file formats

Both `.json` and `.exmod` files use the same ICARUS JSON structure. The editor handles both transparently.

---

## File Tab Bar

Every open file gets its own **tab** at the top of the module.

### Tab visual states

| State | Appearance | Meaning |
|---|---|---|
| **Focused** | Solid accent border | This file is the "active" target for editing, saving, and Apply All |
| **Selected** | Faint accent background | This file's recipes appear in the combined recipe list |
| **Dirty** | Small amber dot on label | The file has unsaved changes |

A tab can be **selected but not focused** — its recipes appear in the combined list, but the editor and save operations target only the focused file.

### Click behaviour

| Action | Result |
|---|---|
| Left-click | Focus that tab and collapse the selected set to just that file (single-file mode) |
| Ctrl+Click | Focus that tab and toggle it in/out of the selected set without collapsing others (multi-file combined view) |

You cannot deselect the last remaining selected tab.

### Per-tab hover buttons

When you hover a tab, two small buttons appear inline:

- **⬇ Save** (amber) — saves that file immediately; only visible when the file is dirty.
- **✕ Close** — closes the tab; prompts for confirmation if the file is dirty.

### Opening additional files

Click the **+** button at the right end of the tab bar. A native file picker opens (supports multi-select, `.json` and `.exmod`). Each selected path opens as a new tab. If a file is already open, its existing tab is focused instead.

### Closing the last tab

When the last tab is closed, the module resets to its empty state.

---

## Resizable Divider

An invisible 8px drag handle sits between the recipe list column and the editor column.

- **Drag left/right** — resizes both columns live. Width is clamped between **160 px** and **520 px**.
- On release, the chosen width is saved to `localStorage` under `recipes-editor-list-width` and restored on next load.
- **Right-click the handle** — shows a small context menu with a "↺ Reset width" option that snaps the list back to the default **240 px** and removes the stored value.

---

## Recipes List (Left Column)

### Search

A text input at the top of the list filters recipes by name in real time. The search is case-insensitive and supports multi-token matching (e.g. typing `sm w` will match `SM_Wood_Chair`). Clearing the search shows all recipes.

> The drag-to-reorder handle is hidden while a search is active to prevent ambiguous reordering of filtered results.

### List cards

Each recipe is shown as a card with:

| Element | Description |
|---|---|
| **Left border** | Accent color = selected; amber = has a pending (unapplied) edit; transparent = unmodified |
| **Recipe name** | Truncated with ellipsis if too long |
| **File tag** | Small monospace badge (multi-file mode only) showing which file the recipe lives in |
| **⠇ handle** | Drag handle for reordering (single-file mode only, hidden during search) |
| **Orange dot** | Appears on the right edge when there is a saved-but-unapplied edit for this recipe |

### Drag-to-reorder

Only available in **single-file mode** (one tab selected) when the search field is empty.

1. Grab the **⠇** handle on the left of a recipe card.
2. Drag it vertically to a new position — a dashed drop-target placeholder shows where it will land.
3. Auto-scrolling activates when your cursor is within 72 px of the top or bottom edge.
4. Release to drop. The file is marked dirty.

### Context menu

Right-click any recipe card to open a context menu:

| Option | Behaviour |
|---|---|
| 🗂 **Duplicate** | Creates a copy of the recipe immediately below the original. The copy is named `<original>_D1` (or `_D2`, `_D3`, etc., first unused suffix). You are taken to the new recipe automatically. |
| 🗑 **Delete** | Removes the recipe from the file. The selection adjusts to stay in bounds. |

### Count badge

The **Recipes** header shows a count badge (e.g. `42`) when there is at least one recipe in the list.

### "+ New" button

Located in the top-right of the **Recipes** header.

- **Single file open** — immediately appends a blank recipe named `New_Recipe` and selects it.
- **Multiple files open** — opens a small dialog asking which file the new recipe should be added to.

If the file had no `Crafting-D_ProcessorRecipes.json` section yet, one is created automatically just before `EndOfMod`.

---

## Recipe Editor (Right Column)

Clicking any recipe in the list opens it in the right column for editing. The editor has its own **unsaved-changes state** separate from the file's save state — you must explicitly **Apply** your edits to write them into the file (see [Pending Edits & Applying](#pending-edits--applying)).

### In Files row *(multi-file mode only)*

When two or more files are open, a row of small file-name buttons appears at the very top of the form.

| Button style | Meaning |
|---|---|
| **Accent color + filled background** | This file currently contains the recipe |
| **Dim + transparent background** | This file does NOT contain the recipe |

- **Click a "has it" button** — removes the recipe from that file. Blocked if it would leave the recipe in zero files.
- **Click a "doesn't have it" button** — copies the full recipe (including any current pending edits) into that file. The recipe section is created if it doesn't exist yet.

This is how you **copy a recipe across files** or **move** it (copy then delete from the original).

---

### Name

A plain text field. This is the recipe's unique key inside the data table. It must be unique within the file.

---

### Required Millijoules

An integer field. Entering a value shows two **live time estimates** alongside it:

- **Base time** — `millijoules ÷ MaxMilliwattage` (wattage is read from the recipe's recipe set's `D_Processing` row, falling back to `D_Processing` Defaults, then 1000 mW).
- **+25% speed** — the same calculation multiplied by `0.75` (representing a speed bonus).

Times are formatted as `Xs` (under 60 s) or `Xm Ys` (60 s and over).

---

### Required Blueprint

A picker field. Clicking opens the **Talent Picker** dialog.

- Two-column layout: talent tree filter on the left, matching talents on the right.
- Sources: all open mod files' talent data, plus the base-game `D_Talents.json` (mod entries take precedence).
- Search filters by talent name across all trees.
- Selecting a talent writes its name to `Requirement.RowName`.
- Selecting **(clear)** removes the requirement.

---

### Recipe Set

A picker field. Clicking opens a searchable **list picker** of all rows in `D_RecipeSets.json`.

- Determines which crafting bench / processor can use this recipe.
- Writes to `RecipeSets[0].RowName` (the first entry in the array).

---

### Audio

A picker field. Clicking opens a searchable **list picker** of all rows in `D_CraftingAudioData.json`.

- Determines the sound played when crafting this recipe.
- Selecting **(clear)** removes the `Audio` object entirely.

---

### Inputs

The items a player must provide to craft this recipe.

**Adding inputs:** Click **+ Add** in the section header to append a blank input row.

Each input row contains:

| Control | Purpose |
|---|---|
| **⠇ handle** | Drag to reorder inputs within the array |
| **Item picker** | Opens the **Item Picker** (categories: All, Base Game, one per open mod file). Writes to `Element.RowName`. |
| **Count** | Integer field — how many of the item are required |
| **+** | Insert a blank row immediately below this one |
| **−** | Remove this row |

#### Input clipboard (right-click menu)

Right-click any input row to access copy/paste operations. The clipboard persists across recipe navigation and is shared across all open files within the session.

| Option | Behaviour |
|---|---|
| 📋 **Copy** | Copies this row to the single-row clipboard |
| ⬆ **Paste Above** | Inserts the single-row clipboard entry above this row |
| ✎ **Paste (overwrite)** | Replaces this row with the clipboard entry |
| ⬇ **Paste Below** | Inserts the clipboard entry below this row |
| 📋 **Copy All** | Copies the entire inputs array to the multi-row clipboard |
| ⬇ **Paste All (append)** | Appends all clipboard rows to the end of the inputs |
| ⟳ **Replace All** | Replaces the entire inputs array with the clipboard rows (shown in red — destructive) |

> **Tip:** You can also use the [Base Recipes window](#base-recipes-reference-window) to copy inputs from any base-game recipe directly into your clipboard.

---

### Outputs

The items the recipe produces. Layout is identical to Inputs.

**Adding outputs:** Click **+ Add** in the section header. Each new output defaults to `Count: 1` (unlike inputs which default to `Count: 0`).

The item picker for outputs uses `D_ItemTemplate` as the base-game source (rather than `D_ItemsStatic` used by inputs).

> Outputs do not have a clipboard context menu.

---

### Resource Inputs

Additional resource-type costs consumed during crafting (e.g. water, fuel, electricity, oxygen).

**Adding resource inputs:** Click **+ Add**.

Each row contains:

| Control | Purpose |
|---|---|
| **Resource type picker** | Opens a list picker of rows from `D_IcarusResources.json` |
| **Required Units** | Number field — how many units of the resource are consumed |
| **−** | Remove this row |

---

### Resource Outputs

Resource-type quantities the recipe generates. Layout identical to Resource Inputs.

---

### Advanced

A collapsible section. Click the **ADVANCED ▸** header to expand or collapse it.

| Field | Type | Notes |
|---|---|---|
| **Feature Level** | Picker | Gate the recipe behind a development feature level (`D_FeatureLevels.json`) |
| **Char. Requirement** | Picker | Character-flag required on the player (`D_CharacterFlags.json`) |
| **Session Requirement** | Picker | Session/world-flag required for the recipe to appear |
| **XP Multiplier** | Float | Multiplier applied to the recipe set's base XP reward; leave empty to inherit |
| **Refundable** | Dropdown | `Inherit (default)` or `Block` |
| **bForceDisableRecipe** | Dropdown | `Inherit`, `true`, or `false` |
| **bContainsContainer** | Dropdown | `Inherit`, `true`, or `false` |
| **bSelectOutputItemRandomly** | Dropdown | `Inherit`, `true`, or `false` — randomly selects one output instead of all |

---

## Pending Edits & Applying

The Recipe Editor uses a **two-step edit flow**: you edit fields freely, then explicitly commit those changes to the file.

### Why two steps?

This lets you browse between multiple recipes, making edits to each, and then review and commit everything in one go — rather than having every keystroke immediately modify the file.

### How it works

1. **Edit fields** — changes are held in local editor state. The left border of the recipe card in the list turns **amber** and the footer shows ⚠ **Unsaved changes**.

2. **Navigate away without applying** — your edits are automatically snapshotted as a **pending edit** for that recipe. The recipe card gains an **orange dot** in the list. The edits are retained even as you work on other recipes.

3. **Return to that recipe** — the pending edits are automatically reloaded into the editor, exactly as you left them.

4. **Apply or Revert:**

| Button | Location | Effect |
|---|---|---|
| **Apply** | Editor footer | Commits the current recipe's edits into the file data. File is marked dirty (needs saving to disk). |
| **Revert** | Editor footer | Discards all current edits (and any pending snapshot) back to the last saved state. |

5. **Apply All (N)** — when you have pending edits on multiple recipes, this button (in the editor footer) opens the **Apply All modal**. `N` = the number of recipes with pending edits in the currently focused file.

### Apply All Modal

| Element | Description |
|---|---|
| Per-recipe cards | Shows each recipe with a diff of every changed field (old value in red strikethrough, new value in green) |
| **Exclude / Restore** button | Fades a recipe out and excludes it from the batch apply; click Restore to bring it back |
| Footer count | "N of M recipes will be applied" |
| **Apply (N)** button | Commits only the non-excluded recipes. Excluded entries keep their pending records. |
| **Cancel** | Closes without applying anything |

---

## Saving

Edits applied to the in-memory file data are **not written to disk** until you explicitly save.

| Action | Effect |
|---|---|
| **Ctrl+S** | Saves the currently focused file |
| **⬇ button on tab** (hover the tab) | Saves that specific file regardless of which tab is focused |

After saving, the dirty indicator is cleared. Empty recipe sections are automatically removed from the file before writing.

Files are written as 4-space-indented JSON.

---

## Base Recipes Reference Window

Click the **Base Recipes ⧉** button in the **Editor** header (top-right of the right column) to open a **separate reference window** showing all base-game recipes from `D_ProcessorRecipes.json`.

This window can remain open alongside the main editor. It supports:

- **Searching** the base-game recipe list.
- **Viewing** full recipe details (inputs, outputs, advanced fields).
- **Mod this recipe ⊙** — adds a copy of a base-game recipe to your focused mod file (if a recipe with that name doesn't already exist in the file).
- **Copy Inputs ⧉** — sends the base recipe's full inputs array to your clipboard (populates the multi-row clipboard used by **Paste All** in the editor).

The reference window stays in sync: when you change the accent color or IMM path in the main app, the reference window updates automatically.

---

## Multi-file Combined View

When two or more tabs are **selected** (Ctrl+Click), the recipe list shows a combined view of all selected files' recipes.

| Behaviour | Details |
|---|---|
| **All recipes shown** | Recipes from all selected tabs, in tab order |
| **File tag badge** | Each recipe card shows a small label indicating which file it belongs to |
| **Duplicate name** | If the same recipe name exists in more than one selected file, the file tag is always shown |
| **Clicking a recipe** | Automatically focuses the tab that contains it, so Apply / Save always target the right file |
| **Drag-to-reorder** | Disabled in combined mode — reordering is only available in single-file mode |
| **Apply All** | Only applies pending edits for the currently **focused** file, not across all files |
| **Pending dots** | A union of pending edits from **all** open files — every orange dot is visible regardless of which tabs are selected |

### Typical multi-file workflow

1. Ctrl+Click a second (or third) tab to add it to the combined view.
2. Browse the combined recipe list to compare or copy recipes across files.
3. Use the **In Files** row at the top of the editor to copy a recipe into another file or remove it from one.
4. Apply and save each file independently using its tab's ⬇ button.

---

## Tips

- **Edit multiple recipes, apply at the end** — make edits across as many recipes as you want, let them accumulate as pending edits (orange dots), then use **Apply All** to review and commit everything in one pass.
- **Copy inputs from base game** — open the **Base Recipes** reference window, find a similar recipe, and hit **Copy Inputs** to load those inputs into your clipboard. Then use **Paste All (append)** or **Replace All** in your recipe.
- **Clone a recipe template** — use **Duplicate** (right-click context menu) to clone a recipe as a starting point, then rename and adjust rather than building from scratch.
- **Multi-file merging** — open both a "source" and a "destination" mod file, Ctrl+Click both tabs to combine them, then use the **In Files** buttons to copy individual recipes from the source into the destination.
- **Resize the list panel** — if recipe names are long, drag the divider to the right to give the list more space. Width is remembered between sessions.
- **Ctrl+S saves the focused file** — when in multi-file mode, always check which tab has the solid accent border before hitting Ctrl+S. Use the per-tab ⬇ button to save a non-focused file.
