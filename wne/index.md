# WNE — Wired Network Emulator

An L2 network emulator for **Linux, macOS, and Windows** — register LAN/
Wi-Fi interfaces as nodes, bridge them together, and inject Bypass /
Packet Loss / Bandwidth / Delay / Jitter impairments on the traffic
between them, all from a browser-based UI.

Linux, macOS, Windows 용 L2 네트워크 에뮬레이터입니다 — LAN/Wi-Fi
인터페이스를 노드로 등록하고 브리지로 묶어, 그 사이 트래픽에 Bypass /
Packet Loss / Bandwidth / Delay / Jitter 임페어먼트를 웹 UI에서 바로
적용할 수 있습니다.

## Download / 다운로드

**Linux — Ubuntu 24.04+ (.deb)**

**[⬇ Download .deb →](downloads/wne-latest_amd64.deb)** (v0.1.1)

```
sudo apt install ./wne-latest_amd64.deb
```

**macOS — 14+ (.pkg)**

**[⬇ Download .pkg →](downloads/wne-latest.pkg)** (v0.1.1)

```
sudo installer -pkg wne-latest.pkg -target /
```

**Windows — 10/11/Server 2019+ (.zip)**

**[⬇ Download .zip →](downloads/wne-windows-latest.zip)** (v0.1.1)

압축 해제 후 관리자 권한으로 `install.ps1` 실행. Python 3.10+ 와 Npcap
(WinPcap 호환 모드)이 필요합니다.
Extract, then run `install.ps1` as Administrator. Requires Python 3.10+
and Npcap (installed in WinPcap-compatible mode).

All three builds share the same config schema, Web UI, and CLI
(`wnectl`) — a `config.json` written on one OS loads on the others as-is.

세 빌드 모두 동일한 설정 스키마 / Web UI / CLI(`wnectl`)를 공유합니다 —
한 쪽에서 만든 `config.json` 을 다른 OS에 그대로 가져다 쓸 수 있습니다.

기본 로그인 / Default login: `admin` / `admin` (설치 후 즉시 변경하세요 /
change immediately after install) · Web UI: `http://<host>:8080`

## Manual / 매뉴얼

- [Read in English →](HELP.en.html)
- [한국어로 보기 →](HELP.ko.html)

## Source / 소스 코드

이 프로젝트는 오픈소스입니다. 소스 코드, 이슈 트래커, 상세 구현 문서는
GitHub 저장소를 참고하세요.

This project is open source. Source, issue tracker, and in-depth
implementation docs live in the GitHub repository.

**[github.com/outsidus/wne →](https://github.com/outsidus/wne)**

## Other

- [Support](SUPPORT.html)
- [Privacy](PRIVACY.html)
