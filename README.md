# Tunnel Manager Downloads

This repository is the public binary distribution channel for Tunnel Manager.
The current verified package set is:

- [Tunnel Manager v0.1.1](packages/v0.1.1)

Future tagged builds will also be attached to:

https://github.com/helal00/ssh-tunnel-manager-dists/releases

Tunnel Manager is free/open-source software. Source is maintained at:

https://github.com/helal00/ssh-tunnel-manager

## Packages

- Windows `amd64`: complete desktop/tray package in ZIP format.
- Linux `amd64` and `arm64`: portable basic command builds in `tar.gz` format.
- macOS `amd64` and `arm64`: portable basic command builds in `tar.gz` format.

Linux and macOS native desktop integration is not yet release-certified. Those
archives are labeled basic support until interactive platform QA and signing are
complete.

## Verify a download

Each versioned package directory and GitHub Release includes
`SHA256SUMS.txt`.

Linux/macOS:

```sh
sha256sum -c SHA256SUMS.txt --ignore-missing
```

Windows PowerShell:

```powershell
Get-FileHash .\tunnelm_*.zip -Algorithm SHA256
```

Compare the resulting hash with the matching line in `SHA256SUMS.txt`.

## Privacy boundary

Published archives contain binaries, the Apache-2.0 license, security and
installation notes, and inert generic configuration examples. They contain no
credentials, private keys, server addresses, usernames, local development
paths, runtime configuration, logs, bridge files, or notification state.

Tunnel Manager stores normal configuration and machine-specific state in the
current operating-system user's application-data directories. Updating a
portable binary does not replace that data.
