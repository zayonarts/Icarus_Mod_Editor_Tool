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
- Fixed update progress bar: now streams download chunks with live percentage
- Fixed batch script closing after successful update; stays open only on error
- Fixed "batch file cannot be found" error during self-replacement

## v1.4.205 — March 19, 2026
- Redesigned changelog modal: styled version cards, collapsible entries, latest pinned open
- Auto-updater overhaul: streaming download progress, UpdateBanner and UpdateGate components
- Added 45s CDN cache retry to ensure update detection on fresh installs

## v1.4.204 — March 19, 2026
- Test update: verify update progress button styling and streaming download

## v1.4.203 — March 19, 2026
- Test update: verify optional UpdateBanner display and styling

## v1.4.202 — March 19, 2026
- Test update: version bump to verify the auto-updater flow

## v1.4.201 — March 19, 2026
- Initial public release
- Modinfo Editor module
