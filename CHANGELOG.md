## v1.4.262 — March 21, 2026 — DEV-Only Update (no impact for regular users)
- [DEV] Item Editor: fix — resolved infinite re-render loop (maximum update depth exceeded) that occurred when the Items module was open; caused by the dirty-state useEffect firing on every App render due to the inline onDirtyChange prop creating a new function reference each time; now guarded by a ref that only propagates when the dirty boolean actually changes
- [DEV] Item Editor: rename — Cancel button on the warning panel now fully exits rename mode (same as the × button), instead of only dismissing the warning while leaving the rename input open
- [DEV] Item Editor: rename — warning panel (base-game override / cross-block usages) now renders full-width below the name+Remove row so the Remove button stays fixed in the top-right corner
- [DEV] Item Editor: rename — trying to rename to a base-game item name shows the same inline warning panel (same style as the cross-block usages warning); user must click Proceed to confirm the override or Cancel to go back
- [DEV] Item Editor: rename — attempting to rename to a name already used by another item in the same mod file is now a hard block (error message, no proceed path)
- [DEV] Item Editor: rename — "Proceed" confirmation step now also cascade-updates all D_ItemsStatic RowName references found in other mod blocks (e.g. recipes with this item as input/output) so references remain consistent after rename
- [DEV] Item Editor: rename — clicking OK when the item is referenced in other mod blocks now shows an inline warning listing every affected row (block file › row name) before proceeding; user must click Proceed or Cancel
- [DEV] Item Editor: rename — replaced blur-to-confirm behavior with an explicit OK button; rename only applies when OK (or Enter) is pressed; Escape or × cancels without changes; component rows are no longer auto-renamed (user manages those independently)
- [DEV] Item Editor: fix — reverting an item or an individual field now restores the corresponding original component block rows from the saved snapshot, fixing modded field indicators disappearing after a revert
- [DEV] Item Editor: fix — reverting a renamed item now correctly restores both the item name and all other field values without wiping field edits made independently before or after the rename
- [DEV] Item Editor: renaming an item now records as a single "⟳ renamed" change entry (old name → new name) instead of a separate added + removed pair
- [DEV] Item Editor: Changes panel — auto-closes when all changes have been individually reverted; Revert All button is hidden when no changes remain
- [DEV] Item Editor: Changes panel — Revert All button restores all items to their last-saved state at once
- [DEV] Item Editor: Changes panel — per-field ↩ revert button reverts an individual field change without affecting other edits on the same item; also restores the linked component block row so modded indicators remain correct
- [DEV] Item Editor: Changes panel — per-item Revert button restores that item to its last-saved state, including all linked component block rows
- [DEV] Item Editor: Changes panel — per-item card shows each changed field with old value (red strikethrough) and new value (green), matching the Recipes Editor diff style
- [DEV] Item Editor: added Changes panel (via toolbar Changes button) — lists all pending additions, modifications, renames, and removals with colored type badges and change counts in the header
- [DEV] Item Editor: item list shows colored dot indicators per item — green dot for added items, amber dot for modified or renamed items
- [DEV] Item Editor: mod file is auto-created (blank) when the selected file path does not yet exist on disk
- [DEV] Item Editor: added save caching — all edits are held in memory and only written to disk when the toolbar Save button is clicked; Save button is disabled when there are no unsaved changes
- [DEV] Item Editor: DeployableSetup SnapActorTags and SnapSocketsOrTags fields now use inline chip-style tag editor (add via Enter/comma, remove per chip) instead of comma-separated text input
- [DEV] Item Editor: DeployableSetup Add Field dropdown — fixed overflow:hidden overriding overflowY:auto, dropdown now scrolls correctly
- [DEV] Item Editor: Deployable variants — copy button label changed from "Mod Setup" to "Mod" to match all other editors
- [DEV] Item Editor: DeployableSetup asset browser restyled to match the row browser (flat monospace list, filename + game path subtitle, no folder headers)
- [DEV] Item Editor: Deployable variants — Browse button moved after Mod button for consistency with other editors
- [DEV] Item Editor: Deployable variants — added Browse button (row browser) to pick D_DeployableSetup rows from base game and mod, with All/Base Game/Modded tabs and ● Mod badges
- [DEV] Item Editor: Deployable variants section header now shows "D_DeployableSetup" subtitle for clarity
- [DEV] Item Editor: added Deployable Setup editor — D_DeployableSetup fields (blueprint, icon, mesh, sound, placement settings) shown inline per variant once modded
- [DEV] Item Editor: added Deployable component editor — D_Deployable fields (Behaviour, weather flags, audio occlusion, variants list with add/remove)
- [DEV] Item Editor: renaming an item cascades to all linked component rows that share the old item name (Attachments + Alteration, Meshable, Itemable, Durable, InventoryContainer, Decayable, Highlightable, Focusable, Interactable, Usable, Actionable, Deployable) — custom-named component rows are left untouched
- [DEV] Item Editor: item name (D_ItemsStatic row) is now editable — click the ✎ pencil icon next to the name to rename inline; confirms on Enter or blur, cancels on Escape; duplicate names are rejected with an error message
- [DEV] Item Editor: Generated_Tags are now automatically kept in sync with the item's components — adding, removing, or browsing a component updates the corresponding Traits.* gameplay tag (21 components mapped: Meshable, Itemable, Interactable, Highlightable, Hitable, Equippable, Actionable, Buildable, Consumable, Usable, Combustible, Deployable, Armour, Ballistic, Fillable, Durable, Rocketable, Inventory, Processing, Thermal, Flammable)
- [DEV] App: changelog modal — fixed "Item Editor" category label (was incorrectly displaying as "Item Creator")
- [DEV] App: changelog modal — fixed "All Editors" and "Item Editor" DEV entries now correctly grouped under their own labeled sections
- [DEV] App: changelog modal — removed duplicate category aliases (Recipe Editor / Talents Editor)

