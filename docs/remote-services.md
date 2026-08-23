# Tunnel Imports And Remote Services

## Safe Import Bundles

Applications may provide a versioned `tunnel-manager-tunnel` TOML file. A
bundle can describe:

- canonical `backend_id = "user@host"`;
- remote service host and port;
- suggested local port and optional fallback range;
- TCP or HTTP health check;
- browser launch and application identity;
- event and Local File Bridge endpoints; and
- an optional typed remote-service unit.

Bundles are declarative text. They cannot contain passwords, passphrases,
private keys, arbitrary SSH options, local executable launches, or local
actions. Tunnel Manager previews and validates imported values before saving.

## Remote Service Management

A tunnel may optionally monitor or control the server-side application it
forwards to. Supported policy is configured by the user; a bundle may suggest
a typed unit but cannot silently execute arbitrary commands.

`systemd-user` is the preferred provider. A user service can normally be
started without remote `sudo`. System services require independently
configured non-interactive authorization. Advanced command providers store an
executable and argument list separately and remain user-configured.

The selected tunnel's **Service** panel provides Status, Start, Stop, and
Restart. Remote service success is not enough by itself: local TCP/HTTP health
through the tunnel remains the final authority before the app is opened.

By default, stopping a tunnel does not stop its remote service. This avoids
interrupting other users or processes. An advanced stop policy must be enabled
explicitly when service ownership is exclusive.
