# Getting Started

## 1. Create An SSH Profile

A profile represents one canonical SSH backend such as `alice@example.com`.
Its display name is only a label. All tunnels for that SSH account and host are
grouped under the same profile.

Tunnel Manager uses the system OpenSSH client, so existing host aliases,
keys, agents, `known_hosts`, and `ProxyJump` rules continue to work.

## 2. Add Or Import A Tunnel

Add a tunnel manually when you know the remote host/port and desired local
port. Compatible applications may instead provide a text-only TOML import
bundle containing the endpoint, suggested local port, health check, browser
launch, event endpoint, and optional remote-service information.

Imported local ports are suggestions. Tunnel Manager assigns another allowed
port when the suggestion is already occupied and the bundle defines a fallback
range. It never adopts a different backend's listener merely because both apps
look similar.

## 3. Start

Select the tunnel and choose **Start**. Tunnel Manager:

1. checks local-port ownership;
2. tries normal OpenSSH key/config authentication;
3. asks for a password only when authentication is required;
4. optionally checks or starts the configured remote service;
5. establishes the local forward;
6. verifies TCP or HTTP application health; and
7. enables **Open App** only when the app is usable.

A failed SSH connection is not reported as started. Stop/Restart/Open App stay
unavailable until Tunnel Manager owns a working tunnel.

## 4. Quit Correctly

Closing the manager window normally leaves Tunnel Manager in the tray. Choose
**Quit** from the tray menu to stop every manager-owned tunnel and app process.
External listeners are preserved unless you explicitly choose the verified
**Reuse Port** recovery action.

## Configuration Location

The default configuration is machine-local and user-local:

- Windows: `%APPDATA%\TunnelManager\config.toml`
- Linux: the current user's standard application config directory
- macOS: the current user's standard Application Support location

Use **Switch Config** only when intentionally selecting another TOML file.
Machine-specific credentials, managed key paths, notification state, and
bridge working files are never stored in a shared portable TOML.
