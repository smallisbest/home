# WNE User Guide

[← Back to download page](index.html) · [한국어 버전](HELP.ko.html)

WNE (Wired Network Emulator) is an L2 network emulator that lets you
register LAN/Wi-Fi interfaces as **nodes**, join two nodes into a
**bridge**, and apply impairments (delay, jitter, bandwidth limiting,
packet loss) to the traffic that crosses that bridge. It runs on Linux,
macOS, and Windows with the same config schema and Web UI on all three.

---

## Table of contents

1. [Install](#1-install)
2. [First login](#2-first-login)
3. [Core concepts](#3-core-concepts)
4. [Creating nodes and bridges](#4-creating-nodes-and-bridges)
5. [Configuring impairments](#5-configuring-impairments)
6. [Wi-Fi AP](#6-wi-fi-ap)
7. [CLI (`wnectl`)](#7-cli-wnectl)
8. [FAQ](#8-faq)
9. [Learn more](#9-learn-more)

---

## 1. Install

### Linux (Ubuntu 24.04+)

```bash
sudo apt update
sudo apt install ./wne-latest_amd64.deb
```

`apt` resolves and installs the required dependencies (`python3`,
`iproute2`, `nftables`, `hostapd`, etc.) automatically. The `wned` and
`wne-web` systemd services are registered and started for you.

### macOS (14+)

```bash
sudo installer -pkg wne-latest.pkg -target /
```

If Python 3.12 or libpcap aren't already on the machine, install them
first: `brew install python@3.12 libpcap`. The `com.wne.daemon` and
`com.wne.web` launchd jobs start automatically after install.

### Windows (10/11/Server 2019+)

1. Extract the `.zip` wherever you like
2. Install **Npcap** from [npcap.com](https://npcap.com/#download) —
   make sure to check **"Install Npcap in WinPcap API-compatible Mode"**
3. From an elevated PowerShell:
   ```powershell
   powershell -ExecutionPolicy Bypass -File .\install.ps1
   ```

This registers `wned` and `wne-web` as Scheduled Tasks running as
SYSTEM at boot.

### Verifying the install

On any of the three OSes, open `http://<host>:8080` in a browser — you
should see the same Web UI.

---

## 2. First login

- Default credentials: `admin` / `admin`
- Change the password immediately from the **비밀번호** (password)
  button in the top-right corner.
- If the machine is reachable from an untrusted network, restrict
  access to port 8080 with a firewall rule.

---

## 3. Core concepts

| Term | Meaning |
|---|---|
| **Node** | A logical id bound to one LAN or Wi-Fi interface |
| **Bridge** | Joins exactly two nodes and forwards L2 traffic between them |
| **L / R** | The first node you click when creating a bridge is L (Start), the second is R (Term) |
| **Direction** | `L→R` / `L←R` / `L↔R` — which way an impairment applies |
| **Impairments** | Bypass / Delay / Jitter / Bandwidth / Loss |

Right after install, WNE seeds one node per system network interface
and starts with no bridges.

> ⚠️ **Careful**: don't put the interface you're currently connected
> through (SSH, RDP, etc.) into a bridge. Once an interface becomes a
> bridge member it switches to pure L2 forwarding and may drop your
> connection.

---

## 4. Creating nodes and bridges

The Web UI lays nodes out in a circle on an SVG canvas.

- **Add a node**: the **+ 노드** button, top-left
- **Edit a node**: click it — the right panel lets you change the
  Interface, Type (LAN/WiFi), and Link state (UP/DOWN)
- **Create a bridge**: **drag one node onto another**. The node you
  clicked first becomes L, the one you dropped onto becomes R
- **Edit a bridge**: click the bridge (its line or center hub) — the
  right panel exposes Direction, Bridge state (ENABLED/DISABLED), and
  the impairment fields
- **저장 및 적용** (Save & Apply): top-right button — pushes your edits
  to the running system immediately
- **⚙ Commands**: preview the commands your current configuration would
  run (copy / email / download as a file)
- **🔌 인터페이스** (Interfaces): restrict which system interfaces WNE
  is allowed to use

---

## 5. Configuring impairments

Select a bridge, then in the right panel:

- **Bypass**: when checked, all impairments are ignored — pure forwarding
- **Delay (ms) / Jitter (ms)**: base delay and how much it varies
- **Bandwidth (kbps)**: a throughput cap
- **Packet Loss**:
  - **Percent**: each frame is independently dropped with that probability
  - **Every-N**: exactly every Nth frame is dropped
  - **Bit pattern** (e.g. `0001100000111`): cycles through the pattern,
    dropping frames on the `1` positions
- Loss can target `L→R` only, `L←R` only, or both directions, via the
  bridge's Direction setting

---

## 6. Wi-Fi AP

**Fully supported on Linux only**, via `hostapd`.

1. Check the card supports AP mode:
   `iw list | grep -A 10 "Supported interface modes"`
2. Detach it from NetworkManager if it's occupied:
   `nmcli device set wlan0 managed no`
3. Register it as a WiFi-type node, then configure SSID / channel /
   passphrase from the **📶 Wi-Fi** button in the header
4. To make it a real AP that reaches another network, **bridge** that
   Wi-Fi node with the LAN node you want it relayed to

macOS has no scriptable AP API, so WNE documents a workaround: enable
**Internet Sharing** in System Settings, then register the resulting
`bridge100` interface as a regular node. Windows uses the legacy
`netsh wlan hostednetwork` mechanism, which depends on driver support.

---

## 7. CLI (`wnectl`)

```bash
wnectl status            # currently-applied bridges + config path
wnectl get                # dump the current config as JSON
wnectl set backup.json    # apply config from a file
wnectl reload             # re-read the on-disk config and apply it
wnectl interfaces         # list system interfaces
wnectl check              # permission / dependency / daemon diagnostics
```

(Needs `sudo` on Linux/macOS. On Windows, run `wnectl.cmd` from an
elevated PowerShell.)

---

## 8. FAQ

**The Web UI won't load.**
Check service status — Linux: `systemctl status wned wne-web`; macOS:
`launchctl print system/com.wne.daemon`; Windows: the `wned` and
`wne-web` entries in Task Scheduler. Also confirm nothing is blocking
port 8080 in your firewall.

**I saved a bridge but it isn't actually forwarding traffic.**
Make sure the interface name on the node matches what the system
actually calls it (`wnectl interfaces`).

**I lost my SSH connection after saving.**
You likely bridged the interface you're managing the box through.
Connect via console and stop the services
(`systemctl stop wne-web wned`) — the daemon's teardown removes every
bridge/rule it created, and your connection should come back.

**Do the three OS builds behave identically?**
Linux uses the kernel-native `tc` / `nftables` / `hostapd` mechanisms
directly. macOS uses `dummynet` / `pf` plus some userspace handling.
Windows implements every impairment in userspace on top of Npcap. The
config file and Web UI are identical across all three, but macOS and
Windows generally have lower throughput than Linux.

---

## 9. Learn more

Found a bug or have a feature request? See the [Support](SUPPORT.html) page.
