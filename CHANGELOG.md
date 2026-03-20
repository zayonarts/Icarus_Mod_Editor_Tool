## v1.4.215 — March 20, 2026
- App: update gate download button now shows live progress bar and percentage (same as update banner)
- [DEV] All Editors: added tinted repeating background pattern (IconBG-Pattern-1) tinted to each module's accent color
- Modinfo Editor: background pattern visible only when no file is selected, hidden while editing
- [DEV] Recipes Editor: background pattern visible only on no-IMM and no-file-open screens
- [DEV] Item Editor: background pattern visible only on no-IMM and no-file-open screens
- [DEV] Talent Editor: background pattern visible only on the no-IMM screen

## v1.4.209 — March 20, 2026
- App: changelog categories — changes are now grouped by module with colored dividers
- App: changelog DEV section — development-only changes shown separately, dimmed, at the bottom of each entry
- App: changelog lines now auto-capitalize

## v1.4.208 — March 19, 2026
- [DEV] Build: removed bundled development talent icons from the executable — exe is now significantly lighter
- [DEV] Talent Editor: icons are now fetched from GitHub at runtime and stored next to the exe (unchanged behaviour)
- [DEV] Talent Editor: added ⟳ reload button to refresh icons from local cache without re-downloading
- [DEV] Talent Editor: icon URLs are now cache-busted after each fetch so newly downloaded icons show immediately
- Modinfo Editor: warn when a mod's description is empty (Daedalus rejects empty-string description)
- Modinfo Editor: show red ⚠ badge on mod list items with no description
- Modinfo Editor: sanitize empty-string optional fields on file load so they are not written back
- Modinfo Editor: show amber ⚠ badge on mod list items with duplicate names

## v1.4.206 — March 19, 2026
- App: fixed update progress bar — now streams download chunks with live percentage
- App: fixed batch script closing after successful update; stays open only on error
- App: fixed "batch file cannot be found" error during self-replacement

## v1.4.205 — March 19, 2026
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
