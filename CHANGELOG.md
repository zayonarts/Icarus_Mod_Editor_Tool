## v1.5.431 - April 16, 2026
[Update Message]: Minor Updates and Scripts Module on Dev Build.
- [DEV] App: Fixed changelog missing "Scripts" label.
- [DEV] Scripts: Working on a Python Script runner, needs expanding of code, partially functional.
- [DEV] App: Backend fixes and optimizations.

## v1.4.417 - April 7, 2026
[Update Message]: Minor Updates.
- [DEV] Talent Editor: Added "Round Snap" button (in the controls bar, next to the snap grid dropdown) controls *how* snapping is applied when dragging nodes.

## v1.4.416 - April 3, 2026
[Update Message]: Talent Editor Heavy Rework!
- [DEV] Talent Editor: Fixed a quick quick quick fix for a dropdown missed redesign.
- [DEV] Talent Editor: Asset picker rebuilt from scratch, looks clear and visually pleasing for users.
- [DEV] Talent Editor: Changed few fields and their behavior, pickers generalized.
- App: Installed local MCP server for better testing purposes and faster app development.
- [DEV] Talent Editor: Visual Overhaul to fit the rest of the app.
- [DEV] Talent Editor: added all missing fields and structure for properties panel.
- App: minor fixes to update process.

## v1.4.409 - April 3, 2026
[Update Message]: Visual Overhaul Remaining Stuff - Wiki and Base Recipe Viewer. + Minor Fix
- Recipe Editor: fix right click menu positioning offset for input/output.
- App: disabled native right click menu and built custom one for very strict needs.
- Modinfo Editor: fixed some visual bugs of fields/right click menus.
- Recipe Editor: fixed some visual bugs of fields/right click menus.
- Recipe Editor: moved Feature Level out of Advanced options.
- Recipe Editor: the Base Recipe viewer overhaul, forgot completly about that as well.
- Recipe Editor: fix file closing issue, flashing white border on close.
- App: Wiki Visual Overhaul, forgot about it also fixed pop-up issue.
- App: updater now checks every 5 minutes while app is open for updates.

## v1.4.400 - April 3, 2026
[Update Message]: Complete Visual Overhaul - Modinfo Editor & Recipe Editor & Changelogs.
- App: A lot of other changes done that are not listed.
- App: update status bar: Complete Visual Overhaul.
- App: changelog: Complete Visual Overhaul.
- Recipe Editor: Complete Visual Overhaul.
- Recipe Editor: fix - UTF-8 opening files that had different UTF-8 used was not read, now everything is allowed
- App: fix - loading mod files that were saved with BOM encoding no longer causes a "not valid UTF-8" error
- App: fix - "Remove from Recent" in the right-click file menu now works correctly
- Modinfo Editor: Complete Visual Overhaul.
- Modinfo Editor: fix - the editor panels no longer briefly flash into view before the file has finished loading
- Modinfo Editor: fix - improved internal load tracking to prevent a rare edge case where the editor could mistakenly appear before the file was fully ready
- App: fix - the accent colour highlight on hovered Recent Files entries now fades in smoothly instead of snapping on instantly
- App: fix - the Recent Files dropdown and right-click menu now correctly use the app's theme colours for their glass-style background
- App: fix - the accent colour (set in the colour picker) now applies correctly inside the Recent Files dropdown
- App: fix - the Recent Files dropdown and context menu now show the correct glass blur effect; previously the toolbar was silently blocking the blur from appearing
- [DEV] Item Editor: feature - added a Feature Level field at the very bottom of the item editor; lets you set which game feature tier the item requires; selecting (none) removes the field from the output entirely
- Modinfo Editor: fix - the GitHub file picker was cutting off the footer and action buttons when the file list was long; the dialog now has a fixed height so everything fits correctly
- Modinfo Editor: restyle - the GitHub file picker dialog has been visually updated to match the rest of the app's popup style
- App: feature - a new shared popup layout is now used across all app dialogs (sticky header, tabs, scrollable body, footer) making them look and behave consistently
- App: feature - all dialogs now use a proper dark backdrop that always appears behind the dialog window
- App: feature - scrollbars inside popup dialogs are now slim and subtle
- App: fix - several shared styling classes were cleaned up and made consistent across the whole app
- App: fix - several elements that were hardcoded to a fixed blue colour now correctly respond to the module's accent colour picker
- App: fix - the status bar badge and the file path strip now correctly update colour when you change the accent colour picker
- [DEV] Item Editor: feature - all item and asset pickers now scroll to and highlight the currently selected value when you open them

