# Privacy Policy — WNE

_Last updated: 2026-08-10_

WNE is a self-hosted network emulator that runs entirely on infrastructure
you control. This policy describes what happens to your data when you
use it.

## What WNE does

WNE runs a daemon and a small web server **on the machine you install it
on**. It reads your machine's network interface list and, based on the
configuration you create in the Web UI or CLI, applies bridging and
traffic-shaping rules to your own network interfaces (via `tc`/
`nftables`/`hostapd` on Linux, `dummynet`/`pf` on macOS, or a userspace
worker on Windows). None of this involves any server operated by the
developer.

## What WNE does not do

- No accounts, sign-in, telemetry, analytics, or crash reporting.
- No data — configuration, network traffic, or otherwise — is sent to
  the developer or any third party. WNE has no backend of its own.
- The only network traffic WNE's own processes generate is local to
  the host it's running on (its Web UI on `:8080`, and the daemon's
  own loopback/local-socket IPC).

## Things to be aware of yourself

- The daemon needs root (Linux/macOS) or SYSTEM (Windows) privileges to
  configure networking — this is inherent to what the tool does, not
  something WNE requests beyond what's necessary.
- The Web UI's default login (`admin`/`admin`) should be changed
  immediately, and Basic Auth is sent in the clear — if you expose the
  Web UI beyond `localhost`, put it behind a TLS-terminating reverse
  proxy (nginx, Caddy, etc.).
- Because WNE puts interfaces into bridges / promiscuous capture mode,
  it will affect real traffic on whatever interfaces you assign to it —
  double-check before bridging an interface you're using to manage the
  machine itself.

## Changes to this policy

If a future feature changes any of the above (e.g. an optional
update-check), this file will be updated and the date above revised.

## Contact

Questions about this policy: offsidus@gmail.com
