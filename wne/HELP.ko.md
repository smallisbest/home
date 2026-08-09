# WNE 사용 설명서

[← 다운로드 페이지로](index.html) · [English version](HELP.en.html)

WNE(Wired Network Emulator)는 여러 LAN/Wi-Fi 인터페이스를 **노드**로
등록하고, 두 노드를 **브리지**로 묶어 그 사이 트래픽에 임페어먼트(지연,
지터, 대역폭 제한, 패킷 손실)를 부여하는 L2 네트워크 에뮬레이터입니다.
Linux / macOS / Windows 세 플랫폼에서 동일한 설정 스키마와 웹 UI로
동작합니다.

---

## 목차

1. [설치](#1-설치)
2. [첫 로그인](#2-첫-로그인)
3. [기본 개념](#3-기본-개념)
4. [노드와 브리지 만들기](#4-노드와-브리지-만들기)
5. [임페어먼트 설정](#5-임페어먼트-설정)
6. [Wi-Fi AP](#6-wi-fi-ap)
7. [CLI (`wnectl`)](#7-cli-wnectl)
8. [자주 묻는 질문](#8-자주-묻는-질문)
9. [더 알아보기](#9-더-알아보기)

---

## 1. 설치

### Linux (Ubuntu 24.04+)

```bash
sudo apt update
sudo apt install ./wne-latest_amd64.deb
```

apt 가 필요한 의존 패키지(`python3`, `iproute2`, `nftables`, `hostapd`
등)를 자동으로 함께 설치합니다. 설치 후 systemd 서비스(`wned`,
`wne-web`)가 자동으로 등록되고 실행됩니다.

### macOS (14+)

```bash
sudo installer -pkg wne-latest.pkg -target /
```

Homebrew 로 Python 3.12 와 libpcap 이 없으면 먼저 설치가 필요할 수
있습니다: `brew install python@3.12 libpcap`. 설치 후 launchd 데몬
(`com.wne.daemon`, `com.wne.web`)이 자동으로 실행됩니다.

### Windows (10/11/Server 2019+)

1. `.zip` 압축을 원하는 위치에 해제
2. **Npcap** 설치 — [npcap.com](https://npcap.com/#download) 에서
   **"Install Npcap in WinPcap API-compatible Mode"** 옵션을 반드시 체크
3. 관리자 권한 PowerShell 에서:
   ```powershell
   powershell -ExecutionPolicy Bypass -File .\install.ps1
   ```

Task Scheduler 에 `wned` / `wne-web` 태스크가 SYSTEM 계정으로 부팅 시
자동 실행되도록 등록됩니다.

### 설치 확인

세 OS 모두 설치 후 브라우저로 `http://<호스트>:8080` 에 접속하면 같은
웹 UI 가 뜹니다.

---

## 2. 첫 로그인

- 기본 계정: `admin` / `admin`
- 로그인 후 우측 상단 **비밀번호** 버튼으로 즉시 변경하세요.
- 외부에서 접근 가능한 네트워크에 설치했다면 방화벽으로 8080 포트
  접근을 제한하는 것을 권장합니다.

---

## 3. 기본 개념

| 용어 | 의미 |
|---|---|
| **노드** | LAN 또는 Wi-Fi 인터페이스 하나에 대응하는 논리 ID |
| **브리지** | 노드 2개를 묶어 그 사이 L2 트래픽을 포워딩 |
| **L / R** | 브리지의 첫 번째 클릭한 노드 = L(Start), 두 번째 = R(Term) |
| **Direction** | `L→R` / `L←R` / `L↔R` — 임페어먼트를 적용할 방향 |
| **임페어먼트** | Bypass / Delay / Jitter / Bandwidth / Loss |

설치 직후에는 시스템의 네트워크 인터페이스가 자동으로 노드로
등록되고, 브리지는 없는 빈 상태로 시작합니다.

> ⚠️ **주의**: 지금 SSH/원격 접속에 쓰이는 인터페이스는 브리지 멤버로
> 넣지 마세요. 브리지 멤버가 되면 그 인터페이스는 순수 L2 포워딩
> 전용으로 전환되어 접속이 끊길 수 있습니다.

---

## 4. 노드와 브리지 만들기

웹 UI 캔버스에는 노드들이 원형으로 배치됩니다.

- **노드 추가**: 좌상단 **+ 노드** 버튼
- **노드 편집**: 노드 클릭 → 우측 패널에서 Interface, Type(LAN/WiFi),
  Link state(UP/DOWN) 편집
- **브리지 생성**: 노드 하나를 다른 노드 위로 **드래그 앤 드롭**. 먼저
  클릭한 노드가 L, 놓은 노드가 R
- **브리지 편집**: 브리지(선 또는 가운데 원) 클릭 → 우측 패널에서
  Direction, Bridge state(ENABLED/DISABLED), 임페어먼트 편집
- **저장 및 적용**: 우측 상단 버튼 — 편집 내용이 즉시 시스템에 반영됨
- **⚙ Commands**: 지금 설정이 적용될 때 실행될 명령을 미리보기 (복사 /
  메일 / 파일 저장 가능)
- **🔌 인터페이스**: WNE 가 사용할 수 있는 인터페이스를 화이트리스트로
  제한

---

## 5. 임페어먼트 설정

브리지를 선택하면 우측 패널에서:

- **Bypass**: 체크하면 모든 임페어먼트 무시, 순수 포워딩만
- **Delay (ms) / Jitter (ms)**: 지연시간과 그 변동폭
- **Bandwidth (kbps)**: 대역폭 상한
- **Packet Loss**:
  - **퍼센트**: 매 프레임을 독립 확률로 드롭
  - **N개당 1개**: 정확히 N번째 프레임마다 드롭
  - **비트 패턴** (예: `0001100000111`): 패턴을 순환하며 1인 자리를 드롭
- Loss 는 Direction 에 따라 `L→R`만, `L←R`만, 또는 양방향에 적용 가능

---

## 6. Wi-Fi AP

**Linux 에서만 완전히 지원**됩니다 (`hostapd` 기반).

1. 무선 카드가 AP 모드를 지원하는지 확인: `iw list | grep -A 10 "Supported interface modes"`
2. NetworkManager 가 점유 중이면 제외: `nmcli device set wlan0 managed no`
3. Wi-Fi 타입 노드를 등록하고, 헤더의 **📶 Wi-Fi** 버튼에서 SSID/채널/
   패스프레이즈 설정
4. 실제로 다른 네트워크와 통신하는 AP 로 쓰려면, 이 Wi-Fi 노드를 원하는
   LAN 노드와 **브리지로 묶으세요**

macOS 는 프로그래밍 가능한 AP API 가 없어, 시스템 설정의 **인터넷
공유**로 생기는 `bridge100` 을 일반 노드로 등록하는 우회 방법을
사용합니다. Windows 는 `netsh wlan hostednetwork` (레거시, 드라이버
지원 필요)를 사용합니다.

---

## 7. CLI (`wnectl`)

```bash
wnectl status            # 현재 적용된 브리지 + config 경로
wnectl get                # 현재 config JSON 출력
wnectl set backup.json    # 파일에서 config 적용
wnectl reload             # 디스크 config 다시 읽어 적용
wnectl interfaces         # 시스템 인터페이스 목록
wnectl check              # 권한/의존성/데몬 상태 진단
```

(Linux/macOS 는 `sudo` 필요. Windows 는 관리자 PowerShell 에서 `wnectl.cmd`.)

---

## 8. 자주 묻는 질문

**Web UI 가 안 열려요.**
서비스 상태 확인: Linux `systemctl status wned wne-web` / macOS
`launchctl print system/com.wne.daemon` / Windows Task Scheduler 의
`wned`, `wne-web` 항목. 방화벽이 8080 포트를 막고 있지 않은지도
확인하세요.

**저장했는데 브리지가 실제로 안 생겨요.**
노드에 등록된 인터페이스 이름이 실제 시스템과 일치하는지 확인하세요
(`wnectl interfaces`).

**SSH 접속이 끊겼어요.**
관리용 인터페이스를 브리지 멤버로 등록했을 가능성이 높습니다. 콘솔로
접속해 서비스를 잠시 멈추면(`systemctl stop wne-web wned`) 데몬이
teardown 하며 만들었던 브리지/규칙을 모두 정리합니다.

**세 OS 의 동작 방식이 다른가요?**
Linux 는 `tc`/`nftables`/`hostapd` 커널 네이티브 메커니즘을 그대로
사용합니다. macOS 는 `dummynet`/`pf` + 일부 유저스페이스 처리, Windows
는 전체 임페어먼트를 유저스페이스(Npcap 기반)에서 처리합니다. 설정
파일과 웹 UI 는 동일하지만 macOS/Windows 는 처리량이 Linux 보다 낮을
수 있습니다.

---

## 9. 더 알아보기

문제를 발견했거나 기능 요청이 있으면 [Support](SUPPORT.html) 페이지를
참고하세요.