## v1.4.343 - March 29, 2026
[Update Message]: Talent Editor graph improvements - rename safety, wheel zoom, and boundary guides.
- [DEV] Talent Editor: fix - typing a rename for a node and then clicking away or onto another node no longer silently discards what you typed; the rename is now always saved before switching selection
- [DEV] Talent Editor: fix - scrolling the mouse wheel over the graph canvas now always zooms in and out; it was previously panning the view up and down when Ctrl was not held
- [DEV] Talent Editor: feature - dashed guide lines now appear at the X=0 and Y=0 axes on the graph canvas, giving a clear visual anchor for the coordinate origin
- [DEV] Talent Editor: feature - a dashed boundary line now appears at Y=1350 on Blueprint tier pages to mark the layout boundary for that tree
- [DEV] Talent Editor: fix - typing a rename and clicking elsewhere without pressing Enter no longer loses the name you typed
- [DEV] App: fix - the Recent Files list for the Item Editor and Talent Editor was being wiped every time the app restarted
- [DEV] Talent Editor: fix - custom talent tree pages were no longer appearing in the tab bar after the drag-and-drop fix; they now always show correctly on file load; also fixed a bug where doing a second drag would move the wrong item
- [DEV] Talent Editor: improve  dragging custom talents to reorder them is now much smoother; the dragged item is replaced by an invisible placeholder, the ghost follows your cursor correctly, and the drop zone is clearly marked
- [DEV] Talent Editor: fix - when renaming multiple talents one after another, only the last rename was being saved; all renames now apply correctly in sequence

## v1.4.334 - March 28, 2026
[Update Message]: Item Editor fixes  duplicate menu, recent files, and a build type error.
- App: fix - the Recent Files list and accent colour settings for the Item Editor and Talent Editor were being lost every time the app restarted
- [DEV] Item Editor: fix - the Duplicate item menu (shallow/deep copy) was disappearing the moment the mouse moved toward it; it now stays open correctly
- Recipe Editor: fix - fixed an internal code error that had no impact on users

## v1.4.331 - March 28, 2026
[Update Message]: Recipe Editor field order fix + multi Recipe Set support & Startup loading screen added.
- App: feature - the app now shows a proper animated loading screen on startup instead of a frozen blank window; a progress bar moves through the startup steps and fades out when everything is ready
- Recipe Editor: feature - the Recipe Sets field now supports picking multiple sets at once; a multi-select window opens, lets you toggle sets on and off, and closes when you press Done
- Recipe Editor: fix - saving a recipe no longer rearranges the field order in the output file; fields are always written in the order the game expects

## v1.4.328 - March 28, 2026
[Update Message]: Tiny Item Editor fix & Update banner now shows the release message..
- App: fix - the update notification banner now shows the actual release message from the changelog instead of just a generic version number
- [DEV] Item Editor: fix - Skinning Efficiency was silently being removed when modding knives, machetes, and sickles; it is now correctly preserved

## v1.4.326 - March 28, 2026
[Update Message]: Changelog and Scroll Fixes.
- App: fix - Recipe Editor changes were not appearing under the Recipe Editor section in the changelog modal
- App: fix - developer-only changelog entries were appearing in the wrong section of the changelog modal
- [DEV] Item Editor: fix - switching between items was resetting the scroll position back to the top of the edit form; it now stays where you left it

## v1.4.323 - March 28, 2026
[Update Message]: Tiny fix for Item Editor, DEV Only.
- [DEV] Item Editor: fix - the item pickers inside the Durability and Recipe Set editors were replaced with the modern tabbed browser (All / Base Game / Modded) used everywhere else in the app

