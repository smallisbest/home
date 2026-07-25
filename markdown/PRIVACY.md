# Privacy Policy — MarkdownViewer

_Last updated: 2026-07-25_

MarkdownViewer is a local markdown/SVG viewer for macOS. This policy describes
what happens to your data when you use it.

## What the app does with your files

MarkdownViewer opens and renders files you choose, entirely on your Mac. The
app runs inside Apple's App Sandbox and only ever accesses files you
explicitly open — it does not scan, upload, or transmit the contents of your
documents anywhere, except when you use one of the two network features
described below, and even then only to the destination you specify.

## Network features (both are opt-in)

**Open from URL.** If you use File > Open from URL, the app downloads the
markdown file directly from the URL you enter (a web server URL, or a GitHub
"blob" URL, which is resolved to its raw-content equivalent) so it can be
displayed. This is a direct connection from your Mac to that server —
MarkdownViewer's developer never sees this traffic or the content fetched.

**Push to GitHub.** If you choose to push edits back to GitHub, you provide a
GitHub personal access token. That token is stored only in your Mac's own
Keychain (via the system Keychain Services API) and is never sent anywhere
except in the HTTPS requests MarkdownViewer makes directly to GitHub's own
API (`api.github.com`) to read or commit the file on your behalf. You can
remove it at any time via Keychain Access.app.

## What the app does not do

- No accounts, sign-in, or user profiles.
- No analytics, crash reporting, advertising, or tracking SDKs of any kind.
- No data is collected by or transmitted to the developer's own servers —
  MarkdownViewer doesn't operate any backend at all.

## Changes to this policy

If this policy changes (e.g. a future feature adds a new network
destination), this file will be updated and the date above revised.

## Contact

Questions about this policy: melvin.kang@kakaocorp.com