## v1.4.227 — March 20, 2026
- App: update gate download button now shows live progress bar and percentage (same as update banner)
- [DEV] All Editors: added tinted repeating background pattern (IconBG-Pattern-1) tinted to each module's accent color
- Modinfo Editor: background pattern visible only when no file is selected, hidden while editing
- [DEV] Recipes Editor: background pattern visible only on no-IMM and no-file-open screens
- [DEV] Item Editor: background pattern visible only on no-IMM and no-file-open screens
- [DEV] Talent Editor: background pattern visible only on the no-IMM screen

## v1.4.221 — March 20, 2026
- App: changelog categories — changes are now grouped by module with colored dividers
- App: changelog DEV section — development-only changes shown separately, dimmed, at the bottom of each entry
- App: changelog lines now auto-capitalize

## v1.4.218 — March 19, 2026
- [DEV] Build: removed bundled development talent icons from the executable — exe is now significantly lighter
- [DEV] Talent Editor: icons are now fetched from GitHub at runtime and stored next to the exe (unchanged behaviour)
- [DEV] Talent Editor: added ⟳ reload button to refresh icons from local cache without re-downloading
- [DEV] Talent Editor: icon URLs are now cache-busted after each fetch so newly downloaded icons show immediately
- Modinfo Editor: warn when a mod's description is empty (Daedalus rejects empty-string description)
- Modinfo Editor: show red ⚠ badge on mod list items with no description
- Modinfo Editor: sanitize empty-string optional fields on file load so they are not written back
- Modinfo Editor: show amber ⚠ badge on mod list items with duplicate names

## v1.4.210 — March 19, 2026
- App: fixed update progress bar — now streams download chunks with live percentage
- App: fixed batch script closing after successful update; stays open only on error
- App: fixed "batch file cannot be found" error during self-replacement

## v1.4.207 — March 19, 2026
- App: redesigned changelog modal — styled version cards, collapsible entries, latest pinned open
- App: auto-updater overhaul — streaming download progress, UpdateBanner and UpdateGate components
- App: added 45s CDN cache retry to ensure update detection on fresh installs

## v1.4.204 — March 19, 2026
- App: test update — verify update progress button styling and streaming download

## v1.4.203 — March 19, 2026
- App: test update — verify optional UpdateBanner display and styling

## v1.4.202 — March 19, 2026
- App: test update — version bump to verify the auto-updater flow

## v1.4.201 — March 19, 2026
- App: initial public release
- Modinfo Editor: initial release
