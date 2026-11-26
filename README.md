# Datez 1.0.1

QML-only per-page date tagging and calendar view for **reMarkable Paper Pro**, firmware **3.28.0.172**, with XOVI and `qt-resource-rebuilder`.

**Author:** Nick Greenfield  
**Version:** 1.0.1  
**Licence:** GPL-3.0-or-later

## What it does

1. Adds a calendar icon button at the top right of any open document (`rightMargin: 720`, slot 6 — one slot left of Tagz at `620`).
2. Tap it to open the **Datez** panel: a Monday-start month calendar, **Today** / **Set on this page** / **Clear** buttons, an **Auto-date new pages** toggle, a **Pages on \<date\>** list for the selected day, and an **All dated pages** collapsible month/day list.
3. Dates are stored as official cloud-synced ISO `pageTags` (e.g. `2026-11-26`) via `LibraryController.setPageTags` — no raw `.content` writes while `xochitl` is open, no Go daemon, and no private local database.
4. Because dates are official `pageTags`, they sync to the reMarkable cloud and work with stock tag filtering. Tapping any dated page in the Datez calendar or list jumps straight to that page by stable page ID.

## Coexistence with other mods

Separate `.qmd` file (`/home/root/xovi/exthome/qt-resource-rebuilder/datez.qmd`) alongside Stickerz (`120`), Folderz (`220`), Shortcutz (`320`/`420`/`520`), and Tagz (`620`).

- Uses `dtz*` state prefixes on the navigator root so it never collides with other mods.
- Imports `xofm.libs.library 1.0` (matching Tagz) and `Qt.labs.folderlistmodel 2.9` (matching Folderz), so it works standalone or alongside Tagz and Folderz.

## Install on Windows

1. Plug the Paper Pro in over USB and unlock it.
2. Run `Datez-1.0.1-Setup.exe` (SHA-256 `2d3f687abf0831f4b2f988d931ade162da2bd0774915622840e2989ec59fee76`). If Windows SmartScreen warns, verify the SHA-256 and choose **More info → Run anyway**.
3. Enter the tablet IP (default `10.11.99.1`), username (`root`), and the SSH password from **Settings → General → Help → About → Copyrights and licences** (never saved).
4. Choose **1 — Install or update** (or **2 — Uninstall**).
5. Open **reManager**, **rebuild the hashtab**, and **restart XOVI / xochitl**. The installer never restarts the UI automatically.

### Manual (scp + ssh)

```
scp -r datez-1.0.1 root@10.11.99.1:/home/root/datez
ssh root@10.11.99.1
sh /home/root/datez/install.sh
```

Then rebuild the hashtab and restart XOVI / xochitl in reManager.

### What install.sh does

* Backs up `qt-resource-rebuilder`.
* Installs only under `/home/root`.
* Never touches `/etc`, rootfs, mounts, or `systemctl`.
* Leaves other `.qmd` mods untouched.

## Uninstall

Run `Datez-1.0.1-Setup.exe` and choose **2 — Uninstall**, or over SSH:

```
sh /home/root/datez/uninstall.sh
```

Then rebuild the hashtab and restart XOVI / `xochitl` in reManager.

## Known limits and risks

* **Session-only Auto-date in 1.0.1:** The **Auto-date new pages** toggle is stored on the navigator root (`dtzAutoDate`) and resets when `xochitl` restarts (avoids Qt 6 `QtCore/Settings` conflicts with `xochitl`'s own `Settings` singleton).
* **Firmware pinned to `3.28.0.172`:** QMD resource hashes are specific to `3.28.0.172` (keep auto-updates off; do not blindly apply to newer firmware).
* **XOVI tether / recovery:** If a hash ever fails to resolve after an update, uninstall Datez, rebuild the hashtab, and restart XOVI in reManager.
* **Cloud-only / not downloaded notebooks:** Opening an undownloaded notebook cannot jump to its page until downloaded.

## Provenance and credits

* Inspired by the r/RemarkableTablet discussion [*Suggestion: Date-tagging / date search*](https://www.reddit.com/r/RemarkableTablet/comments/1y5d3qc/suggestion_datetagging_date_search/).
* Clean-room QML implementation — no code copied from `guibor/notebook-date-index` (which has no open-source licence selected).
* Credits: asivery (XOVI, `qt-resource-rebuilder`, `qmldiff`), alefaraci, ingatellent, and Nick Greenfield's Stickerz / Folderz / Tagz patterns.

## Licence

GPL-3.0-or-later. Author: Nick Greenfield.