## v1.4.322 - March 28, 2026
[Update Message]: Launched the Recipe Editor !!! - New feedback button, feel free to report anything, give me suggestions or bugs, i'll sort trough them xD.
- App: feature - a wiki button was added to the Modinfo Editor and Recipe Editor toolbars
- App: release  the Recipe Editor is now publicly available to all users; it no longer requires a special unlock key
- [DEV] Item Editor: fix - fixed an internal code error that had no impact on users
- Recipe Editor: feature - the recipe list and editor panels now have a draggable divider so you can resize them; your chosen width is saved and restored between sessions; right-clicking the divider lets you reset it to the default width
- [DEV] App: change  the footer "bug report" button was renamed to "Feedback" to better reflect that it also accepts suggestions
- [DEV] App: feature - a Feedback button was added to the footer bar; clicking it opens a GitHub Issues page where you can report bugs or suggest features
- [DEV] Item Editor: feature - added full Item Template support; the editor now shows whether an item has a template entry and lets you attach, detach, and configure it including Dynamic Data fields; renaming an item correctly updates its template entry
- [DEV] Item Editor: feature - when renaming an item that is referenced elsewhere in the mod file, you can now choose to rename everything ("Rename + update refs"), rename only the item row ("Item only"), or cancel
- [DEV] Item Editor: feature - the item list and editor panels now have a draggable divider so you can resize them; your chosen width is saved and restored between sessions
- [DEV] Item Editor: feature - items in the list can now be dragged to reorder them; a ghost follows your cursor and a placeholder marks where the item will land; the list auto-scrolls when you drag near the edges
- [DEV] Item Editor: feature - hovering over a truncated item name in the list for 3 seconds shows a tooltip with the full name
- [DEV] Item Editor: fix - reordering items by dragging now correctly marks the file as modified and enables the Save button
- [DEV] Item Editor: fix - Item Template entries are now kept in the correct sorted order when items are reordered
- [DEV] Item Editor: fix - the "+ Add Stat" button in the Additional Stats section now works correctly; clicking it shows a blank row ready to fill in
- [DEV] All Editors: feature - search bars across the whole app now support multi-word searches; typing multiple words narrows results to entries that match all of them at once
- [DEV] Item Editor: fix - renaming a component row now correctly updates all references to that row across the entire mod file
- [DEV] Item Editor: fix - decimal values such as 1.45 can now be typed in numeric fields without the decimal point being swallowed mid-input
- [DEV] Item Editor: refactor  picker and browser overlays were moved into a dedicated file to improve code organisation; no user-facing change
- [DEV] Item Editor: fix - using Browse to swap a component row to one that already exists in the mod file now correctly updates the reference and cleans up the old row, instead of overwriting the existing one
- [DEV] Item Editor: fix - a Fork button () was added to modded component rows; clicking it creates a private copy of the row for the current item so changes to it won't affect other items sharing the same row
- [DEV] Item Editor: added inline Resource editor  appears when the Resource component is modded; lets you add, edit, and remove flow connections (Energy, Water, Fuel, Oxygen, Crude Oil, Refined Oil)
- [DEV] Item Editor: added inline Inventory Container editor  appears when the Inventory Container component is modded; provides fields for Inventory Info, Attachment Slot, and the inventory tick toggle
- [DEV] Item Editor: added inline Interactable editor  appears when the Interactable component is modded; lets you manage the four interaction lists (press, hold, alt-press, alt-hold) with add/remove controls
- [DEV] Item Editor: added inline Usable editor  appears when the Usable component is modded; optional fields can be added via a dropdown menu
- [DEV] Item Editor: added inline Actionable editor  appears when the Actionable component is modded; optional boolean fields can be added via a dropdown menu
- [DEV] CSS Styling: fix - a CSS comment style was being misread by the parser, causing phantom entries to appear in the Design Panel; now handled correctly
- [DEV] CSS Styling: improve  the dark theme CSS section now has the same category labels as the root section, so all sections are visible in the Design Panel
- [DEV] CSS Styling: improve  CSS sections are now shown as collapsible cards in the Design Panel
- [DEV] CSS Styling: improve  colour properties in the Design Panel now show a colour picker swatch next to the value field
- [DEV] CSS Styling: fix - a CSS comment was using curly-brace notation that confused the parser; changed to parenthesis notation
- [DEV] Design Panel: fix - the panel now fills the content area correctly without overflowing below the status bar
- [DEV] Design Panel: fix - the property list and declaration list inside the panel now scroll correctly
- [DEV] App: improve  the Design Panel tab is pinned to the far right of the tab bar, cannot be dragged, and hides the IMM Path widget while active
- [DEV] App: improve  the Design Panel tab is excluded from the draggable tab section and cannot be reordered

