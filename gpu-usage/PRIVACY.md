# GPU Usage Privacy Policy

Effective October 9, 2026. This policy covers GPU Usage for Mac, iPhone, iPad and Apple Watch, the GPU Usage VS Code extension, and the GPU Usage CLI.

## Your servers and readings

GPU Usage connects directly to SSH servers you select. It reads NVIDIA GPU usage or whole-device Apple GPU utilization and driver memory allocation and, when you enable job monitoring, process identifiers, names and start identities. The app does not install a service on the server, change your jobs, or send those readings to a developer-operated backend.

The Mac app discovers explicit entries in your SSH configuration. You can disable or delete a server in Settings. Deleting a server removes its synced connection and watches; it does not change your SSH configuration or remote files. Removing a server also requests deletion of its app-managed Keychain credential when no other connection uses it. Credentials managed separately by your SSH configuration remain yours.

The local Mac is discovered automatically and queried locally. For Apple GPU monitoring, process selection lists the connected user's processes; it does not attribute GPU use to a specific process.

## Apple Watch

The paired iPhone sends recent GPU readings, server display names, selected job names and monitoring status to the Watch using Apple WatchConnectivity. SSH addresses, passwords and keys are not sent to the Watch. The latest snapshot is cached on the Watch for offline viewing, with stale readings marked. The Watch does not contact GPU servers or a developer-operated backend.

## iCloud and Keychain

Connections, trusted host keys, monitoring preferences, selected process watches and an Apple-signed subscription proof are stored in your private CloudKit database using encrypted record fields. The first host key is stored automatically and later key changes require confirmation. SSH passwords and supported private keys use Apple's synchronizable Keychain. iCloud and Keychain synchronization are provided by Apple under your Apple Account. GPU Usage does not run a separate account, credential store, or synchronization server.

Settings are also stored locally. On Mac the settings file and current snapshot have owner-only file permissions. The VS Code extension reads local settings and snapshots; it does not read SSH credentials. Configuration recovery copies may remain on the device after a damaged file or iCloud account change. They contain settings, not private keys or passwords.

## Notifications and purchases

Notifications are created on the device from successful observations. There is no developer-operated push relay. iOS/iPadOS determine when background checks are allowed. Notification mirroring to the Watch follows Apple's device and notification settings. Notification text may include the server name and selected process name; control lock-screen previews in system settings.

Apple processes subscriptions and payment details. GPU Usage receives a signed transaction proof to check access to Pro features; payment card details are not provided to the app. The proof is synchronized to your other devices through your private iCloud database.

## Analytics and controls

GPU Usage includes no advertising SDK or third-party analytics SDK and does not track you across other apps. Apple may provide app diagnostics or purchase reports according to your Apple settings and its policies. GitHub hosts the Mac and VS Code downloads and applies its own privacy policy to visits and downloads.

You can remove connected servers in the app, revoke SSH credentials at the server, disable notifications in system settings, cancel the subscription in your Apple account settings, and manage iCloud/Keychain data using Apple's account controls. Deleting the app from one device does not by itself remove data stored in iCloud or on your other devices.

## Contact

For support and privacy inquiries, use [GPU Usage support](https://github.com/saankim/myapps-releases/issues). Do not post passwords, private keys, or confidential server details in a public issue.
