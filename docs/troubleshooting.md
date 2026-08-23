# Troubleshooting

## Tunnel Reports Started But The App Is Unavailable

Current builds distinguish SSH process startup from tunnel and application
health. Inspect the selected tunnel's message, SSH diagnostics, remote-service
state, and HTTP/TCP health. Open App should remain unavailable until health is
successful.

## Local Port Is In Use

Choose another local port. If Tunnel Manager identifies a matching external
OpenSSH `-L` listener, it may offer **Reuse Port** with the PID, executable,
command line, and confirmation. It refuses to terminate unrelated listeners.

## Password Is Requested Repeatedly

Use Profile Authentication to confirm retention mode. Session retention ends
when Tunnel Manager quits. OS-vault retention occurs only after a successful
SSH authentication and requires a working platform credential service.

## Remote Service Will Not Start

Check the provider and exact unit name in the Service panel. Prefer a
`systemd --user` service that can be controlled without `sudo`. Service status
does not replace the tunnel's TCP/HTTP health result.

## App Window Does Not Recover After Sleep

Wait for the tunnel and `/api/health` to recover. Tunnel Manager sends generic
restoration events to an existing managed window; it does not create or focus
a new window merely because the network recovered. Use **Open App** explicitly
if the previous app window was closed.

## Collect Diagnostic Information

Use the bounded read-only Info, Log, SSH Info, Status JSON, Package Info, and
Capabilities views. The CLI equivalents include:

```text
tunnelm version --json
tunnelm package-info --json
tunnelm capabilities --json
tunnelm status --config PATH --json
```

Remove passwords, private paths, hostnames, usernames, and server addresses
before sharing diagnostics publicly.
