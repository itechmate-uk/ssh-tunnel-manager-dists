# Tunnel Manager

Tunnel Manager is a free desktop utility for running secure OpenSSH local
port forwards without repeatedly typing long SSH commands. It groups tunnels
by SSH backend, monitors connection and application health, can manage an
optional remote service, and keeps frequently used browser apps and private
development services available from one tray application.

[Download the latest release](https://github.com/helal00/ssh-tunnel-manager-dists/releases)

> **Unsigned preview builds:** current downloadable binaries are not yet
> signed by Microsoft, Apple, or a Linux package repository. Windows
> SmartScreen and equivalent platform protections may therefore warn before
> first launch. Verify the release checksum and repository URL before using a
> platform-specific **Run anyway** or **Open** action. Never bypass a warning
> for an unverified download. See [Unsigned Build Safety](docs/unsigned-builds.md).

## What It Does

- Starts, stops, restarts, and reconnects system OpenSSH `-L` tunnels.
- Groups multiple tunneled services under one `user@host` SSH profile.
- Supports existing OpenSSH keys, `ssh-agent`, `ssh_config`, `ProxyJump`, and
  password authentication without storing plaintext credentials.
- Optionally keeps a password for the current session or in the operating
  system credential vault.
- Can create and install a dedicated per-profile SSH key after explicit user
  approval.
- Monitors TCP or HTTP health and reports SSH, remote-service, tunnel, and app
  status separately.
- Can observe or control a configured remote user service before opening its
  local app.
- Opens healthy tunneled browser applications and keeps manager-owned windows
  synchronized across reconnect and computer resume.
- Delivers application notifications and supports a permission-based Local
  File Bridge for editing approved remote files locally.
- Imports safe text-only TOML tunnel bundles supplied by compatible apps.
- Stores configuration and machine-specific state in the current user's
  application-data directory, so the program folder can move safely.

## Quick Start

1. Download the archive for your operating system from
   [GitHub Releases](https://github.com/helal00/ssh-tunnel-manager-dists/releases).
2. Verify it against the release `SHA256SUMS.txt`.
3. Extract it to a per-user application folder.
4. On Windows, run `tunnelm.exe`. Use `tunnelm-cli.exe` for console diagnostics.
5. Add an SSH profile, then add or import a tunnel for that profile.
6. Start the tunnel. Tunnel Manager uses normal OpenSSH keys first and asks for
   a password only when the SSH client reports that authentication is needed.

Windows currently provides the complete manager, tray, WebView, notifications,
authentication, remote-service, and Local File Bridge experience. Linux and
macOS archives currently provide the command interface only; native desktop
parity is planned and must not be inferred from the availability of an archive.

## Documentation

- [Installation And Updates](docs/installation.md)
- [Getting Started](docs/getting-started.md)
- [SSH Authentication And Managed Keys](docs/ssh-authentication.md)
- [Tunnel Imports And Remote Services](docs/remote-services.md)
- [Notifications And Local File Bridge](docs/notifications-and-file-bridge.md)
- [Platform Support](docs/platform-support.md)
- [Unsigned Build Safety](docs/unsigned-builds.md)
- [Troubleshooting](docs/troubleshooting.md)
- [Security Policy](SECURITY.md)

## Downloads And Verification

Each GitHub Release contains portable archives plus `SHA256SUMS.txt`. Newer
releases also include a source-bound release manifest and per-package SBOMs.
The checked-in [`packages/v0.1.1`](packages/v0.1.1) directory is an older
reproducibility mirror; use GitHub Releases for current downloads.

Linux/macOS:

```sh
sha256sum -c SHA256SUMS.txt --ignore-missing
```

Windows PowerShell:

```powershell
Get-FileHash .\tunnelm_*.zip -Algorithm SHA256
```

Compare the printed hash with the matching line in `SHA256SUMS.txt`.

## Privacy Boundary

Published archives contain program files, licensing/security information, and
inert generic examples. They contain no credentials, private keys, server
addresses, usernames, local development paths, runtime configuration, logs,
bridge files, or notification state.

Tunnel Manager stores normal configuration and machine-specific state in the
current operating-system user's application-data directories. Updating or
moving a portable binary does not replace that data.

## Source And License

Tunnel Manager is free and open-source software. The source repository is:

https://github.com/helal00/ssh-tunnel-manager

Licensed under Apache-2.0.
