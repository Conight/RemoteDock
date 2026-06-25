# RemoteDock

RemoteDock is a native macOS remote file manager for SFTP, WebDAV, object storage, and cloud drives.

It provides a desktop interface for browsing, transferring, previewing, editing, sharing, and synchronizing files across remote storage providers.

## Download RemoteDock

RemoteDock public builds are available now from GitHub Releases.

**[Download RemoteDock](https://github.com/conight/RemoteDock/releases)**

Install the app:

1. Download the latest `.dmg` release.
2. Double-click the disk image to mount it.
3. In the DMG Finder window, drag `RemoteDock.app` onto the `Applications` shortcut.
4. Open RemoteDock from Finder or Launchpad.

If macOS reports that `RemoteDock.app` is damaged or cannot be opened, see [First Launch on macOS](#first-launch-on-macos).

## Project Status

RemoteDock is an early-stage vibe coding project. Public builds are available for testing and feedback, but the source code will be opened after the codebase is stable enough to maintain as a public project.

APIs, data models, protocol behavior, and packaging may change between early releases.

## Release Packaging

Create a drag-to-Applications disk image:

```bash
Scripts/create-dmg.sh
```

The script builds a Release archive, creates a drag-to-Applications DMG containing `RemoteDock.app` and an `Applications` shortcut, and applies the Finder window layout users see after mounting the disk image.

For public distribution outside the Mac App Store, use a Developer ID signed build and notarize the final DMG with Apple before publishing it.

## First Launch on macOS

Early builds may not be notarized. If macOS blocks the app after installation, the app bundle may still have Apple's quarantine attribute attached.

Only run the following command for a build you downloaded from a source you trust:

```bash
xattr -dr com.apple.quarantine /Applications/RemoteDock.app
```

Then open the app again:

```bash
open /Applications/RemoteDock.app
```

This command removes Gatekeeper quarantine metadata from the app bundle. It does not sign, notarize, or verify the app. If Terminal reports a permission error, run the same command with `sudo`.

## Requirements

- macOS 26.5 or later for current builds
- A Mac with network access to the remote services you want to connect to
- Your own credentials for each remote server or cloud provider

## Features

- Native macOS interface with sidebar navigation, menu commands, toolbar actions, and Quick Look previews
- Bookmark management, connection history, Quick Connect, and path navigation
- Remote directory browsing with list and outline views
- Uploads, downloads, folder transfers, configurable parallel transfers, and transfer history
- Create, rename, move, copy, and delete remote files or folders
- Download files for local editing and upload edited copies back to the remote
- Remote-to-local, local-to-remote, and mirror synchronization workflows
- Recursive search, directory size calculation, file information, and selected custom metadata support
- Temporary download URLs for supported object storage providers
- Provider-managed download URLs and shared links for supported cloud drives
- RemoteDock bookmark import/export and Cyberduck `.duck` bookmark import/export
- Passwords, OAuth tokens, access keys, and other secrets stored in the macOS Keychain

## Supported Connections

| Type | Authentication | Notes |
| --- | --- | --- |
| SFTP | Password, private key | Uses SwiftNIO SSH; encrypted private-key passphrases still have limitations |
| WebDAV / WebDAV HTTPS | Password, anonymous | Nextcloud reuses the WebDAV implementation |
| Nextcloud | Password | Supports Nextcloud URL parsing |
| Amazon S3 | Access key | Supports bucket/prefix paths |
| Google Cloud Storage | Access key | Supports bucket/prefix paths |
| Azure Blob Storage | Access key | Supports container/prefix paths |
| Backblaze B2 | Access key | Supports bucket/prefix paths |
| OneDrive | OAuth | Requires your own OAuth Client ID |
| Google Drive | OAuth | Requires your own OAuth Client ID |
| Dropbox | OAuth | Requires your own OAuth Client ID |
| Box | OAuth | Requires your own OAuth Client ID and Client Secret |
| Local Disk | macOS folder permission | Useful for local browsing and workflow testing |

FTP, FTPS, SMB, OpenStack Swift, DRACOON, and other legacy surfaces are represented in parts of the internal model, but they are not currently available in the New Connection UI.

## OAuth Setup

RemoteDock does not ship with shared OAuth credentials for third-party cloud providers.

For Google Drive, OneDrive, Dropbox, and Box:

1. Create an app in the provider's developer console.
2. Configure the app to allow the RemoteDock callback scheme where required:

```text
remotedock://
```

3. Open RemoteDock.
4. Create a new connection for the provider.
5. Paste your OAuth Client ID or Client Secret into the fields shown in the New Connection UI.

Do not share personal or production OAuth credentials in public issues, screenshots, or support requests.

## Security Notes

- RemoteDock bookmark exports do not include passwords, tokens, secrets, or Keychain references.
- Bookmark import rejects RemoteDock/Cyberduck files that contain sensitive fields.
- Local folders, private keys, and known_hosts files use macOS security-scoped bookmarks for permission management.
- Credentials are stored through the macOS Keychain.
- Public issues should not include access keys, OAuth tokens, credential-bearing URLs, or private file paths.

## Source Availability

The source code is planned to be opened after the codebase stabilizes. Until then, GitHub Releases are the primary distribution channel for public builds.

Feedback, bug reports, and compatibility notes are welcome through GitHub Issues.

## License

RemoteDock is licensed under the GNU Affero General Public License v3.0. See [LICENSE](LICENSE) for the full license text.

Copyright © 2026 Conight.
