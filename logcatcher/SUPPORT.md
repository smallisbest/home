# Support — LogCatcher

## Getting help

Full usage documentation — connecting a device, filter syntax
(`app:`, `tag:`, `level:`, `pid:`, `tid:`, `message:`, AND/OR, parentheses),
saved filter groups, search, and keyboard shortcuts — is on the project
homepage: [smallisbest.github.io/LogCatcher](https://smallisbest.github.io/LogCatcher/).

## Requirements

- macOS 11 (Big Sur) or later, **or** Windows 10 or later (x64)
- The Android SDK Platform Tools' `adb`, installed and reachable — either on
  your `PATH` or in a standard install location. LogCatcher does not bundle
  `adb`; get it from
  [developer.android.com/tools/releases/platform-tools](https://developer.android.com/tools/releases/platform-tools)
  if you don't already have it (e.g. from Android Studio).

## Common issues

- **No devices listed.** Run `adb devices` from a terminal/command prompt.
  If that fails, `adb` isn't on your `PATH`, or the device isn't authorized
  yet — unlock the phone and accept the USB-debugging prompt.
- **macOS: "app is damaged and can't be opened."** Only happens with a
  self-built (ad-hoc signed) binary — the DMG on the homepage is signed with
  a Developer ID and notarized by Apple, and opens normally. If you built
  from source, clear the quarantine flag once: `xattr -cr /Applications/LogCatcher.app`.
- **USB direct (macOS): "USB interface is held exclusively."** Another tool
  (commonly Android Studio) already has the device open. Quit it, or switch
  LogCatcher to the TCP or Subprocess connection mode in Settings.
- **USB direct: device never prompts to authorize.** Run `adb kill-server`,
  then `adb devices` once with the phone unlocked and accept the
  USB-debugging dialog on the device, then retry USB direct in LogCatcher.
- **App column shows "?".** The process map is fetched asynchronously right
  after connecting — it fills in within a second or two.
- **Windows: SmartScreen warning.** The Windows build is currently unsigned.
  Click *More info → Run anyway* only if you trust the source you downloaded
  it from.

## Contact

Bug reports, feature requests, or anything else:
[offsidus@gmail.com](mailto:offsidus@gmail.com)
