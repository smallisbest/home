# Support — WNE

## Contact / report an issue

WNE is open source. Bugs, questions, and feature requests go on the
GitHub issue tracker:

**[github.com/outsidus/wne/issues →](https://github.com/outsidus/wne/issues)**

You can also email **offsidus@gmail.com** directly.

WNE 는 오픈소스입니다. 버그 제보, 질문, 기능 요청은 GitHub 이슈
트래커를 이용해 주세요. 이메일(offsidus@gmail.com)로도 연락 가능합니다.

## Manual

- [English](HELP.en.html) · [한국어](HELP.ko.html)
- Full technical reference and internals live in the repo itself:
  `MANUAL.md`, `IMPLEMENTATION.md`, `UBUNTU_GUIDE.md`

## Requirements

- **Linux**: Ubuntu 24.04+ (systemd, iproute2, nftables)
- **macOS**: 14.0 or later
- **Windows**: 10 / 11 / Server 2019+, Python 3.10+, Npcap

## Known limitations

- **Wi-Fi AP mode** is fully native only on Linux (`hostapd`). macOS and
  Windows use OS-level workarounds with narrower driver support — see
  the manual's Wi-Fi AP section for each platform.
- **Windows throughput** is lower than Linux/macOS because all
  forwarding and impairments run in userspace (no in-kernel L2 bridge
  API is scriptable on Windows) — fine for emulation/testing, not for
  production gateway use.
- **Jitter** has no native primitive on macOS's dummynet; it's applied
  best-effort but is more precise on Linux (tc/netem) and Windows
  (userspace delay queue).
