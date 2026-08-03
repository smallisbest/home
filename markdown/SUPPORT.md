# Support — MarkdownViewer

## Contact support

Have a question, found a bug, or want to request a feature? Email
**offsidus@gmail.com** — every message gets a reply directly from the
developer.

## Full manual

The complete feature walkthrough (opening files, Mermaid diagrams,
split-screen editing, GitHub integration, exporting, keyboard shortcuts)
is available in two places:

- **In the app**: Help menu, or the Help button on the launch screen
- **Online**: [English](HELP.en.html) · [한국어](HELP.ko.html)

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
