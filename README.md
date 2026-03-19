# Icarus Mod Editor Tool (IMET)

A desktop application for **ICARUS** modders. IMET provides a clean, module-based GUI for creating and editing mod files — no JSON hand-editing required.

Built with [Tauri](https://tauri.app) · Windows · Auto-updating

---

## Download

Grab the latest release from the [Releases](https://github.com/zayonarts/Icarus_Mod_Editor_Tool/releases/latest) page.

> **IcarusModEditorTool.exe** — portable, no installer required.

**Requirements:** Windows 10 or later (WebView2 is pre-installed on Win10/11).

---

## Modules

### Modinfo Editor

> Edit your mod's `.modinfo` JSON file through a fully visual interface.

The Modinfo Editor lets you manage all entries inside a `modinfo.json` file — the metadata file that ICARUS uses to identify and display your mod.

**Features:**

- **Open & Save** — Load any `modinfo.json` from disk. Save with `Ctrl+S` or the toolbar. Unsaved changes are tracked with a dirty indicator.
- **Mod list panel** — All mods in the file listed on the left. Search/filter by name, drag to reorder, add new entries or delete existing ones.
- **Edit panel** — Click any mod to open its full edit form on the right:
  - **General Information** — Name, Author, Version, Compatibility
  - **Description** — Free-text description field
  - **Links & Resources** — Image URL, Readme URL — with a GitHub file browser to pick files directly from any repository
  - **Files** — `.exmodz` and `.pak` file path fields, also with GitHub browser support
- **GitHub file browser** — Enter a GitHub repo (`owner/repo`) and fetch its file tree. Then browse and pick file paths without leaving the app — URLs are filled in automatically.
- **Preview / Raw toggle** — Switch between edit view and a read-only preview of the current mod's data.

---

## Auto-Updater

IMET checks for updates on launch. When a new version is available, a banner appears at the top of the window. Updates are opt-in — the app will never update without your confirmation.

---

## Coming Soon

The following modules are in development and will be released in future versions:

| Module | Description |
|---|---|
| Recipe Editor | Create and edit crafting recipes |
| Talent Editor | Manage talent tree definitions |
| Item Creator | Build new item entries |

---

## License

This project is not open source. The executable is provided as-is for use by the ICARUS modding community.
