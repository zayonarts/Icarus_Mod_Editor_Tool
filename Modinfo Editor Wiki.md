# Modinfo Editor

The Modinfo Editor is the first module in [IMET (Icarus Mod Editor Tool)](README.md).  
It lets you create and manage `modinfo.json` files visually — no hand-editing JSON required.

---

## What is modinfo.json?

`modinfo.json` is the metadata file ICARUS reads to identify and display your mod in-game.  
A single file can contain **multiple mod entries**, each described by the fields below.

### JSON Structure

```json
{
  "mods": [
    {
      "name": "My Mod",
      "author": "YourName",
      "version": "1.0.0",
      "compatibility": "220",
      "description": "A short description of what the mod does.",
      "imageURL": "https://github.com/owner/repo/raw/main/image.png",
      "readmeURL": "https://github.com/owner/repo/raw/main/README.md",
      "files": {
        "exmodz": "https://github.com/owner/repo/raw/refs/heads/main/MyMod.exmodz",
        "pak":    "https://github.com/owner/repo/raw/refs/heads/main/MyMod_p.pak"
      }
    }
  ]
}
```

### Fields

| Field | Required | Description |
|---|---|---|
| `name` | Yes | Display name of the mod |
| `author` | No | Author name or tag |
| `version` | No | Version string, e.g. `1.0.0` or `1.0.0.220` |
| `compatibility` | No | ICARUS week number this mod is built for (e.g. `220`) |
| `description` | No | Free-text description shown in the mod browser |
| `imageURL` | No | Direct URL to a preview image (`.png`, `.jpg`, `.gif`, etc.) |
| `readmeURL` | No | Direct URL to a readme/documentation file (`.md`, `.txt`, etc.) |
| `files.exmodz` | No* | Direct URL to the `.exmodz` release file |
| `files.pak` | No* | Direct URL to the `_p.pak` release file |

> *At least one of `files.exmodz` or `files.pak` must be set for the mod to be downloadable.  
> The editor will warn you if neither is filled in.

---

## Editor Layout

The Modinfo Editor has three panels side by side:

```
┌─────────────────┬──────────────────────────────┬───────────────┐
│   Mod List      │       Right / Preview         │  Side Tools   │
│  (left panel)   │        (right panel)          │  (side panel) │
└─────────────────┴──────────────────────────────┴───────────────┘
```

---

## Left Panel — Mod List

Shows all mod entries in the file.

- **Search** — Filter by mod name or author (live filter, case-insensitive)
- **Click** — Select a mod to view it in the right panel
- **Double-click** — Open the mod directly in the edit form
- **Drag to reorder** — Grab the `⠿` handle and drag a mod to a new position.  
  A dashed drop-target placeholder shows where it will land.  
  The list auto-scrolls when you drag near the top or bottom edge.
- **Right-click → context menu**:
  - 🗂 **Duplicate** — creates a copy of that mod entry
  - 🗑 **Delete** — removes the entry (with confirmation)
- **Add a New Mod** button — appends a blank mod entry at the bottom of the list

> The drag handle disappears while the search field has text, since reordering filtered results would be ambiguous.

---

## Right Panel — View & Edit

### Viewing a mod

When a mod is selected, the right panel shows its data. Use the **view toggle buttons** at the bottom to switch between:

| Mode | Description |
|---|---|
| **Compact** | Formatted section-based view of the selected mod |
| **Code** | Syntax-highlighted JSON of the selected mod only |
| **Full Compact** | Formatted view of every mod in the file |
| **Full Code** | Syntax-highlighted JSON of the entire file |

URL values are shown in green. Non-URL string values are shown in orange. The colour scheme matches the code view.

### Editing a mod

Enter edit mode by **double-clicking** a mod in the list, or clicking the mod and pressing **Done** is unavailable until you click **☰ Edit** in Single mode. The panel switches to a form with four sections:

#### General

| Field | Notes |
|---|---|
| **Name** | The mod's display name |
| **Author** | Author credit |
| **Version** | Version string. See *Version Suffix* below. |
| **Compatibility** | ICARUS week number |

**Version suffix toggle** — If a week number is set in the side panel, a small toggle button appears next to the Version field showing `.{week}` (e.g. `.220`). Click it to append or remove the suffix from the stored version. This lets you track both the base version and the target week in one field.

#### Description

A multiline free-text field. Resizable vertically.

#### Links

| Field | Notes |
|---|---|
| **Image URL** | URL to a preview image |
| **Readme URL** | URL to a readme/documentation file |

