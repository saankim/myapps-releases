# GPU Usage

Your GPUs, everywhere. A small utility for researchers and developers, with native Mac, iPhone and iPad apps, an Apple Watch companion, a VS Code dashboard, and an agent-friendly CLI.

**Beta limitation:** Production iCloud setup is pending. Cross-device settings sync and iPhone-to-Mac Pro activation do not work yet. Manual server registration and viewing work independently on each device.

[Download the current beta](https://github.com/saankim/myapps-releases/releases/tag/gpu-usage-v2.2.1) · [Privacy](PRIVACY.md)

## Install

**Mac:** Requires an Apple silicon Mac running macOS 26 or later. Unzip the release and move **GPU Usage.app** to **Applications**. Open it and use the menu-bar CPU icon. The app is signed with Developer ID and notarized by Apple.

Starting with 2.2.0, open Settings → **This Mac (이 Mac)** → **Check for Updates (업데이트 확인)** to install updates inside the app. Automatic checks can be turned off there; installation requires your action. Updates are verified with Developer ID and Sparkle signatures. Users on 2.1.2 or earlier need to install the current release manually once.

**iPhone / iPad:** Requires iOS or iPadOS 26 or later. The current version is an internal TestFlight beta; an invited Apple account is required. The public App Store listing is not yet available.

**Apple Watch:** Requires watchOS 26 or later and a paired iPhone with GPU Usage. Open the iPhone app first, then the Watch app. The Watch shows the latest iPhone readings and can request a refresh when the phone is reachable. It does not make SSH connections. Notification mirroring follows your iPhone and Watch notification settings. Companion transport and notification delivery still need confirmation on physical devices.

**VS Code:** Install the `.vsix` file from the release using Extensions → Install from VSIX. Requires VS Code 1.138 or later on macOS. Open the GPU Usage view and keep the Mac app running. The extension also works in a remote workspace because it reads the local Mac app's snapshots.

**CLI:** Download `gpu-usage`, then place it in a directory on your PATH:

```sh
mkdir -p "$HOME/.local/bin"
install -m 755 gpu-usage "$HOME/.local/bin/gpu-usage"
"$HOME/.local/bin/gpu-usage" --help
```

If `~/.local/bin` is not on your PATH, use the full path or add it to your shell's PATH. The launcher invokes the signed Mac app; keep the app in Applications. It does not replace any customized `gpu-usage.sh`.

## Connect

Open Settings → **Server Management (서버 관리)** → **Add Server (서버 추가)**, then enter an SSH address, username and key or password. Mac SSH `Host` entries appear automatically, including included configuration files; wildcard entries and proxy/jump connections are excluded. iPhone and iPad support unencrypted OpenSSH Ed25519 keys and SSH passwords. Each device still needs a network route to the server.

The first server key is remembered automatically, so the initial connection has no fingerprint confirmation step. If the key changes later, monitoring stops and the app shows a fingerprint warning. Confirm the replacement using the existing server trust dialog to reconnect. Existing trusted keys are preserved. SSH remains encrypted and requires user authentication; the app does not modify your SSH configuration or known-hosts files. Automatic first-use trust cannot detect an impersonated server on that very first connection.

Settings sync uses the same iCloud account and iCloud Keychain. There is no QR setup, required Mac setup step on iPhone, remote helper, or push relay. Current beta deployment limitations are listed in the release notes.

## Apple GPU and MPS

An Apple silicon Mac running the menu-bar app adds itself automatically. It reads the whole Apple GPU through local driver counters, without SSH or a privileged helper. Remote Apple silicon Macs can also be queried over direct SSH. To view your local Mac from iPhone/iPad, configure an SSH address and credentials for that Mac and enable Remote Login yourself if desired; the app never enables it for you.

GPU utilization includes Metal/MPS and other GPU users, including the desktop compositor. **Unified memory** means GPU driver allocation divided by the Mac's total physical memory. This is not dedicated VRAM or the memory used by one PyTorch model. The driver counter schema can change in a future macOS release; missing counters produce an error, never a fabricated zero.

On macOS the process picker lists the connected user's processes. It cannot identify which of them uses MPS. Search by name or PID and select the process you want to watch. No remote agent or training-code instrumentation is needed.

## Monitor

The dashboard uses the original compact rows, small usage bars and recent-history charts, with clear stale-data states. A GPU is available when both values are below your thresholds (default: compute 10%, VRAM 20%). Unreachable servers remain visible and can be disabled.

Use the server filter at the top of the dashboard (or in Settings) to choose which servers appear. The GPU count, Mac menu-bar summary and VS Code view follow that selection; the Watch follows the iPhone selection. Hidden servers continue to be queried and can still generate notifications. Select **Show all servers** to include newly discovered servers automatically. CLI host arguments remain independent of the display filter. Selection is saved locally; cross-device sync has the beta limitation above.

The awake Mac polls at your chosen interval while its menu-bar app runs. iPhone and iPad poll while visible; background refresh happens only when the operating system permits it. Apple Watch displays the latest phone snapshot and marks old results for rechecking. Background timing and notifications are not guaranteed. Job completion means the selected process disappeared, not that it succeeded.

In Settings → **Available GPU Alerts (빈 GPU 알림)**, turn alerts on to reveal their conditions. Availability alerts wait for repeated successful readings over a configurable duration (30 seconds by default). A failed query or a long observation gap restarts that duration. After an alert, another alert requires a busy-to-available transition and the configured cooldown (10 minutes by default). **Notify once** turns availability alerts off after the next notification is scheduled; enabling them again starts a fresh observation period.

Expand **Add from Running Tasks (실행 중인 작업에서 추가)** beside the server to find a process by name or PID. Give watched processes a recognizable name, such as an experiment name. Use the task’s More menu to rename or stop watching it; the five most recent history entries stay visible, with older entries expandable. The latest 100 process-end detections remain in job history after a watch is removed. Timestamps show when disappearance was observed, not the exact exit time or a successful result.

Star servers to show them first, using the star in the iPhone/iPad server detail or the server’s More menu in **Server Management (서버 관리)**. Choose **Representative Connection (대표 연결 선택)** in that menu to group connections. A single selection sheet shows matching suggestions; changes apply when you save. Grouping changes presentation and avoids duplicate availability alerts; it preserves the original connections and process watches. Separate them again at any time. The VS Code dashboard uses the same ordering and groups.

Connection failures show a short explanation, retry and server-settings actions, and expandable diagnostic details. Previously collected readings remain visibly stale until a successful refresh.

## Widgets and complications

On iPhone or iPad, add **GPU Usage** from the Home Screen widget gallery after opening the app once. Small and medium widgets show GPU availability, how many GPUs have current readings, and the last observation time (including the date for older readings); accessory widgets are also available. On Apple Watch, add the GPU Usage circular, rectangular or inline complication to a compatible watch face after opening both companion apps.

Widgets and complications use the latest device snapshot, without making SSH connections themselves. They follow the app's server selection and mark expired readings for rechecking instead of showing them as currently available. Tap to open the app and request fresh data. Widget and complication refresh timing is controlled by the operating system.

Viewing and settings sync are free. Pro adds local GPU-available and process-completion notifications and the CLI, at USD **1.99/month** or **19.99/year**, with Apple-localized storefront pricing. TestFlight purchases use Apple's test environment. Public subscriptions require App Review before sale.

```sh
gpu-usage --json
gpu-usage --available=10,20 --json --watch=10
gpu-usage --tsv research-server
```

JSON watch mode emits one snapshot per line. Exit status: 0 success, 2 invalid input or connection failure, 3 Pro required. Credentials are never included in the output. JSON adds `backend: "appleMetal"` for Apple GPUs. For compatibility, the TSV columns still use `vram_*` names; for an Apple GPU these carry the unified-memory driver allocation described above, in GiB.

## Interface preview

These captures use sample servers and GPU data.

![GPU Usage in light mode](dashboard-light.png)
![GPU Usage in dark mode](dashboard-dark.png)

![iPad GPU dashboard](ipad-dashboard.png)
![Apple Watch GPU dashboard](watch-dashboard.png)

![GPU Usage Home Screen widget](widget-medium.png)
