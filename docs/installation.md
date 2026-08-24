# Installation And Updates

## Download

Download only from the public
[Tunnel Manager releases](https://github.com/itechmate-uk/ssh-tunnel-manager-dists/releases).
Select the archive matching your operating system and architecture, and
download `SHA256SUMS.txt` from the same release.

Current releases are portable. They do not require administrator access or a
fixed installation directory.

## Verify

Linux/macOS:

```sh
sha256sum -c SHA256SUMS.txt --ignore-missing
```

Windows PowerShell:

```powershell
Get-FileHash .\tunnelm_*.zip -Algorithm SHA256
```

Compare the resulting archive hash with its `SHA256SUMS.txt` entry. Current
releases also provide source-bound `manifest.json` with platform, archive size,
and SHA-256 metadata.

## Windows

1. Extract the ZIP to a normal per-user folder such as
   `C:\Users\alice\Apps\TunnelManager`.
2. Run `tunnelm.exe`.
3. Keep the companion executables beside it. `tunnelm-cli.exe` provides
   PowerShell/CMD diagnostics.
4. If SmartScreen warns, first verify the checksum and download origin, then
   select **More info > Run anyway**. See [Unsigned Build Safety](unsigned-builds.md).

Normal configuration is stored in `%APPDATA%\TunnelManager\config.toml`.
Moving or replacing the executable does not remove configuration.

## Linux And macOS

Extract the matching archive and ensure the executable bit is present:

```sh
tar -xzf tunnelm_*.tar.gz
chmod +x tunnelm
./tunnelm help
```

On macOS, if Gatekeeper blocks the verified unsigned file, use Finder's
**Control-click > Open** flow or the corresponding Privacy & Security approval.
Do not disable Gatekeeper globally.

The current Linux and macOS archives are command-only preview builds. They do
not yet contain the native manager/tray desktop experience available on
Windows.

## Updating

The Windows manager shows update status between Autostart and About and in the
tray. It downloads the matching platform archive, verifies it against
`manifest.json`, stages it locally, stops manager-owned tunnels, replaces only
installed files whose SHA-256 changed, and restarts automatically.

Versions through `v0.1.2` predate this updater and require one manual upgrade to
`v0.1.5` or newer:

1. Quit Tunnel Manager from its tray so manager-owned tunnels stop.
2. Download and verify the new archive.
3. Extract it to a new folder or replace program files while Tunnel Manager is
   not running.
4. Start it and check `tunnelm version --json` and package information.

The same manual procedure remains the fallback if the portable package folder
is read-only or a verified platform archive is unavailable.

Configuration, device authentication choices, keyring secrets, bridge state,
and notification state live under the current user's OS application-data
directory and are not overwritten by a portable update.

## First-Run Bootstrap

If the application-data configuration does not exist, a valid `config.toml`
beside the executable may seed it on the first run. Later launches use the
application-data copy. Release archives intentionally contain no active
configuration or credentials.
