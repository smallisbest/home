# Support — MarkdownViewer

## Getting help

The app has built-in documentation: **Help menu** (or the Help button on the
launch screen) opens a full walkthrough of every feature — opening files,
Mermaid diagrams, split-screen editing, GitHub integration, exporting, and
keyboard shortcuts.

## Requirements

macOS 13.0 or later.

## Known limitations

- **Linked images/files outside the document's folder.** MarkdownViewer runs
  inside Apple's App Sandbox, which only grants access to files you've
  explicitly opened. Relative links/images that live in the same folder (or a
  subfolder) as the open document work normally; links pointing elsewhere on
  disk may fail to open.
- **"Open from URL" only accepts `https://`.** Plain `http://` is rejected —
  this matches macOS's default network security policy.

## Contact

Questions, bug reports, or feature requests: melvin.kang@kakaocorp.com