## v1.4.288 - March 22, 2026
- [DEV] Item Editor: fix - fixed a harmless internal warning about nested component updates; no user-facing change
- [DEV] Item Editor: fix - when an Itemable component row is renamed, the internal display name and description keys now update to match the new row name
- [DEV] Item Editor: fix - the same display name and description key update now also applies when a Highlightable component row is renamed
- [DEV] Item Editor: refactor  extracted a shared helper used by several rename handlers; no user-facing change
- [DEV] Item Editor: ProcessingEditor - the DefaultRecipeSet field now marks the file as modified as you type, enabling the Save button right away instead of waiting until you click away
- [DEV] Item Editor: ProcessingEditor - the  Base (revert) button on the DefaultRecipeSet field now shows a confirmation popup before reverting, instead of reverting immediately with no warning
- [DEV] Item Editor: ProcessingEditor - the duplicate Mod button was removed from the DefaultRecipeSet field; the single correct Mod button now works as expected
- [DEV] Item Editor: ProcessingEditor - the DefaultRecipeSet Mod button now works correctly; it was previously always shown as disabled
- [DEV] Item Editor: RecipeSetEditor - the recipe set name shown in the card header now updates live as you type, instead of only after you click away
- [DEV] Item Editor: ProcessingEditor - renaming a recipe set row now also updates the display name and description keys to match the new name
- [DEV] Item Editor: ProcessingEditor - the DefaultRecipeSet row now shows a proper Mod / - Base button pair, matching the style used by other component rows
- [DEV] Item Editor: RecipeSetEditor - the Recipe Set Name and Display Text fields now show the human-readable text while editing and save in the correct format to the file
- [DEV] Item Editor: RecipeSetEditor - the Recipe Set Icon field now opens an asset browser picker instead of requiring you to type a path manually
- [DEV] Item Editor: ProcessingEditor - the mod file path is now passed through correctly so the asset browser can find files inside the mod folder
- [DEV] Item Editor: ProcessingEditor - the DefaultRecipeSet picker was upgraded to the full tabbed browser (All / Base Game / Modded) used by other pickers in the app
- [DEV] Item Editor: added inline Processing editor  appears when the Processing component is modded; exposes all D_Processing fields with the appropriate input types
- [DEV] Item Editor: fix - the EndOfMod marker is now always present and always the last entry in the file when saving; if it was accidentally missing it is automatically restored
- [DEV] Item Editor: fix - reverting a component row to base game no longer accidentally removes the EndOfMod marker from the file
- [DEV] Item Editor: component rows - added a  Base button to any modded component row; clicking it removes your override and reverts that row to base-game data
- [DEV] Item Editor: component rows - reverting a component row also removes any child row overrides that were only referenced by that component
- [DEV] Item Editor: remove item - removing an item now also cleans up any component row overrides that were only used by that item, keeping the mod file tidy
- [DEV] Item Editor: fix - component sections that become empty are no longer written as bare empty objects in the saved file
- [DEV] Item Editor: fix - clicking Mod on any component now correctly marks the file as modified and shows the Save button as active
- [DEV] Item Editor: fix - empty internal arrays are no longer written to the output file, keeping saves clean
- [DEV] Item Editor: fix - saved files now use 4-space indentation, matching the format used by the rest of the app
- [DEV] Talent Editor: fix - icon download progress messages were showing raw character codes (e.g. \u2026) instead of the actual symbols; now displays correctly

## v1.4.262  March 21, 2026
- [DEV] Item Editor: fix - fixed an infinite loop that could occur when the Item Editor was open; no user-facing change
- [DEV] Item Editor: rename - the Cancel button on the rename warning panel now fully exits rename mode, instead of only hiding the warning while leaving the rename box open
- [DEV] Item Editor: rename - the rename warning panel now renders full-width below the name row, keeping the Remove button fixed in the top-right corner
- [DEV] Item Editor: rename - renaming an item to a base-game item's name now shows a warning before allowing it; you must click Proceed to confirm
- [DEV] Item Editor: rename - renaming an item to a name already used by another item in the same mod file is now blocked entirely with an error message
- [DEV] Item Editor: rename - confirming a rename also updates all other places in the mod file that reference this item by name
- [DEV] Item Editor: rename - if the item is referenced elsewhere in the mod file, a warning now lists every affected row before you can confirm the rename
- [DEV] Item Editor: rename - renaming now requires pressing OK or Enter to confirm; pressing Escape or - cancels without making any changes
- [DEV] Item Editor: fix - reverting an item now also correctly restores all linked component rows to their saved state
- [DEV] Item Editor: fix - reverting a renamed item now correctly restores the original name and all field values without losing any independent edits
- [DEV] Item Editor: renaming now shows as a single "renamed" entry in the Changes panel instead of a separate add and remove pair
- [DEV] Item Editor: Changes panel - the panel now auto-closes when all changes have been individually reverted
- [DEV] Item Editor: Changes panel - a Revert All button lets you restore everything to the last-saved state at once
- [DEV] Item Editor: Changes panel - each changed field now has its own  revert button so you can undo individual changes without affecting others
- [DEV] Item Editor: Changes panel - each item shows which fields changed, with the old value in red strikethrough and the new value highlighted in green
- [DEV] Item Editor: added a Changes panel  accessible via the toolbar Changes button; lists all pending additions, edits, renames, and removals
- [DEV] Item Editor: item list - a coloured dot now appears next to modified items; green for newly added items, amber for modified or renamed ones
- [DEV] Item Editor: mod file is automatically created blank if the selected file path does not yet exist
- [DEV] Item Editor: all edits are held in memory and only written to disk when you click the Save button; Save is disabled when there is nothing new to save
- [DEV] Item Editor: DeployableSetup - the SnapActorTags and SnapSocketsOrTags fields now use a chip-style tag editor instead of a plain comma-separated text input
- [DEV] Item Editor: DeployableSetup - the Add Field dropdown now scrolls correctly when the list is long
- [DEV] Item Editor: Deployable variants - the copy button label changed from "Mod Setup" to "Mod" to match other editors
- [DEV] Item Editor: DeployableSetup - the asset browser was restyled to match the row browser look used elsewhere in the app
- [DEV] Item Editor: Deployable variants - the Browse button was moved to after the Mod button for visual consistency
- [DEV] Item Editor: Deployable variants - added a Browse button to pick setup rows from a tabbed browser (All / Base Game / Modded)
- [DEV] Item Editor: Deployable variants - the section header now shows "D_DeployableSetup" as a subtitle for clarity
- [DEV] Item Editor: added inline Deployable Setup editor  appears when a Deployable variant row is modded; lets you edit blueprint, icon, mesh, sound, and placement settings
- [DEV] Item Editor: added inline Deployable component editor - lets you configure behaviour, weather flags, audio occlusion, and manage the variant list
- [DEV] Item Editor: renaming an item now also renames all linked component rows that shared the old item name (Attachments, Meshable, Itemable, Durable, and more); custom-named rows are left untouched
- [DEV] Item Editor: item names are now editable  click the pencil icon next to the name to rename; press Enter or click away to confirm, or press Escape to cancel; duplicate names are rejected
- [DEV] Item Editor: gameplay tags (such as Traits.Deployable) are now automatically kept in sync when you add or remove a component from an item
- [DEV] App: changelog modal - the Item Editor category label was incorrectly showing as "Item Creator"; now fixed
- [DEV] App: changelog modal - "All Editors" and "Item Editor" developer entries now correctly appear under their own labelled sections
- [DEV] App: changelog modal - removed duplicate category entries