Both fields have a **browse button (⋯)** that opens the [GitHub File Browser](#github-file-browser) once a repository has been fetched.

URL fields are colour-coded:
- **Green** — valid GitHub raw URL
- **Amber** — external (non-GitHub) URL — a warning card also appears at the bottom of the form
- **Default** — empty

#### Files

| Field | Notes |
|---|---|
| **EXMODZ** | Raw URL to the `.exmodz` release file |
| **PAK** | Raw URL to a `_p.pak` release file |

Same browse button and colour-coding as Links fields. A red warning appears if neither field has a value.

**External URL warning** — If any URL field points to a host outside of GitHub, an amber warning card appears at the bottom of the edit form listing the affected fields. GitHub raw URLs are strongly recommended for compatibility.

### Panel header

While a mod is selected, the panel header shows:
- **Badge** — mod index (`#1`, `#2`, …) or `N mods` in Full views
- **Mod name**
- **Revert** button — appears when there are unsaved edits; reverts all changes back to the last save point
- **Done** button — exits single-edit mode without saving to disk

---

## Side Panel — Tools

The right-side panel contains three tool cards that operate across the whole file.

### Icarus Week

Enter the current ICARUS week number (e.g. `220`).

- **All** — sets the `compatibility` field on every mod in the file to this number
- **Select** — opens the [Mod Select dialog](#mod-select-dialog) to choose which mods to update

### Version

Enter a version string (e.g. `1.0`).

- **All** — sets the `version` field on every mod in the file
- **Select** — opens the [Version Select dialog](#version-select-dialog) to choose which mods to update

### GitHub

Connects to a GitHub repository so the editor can browse its files when filling in URL fields.

1. Paste your repository URL: `https://github.com/owner/repo`  
   The field border turns **green** for a valid URL or **red** for an invalid one.
2. Click **↺ Fetch Files** — the editor calls the GitHub API, detects the default branch, and fetches the full file tree.
3. Status line shows the number of files loaded (e.g. `✅ 47 files from main`).

**Auto-detect** — When you open a `modinfo.json` that already has GitHub raw URLs in it, the editor automatically detects the repository and fetches the file tree without you typing anything. If the file references more than one repository, a list appears so you can pick which one to use.

---

## GitHub File Browser

Opens when you click **⋯** on any URL field (Image URL, Readme URL, EXMODZ, PAK), provided a repository has been fetched.

- **Filter tabs** — narrow the list by file type:
  - All · Images · Readme · EXMODZ · PAK
  - Each tab shows the number of matching files
- **Search** — filter by filename or path
- **Click** — selects a file (preview URL shown at the bottom)
- **Double-click or Select** — fills the URL field and closes the dialog
- **Clear** — removes the URL from the field

The browser constructs raw GitHub URLs automatically:
- Images and readme files → `https://github.com/{owner}/{repo}/raw/{branch}/{path}`
- `.exmodz` and `.pak` files → `https://github.com/{owner}/{repo}/raw/refs/heads/{branch}/{path}`

---

## Mod Select Dialog

Used by **Icarus Week → Select** and **Version → Select**.

A two-panel dialog for applying a value to a specific subset of mods:

- **Left panel** — category filter, derived automatically from the subfolder path in each mod's file URLs (e.g. if your EXMODZ URL is `.../Weapons/MyMod.exmodz`, the category is `Weapons`)
- **Right panel** — list of mods matching the selected category, each with a checkbox
- **Select all** checkbox at the top (scoped to current category filter)
- Shows each mod's current `compatibility` or `version` value below its name

Click **Apply to N mods** to write the value and close.

---

## Changes Dialog

Tracks every field-level edit since the last save (or last revert).

Open it from the toolbar's **Changes** button (when available).

- Lists changed mods with a `#N` badge and mod name
- Each changed field shows:
  - Old value in **red** (−)
  - New value in **green** (+)
- **Revert** button per field — individually reverts a single field without affecting others

If nothing has changed, the dialog shows "No changes since last revert point".

---

## Saving

Changes are not written to disk until you explicitly save.

- **Ctrl+S** — save the file (keyboard shortcut, handled by the app)
- Toolbar **Save** button
- A dirty indicator (unsaved changes marker) appears on the tab when edits are pending

After saving, the snapshot is updated and the Changes dialog will show no changes.

---

## Tips

- Keep files hosted on **GitHub** rather than external hosts — ICARUS and the GitHub File Browser both work best with raw GitHub URLs.
- Use **Icarus Week → All** each update cycle to bump `compatibility` on every mod at once.
- Use the **Version suffix toggle** to append `.{week}` to your version string (e.g. `1.0.220`) so players and the game can identify which update a mod targets.
- The **Mod Select dialog** category filter reads folder names from your file URLs, so organising your GitHub repo into subfolders (`Weapons/`, `Armor/`, etc.) makes bulk updates much faster.
- **Duplicate** a mod entry when adding a variant of an existing mod — it copies all fields so you only need to change what differs.
