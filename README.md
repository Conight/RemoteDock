# RemoteDock

RemoteDock is a native macOS remote file manager built with SwiftUI and SwiftData. It brings SFTP, WebDAV, object storage, and popular cloud drives into one desktop interface for browsing, transferring, previewing, editing, and synchronizing remote files.

> RemoteDock is still in early development. APIs, data models, and protocol capabilities may change as the project evolves. Before publishing the repository, update the release notes to match your actual release plan.

## Features

- Native macOS windowing, sidebar navigation, menu commands, and Quick Look previews
- Bookmark management, connection history, Quick Connect, and path navigation
- Remote directory browsing with list and outline views
- Uploads, downloads, folder transfers, configurable parallel transfers, and transfer history
- Create, rename, move, copy, and delete remote files or folders
- Download and open files, download files for editing, and upload edited copies back to the remote
- Remote-to-local, local-to-remote, and mirror synchronization plans
- Recursive search, directory size calculation, general file information, and custom metadata for selected protocols
- Temporary download URLs for S3, Google Cloud Storage, Azure Blob Storage, and Backblaze B2
- Provider-managed download URLs and shared links for OneDrive, Google Drive, Dropbox, and Box
- RemoteDock bookmark import/export and Cyberduck `.duck` bookmark import/export
- Passwords, OAuth tokens, access keys, and other secrets stored in the macOS Keychain

## Supported Connections

The New Connection screen currently exposes these implemented drivers:

| Type | Authentication | Notes |
| --- | --- | --- |
| SFTP | Password, private key | Uses SwiftNIO SSH; encrypted private-key passphrases still have limitations |
| WebDAV / WebDAV HTTPS | Password, anonymous | Nextcloud reuses the WebDAV implementation |
| Nextcloud | Password | Supports Nextcloud URL parsing |
| Amazon S3 | Access key | Supports bucket/prefix paths |
| Google Cloud Storage | Access key | Supports bucket/prefix paths |
| Azure Blob Storage | Access key | Supports container/prefix paths |
| Backblaze B2 | Access key | Supports bucket/prefix paths |
| OneDrive | OAuth | Requires an OAuth Client ID |
| Google Drive | OAuth | Requires an OAuth Client ID |
| Dropbox | OAuth | Requires an OAuth Client ID |
| Box | OAuth | Requires an OAuth Client ID/Secret |
| Local Disk | macOS folder permission | Useful for local browsing and workflow testing |

The codebase also keeps protocol enums and Cyberduck mappings for FTP, FTPS, SMB, OpenStack Swift, DRACOON, and other legacy surfaces, but those protocols are not currently shown in the New Connection list and still need driver implementations.

## Requirements

- macOS; the current Xcode project deployment target is `macOS 26.5`
- A full Xcode installation with support for Swift 6, SwiftUI, and SwiftData
- Swift Package dependencies resolved automatically by Xcode

External Swift Package dependencies:

- `swift-nio`
- `swift-nio-ssh`
- `swift-crypto`

## Quick Start

Clone the repository:

```bash
git clone https://github.com/<your-org>/RemoteDock.git
cd RemoteDock
```

Open the project in Xcode:

```bash
open RemoteDock.xcodeproj
```

Select the `RemoteDock` scheme in Xcode, wait for Swift Package resolution to finish, then run the macOS app target.

For command-line builds or tests, make sure `xcode-select` points to a full Xcode installation instead of Command Line Tools:

```bash
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
xcodebuild test -project RemoteDock.xcodeproj -scheme RemoteDock -destination 'platform=macOS'
```

## OAuth Configuration

The open source version does not ship with third-party cloud OAuth credentials. The relevant Info.plist keys live in `RemoteDock/Resources/Configuration/RemoteDockInfo.plist`:

- `RemoteDockGoogleOAuthClientID`
- `RemoteDockOneDriveOAuthClientID`
- `RemoteDockDropboxOAuthClientID`
- `RemoteDockBoxOAuthClientID`
- `RemoteDockBoxOAuthClientSecret`

The OAuth callback scheme is:

```text
remotedock://
```

To enable the full sign-in flow for cloud-drive protocols, create an app in the relevant provider developer console and inject the Client ID or Client Secret into your local build configuration. Do not commit personal or production credentials to a public repository.

## Project Structure

```text
RemoteDock/
  App/                         App entry point, window lifecycle, and menu commands
  Domain/                      Protocol, authentication, transfer, and bookmark models
  Features/
    Browser/                   Main browser UI, file actions, dialogs, and toolbar
    Connections/               New/edit connection UI
    Settings/                  Settings UI
  Infrastructure/
    Providers/                 SFTP, WebDAV, object storage, and cloud-drive drivers
    Security/                  Keychain and OAuth support
  Resources/
    Assets/                    App icon and color assets
    Configuration/             Entitlements and Info.plist
  Services/                    Connection, transfer, sync, bookmark, and preview services
RemoteDockTests/               Unit tests
RemoteDockUITests/             UI tests
```

## Security Notes

- RemoteDock bookmark exports do not include passwords, tokens, secrets, or Keychain references.
- Bookmark import rejects RemoteDock/Cyberduck files that contain sensitive fields.
- Local folders, private keys, and known_hosts files use macOS security-scoped bookmarks for permission management.
- When opening public issues, do not paste access keys, OAuth tokens, credential-bearing URLs, or private file paths.

## Contributing

Issues and pull requests are welcome. Good first areas for contribution include:

- Implementing protocol drivers that are currently modeled but unavailable
- Adding real-service integration tests or mock-server coverage
- Improving OAuth setup documentation for each provider console
- Improving sync conflict handling, transfer recovery, and large-directory performance
- Adding GitHub Actions CI, release scripts, and signing/notarization workflows

Before submitting a PR, run:

```bash
xcodebuild test -project RemoteDock.xcodeproj -scheme RemoteDock -destination 'platform=macOS'
```

## License

RemoteDock is licensed under the GNU Affero General Public License v3.0. See [LICENSE](LICENSE) for the full license text.

Copyright © 2026 Conight.