## v1.4.227  March 20, 2026
- App: the download progress bar on the update prompt now shows live progress and a percentage
- [DEV] All Editors: added a subtle tinted background pattern to each module's empty state, tinted to the module's accent colour
- Modinfo Editor: the background pattern is visible only when no file is open
- Recipe Editor: the background pattern is visible only on the start screens (no IMM path set, or no file open)
- [DEV] Item Editor: the background pattern is visible only on the start screens (no IMM path set, or no file open)
- [DEV] Talent Editor: the background pattern is visible only on the no-IMM-path screen

## v1.4.221  March 20, 2026
- App: changelog entries are now grouped by module with colour-coded dividers
- App: developer-only changelog entries are now shown in a separate dimmed section at the bottom of each version
- App: changelog text now auto-capitalises the first letter of each entry

## v1.4.218  March 19, 2026
- [DEV] Build: removed bundled development talent icons from the installer, making it significantly smaller
- [DEV] Talent Editor: talent icons are now fetched from GitHub on first launch and cached next to the app; subsequent launches use the local copies
- [DEV] Talent Editor: added a reload button to refresh icons from the local cache without re-downloading
- [DEV] Talent Editor: newly downloaded icons now show up immediately without needing to restart the app
- Modinfo Editor: a warning is now shown if a mod has an empty description, since Daedalus rejects mods with no description
- Modinfo Editor: mods with no description now show a red  badge in the mod list
- Modinfo Editor: empty optional fields are now automatically cleaned up when loading a file, so they are not written back unnecessarily
- Modinfo Editor: mods with duplicate names now show an amber - badge in the mod list

## v1.4.210  March 19, 2026
- App: fixed the update download progress bar  it now shows live progress as each chunk arrives
- App: fixed the update helper script - it now stays open only when there is an error, instead of always closing immediately after finishing
- App: fixed a "batch file cannot be found" error that was occurring during the self-update process

## v1.4.207  March 19, 2026
- App: the changelog modal was redesigned with styled version cards, collapsible entries, and the latest version pinned open by default
- App: the auto-updater was overhauled with a live download progress bar, an update notification banner, and an update gate screen
- App: added a retry on startup to ensure the update checker reliably detects new versions on fresh installs

## v1.4.204  March 19, 2026
- App: test update - verify update progress button styling and streaming download

## v1.4.203  March 19, 2026
- App: test update - verify optional UpdateBanner display and styling

## v1.4.202  March 19, 2026
- App: test update - version bump to verify the auto-updater flow

## v1.4.201  March 19, 2026
- App: initial public release
- Modinfo Editor: initial release