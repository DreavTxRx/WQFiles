# Worse Query Files

**WQ Files** is a file browser, app data manager and cache cleaner for iOS 27.
"WQ" stands for worse_query, the sandbox technique it is built on. It reaches
other apps' and system containers with no jailbreak and no kernel exploit.
Built and tested on iOS 27.0.1.

## What it opens

- **App Data**: every app's data container (`Containers/Data/Application`),
  shown with the app's name and icon instead of a UUID.
- **App Groups**: shared group containers.
- **Extension Data** and **Internal Daemons**.
- **System Data**: `/var/containers/Data/System` (listing only; its contents
  are owned by root).
- **System Groups**: the parent cannot be listed on 27.0.1, so known container
  names are tried one by one; tap **+** to try another name. Covers the
  MobileGestalt cache and the configuration-profiles (Supervision) folder.
- **Go to Path** for any absolute path.

The container parents give no access token on 27.0.1, so their contents are
found by asking the kernel for the path of each inode number (`fsgetpath`),
the way KAMUI does it.

## Tools

- **Apps**: every app with its data size. Back up an app's data, restore the
  latest backup, clear its data (Documents, Library, tmp), open its folder, or
  share a backup as a ZIP over AirDrop, Files, Mail and so on.
- **Cache Cleaner**: cache and temp size per app, with search, Select All /
  None / Large Caches (50 MB+) and crash logs. Long-press an app to jump to its
  Caches, tmp or data folder.
- **Files**: browse, read and share files. Property lists show as formatted
  XML, images render, text is editable, anything else is a hex dump. Edit and
  **Save** writes back in place (same inode, owner and attributes). Create
  folders, import files, rename and delete. Long-press a folder to share it as
  a ZIP.
- **Log**: everything the app changed, plus the App Group it received.

App names come from LaunchServices' own database, read from lsd's container
(the LaunchServices API itself refuses this app on 27.x). Apple services that
only have localized names use a built-in table. Services without an icon get a
symbol tile.

## Safety

Before a file is overwritten or deleted, its contents are copied to
**Backups** inside the app (also reachable in the Files app). Long-press a
backup to restore it to its original location. In the file browser only empty
folders can be deleted. Clearing an app's data and cleaning caches delete
without a file-by-file backup; Apps offers to back up first. The app changes
nothing on its own; every write is an action you take.

Editing another app's or the system's files can break things. Keep the backups
and only change what you understand.

## Build and signing

`./build.sh` compiles an arm64 binary and ad-hoc signs it with an App Group
entitlement (`group.cz.kolbi.wqfiles`). The technique needs an App Group, so
re-sign with a tool that keeps one; the app uses whichever group it actually
receives and shows it at the top of the **Log**. If it reports "No App Group",
the signer dropped it.

## Credit and license

- Sandbox technique: [worse_query](https://github.com/forcequitOS/worse_query)
  by Taj C (forcequit), GPL-3.0-or-later. This app reimplements it.
- Container enumeration with `fsgetpath`: from
  [KAMUI](https://github.com/murk-sus/kamui-escape) by murk-sus.

WQ Files is GPL-3.0-or-later (see `LICENSE`).
