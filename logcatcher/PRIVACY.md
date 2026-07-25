# Privacy Policy — LogCatcher

_Last updated: 2026-07-25_

LogCatcher is an Android `logcat` viewer for macOS and Windows. This policy
describes what happens to your data when you use it.

## Short version

LogCatcher does not collect, sell, or share any personal data with its
developer or any third party. There is no account, no telemetry, no
analytics, no advertising, and no crash reporting. Almost everything you see
in the app stays on your own Mac or PC — the only outbound network call is a
small, optional version check described below.

## What LogCatcher reads

- **Logcat output** from an Android device you connect via USB or a local
  `adb` server. This is the device's own diagnostic log — the same data
  shown by `adb logcat` on the command line. It is displayed in the
  LogCatcher window and never leaves your computer.
- **Local preference files** that LogCatcher itself writes, so your filters,
  colors, and window layout persist across launches (paths below).

## Network activity

**Update check.** On launch (at most once per 24 hours) and whenever you
choose *Check for Updates…*, LogCatcher fetches a small JSON file —
`https://smallisbest.github.io/LogCatcher/latest.json` — to see whether a
newer version exists. This is a plain read of a static file; no information
about you, your device, or your logs is sent as part of that request. If you
choose to download an update, your browser opens the corresponding page on
`smallisbest.github.io`, same as clicking a link.

**adb communication.** LogCatcher talks to the Android Debug Bridge (`adb`)
that's already installed on your computer, over your local machine (TCP
loopback or a direct USB endpoint). This is not a connection to any server
of ours — it's local communication with a tool you installed and with a
device you explicitly connected.

Outside of these two cases, LogCatcher makes no other network requests.

## Local files LogCatcher writes

These files stay on your own computer and are never uploaded anywhere.

**macOS**
- Preferences: `~/Library/Preferences/com.smallisbest.logcat.plist`
- Saved filters: `~/Library/Application Support/com.smallisbest.logcat/filters.json`

**Windows**
- Preferences: `HKCU\Software\com.smallisbest.logcat`
- Saved filters: `%APPDATA%\com.smallisbest.logcat\filters.json`

You can delete these at any time; LogCatcher starts fresh on the next launch.

## What LogCatcher does not do

- No account, sign-in, or user profile of any kind.
- No analytics SDKs, telemetry frameworks, advertising libraries, or crash
  reporting services.
- No data — logs, filters, or otherwise — is ever transmitted to the
  developer, to Apple, to Google, or to any third party.

## Children's privacy

LogCatcher is a developer tool aimed at adults and is not directed at
children. It does not knowingly collect personal information from anyone,
children included.

## Changes to this policy

If this policy changes materially (for example, a future feature adds a new
network destination), this page will be updated and the date above revised.

## Contact

Questions about this policy: offsidus@gmail.com
