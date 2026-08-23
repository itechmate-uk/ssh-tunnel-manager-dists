# Notifications And Local File Bridge

## Notifications

Healthy tunneled applications may expose a generic event endpoint. Tunnel
Manager polls it and can show OS notifications for new info, warning, error,
permission, completion, and lifecycle events.

Notification delivery state is machine-local. Supporting event sources assign
and acknowledge transient events so another machine does not replay them.
Old events are silently baselined or discarded rather than notifying long
after they occurred.

Sound is enabled by default and can be disabled globally or overridden per
tunnel. A local custom WAV may be selected; a missing/unplayable custom file
falls back to Tunnel Manager's default sound.

## Local File Bridge

The Local File Bridge lets an approved tunneled browser application request a
local editor for a remote file without exposing arbitrary local filesystem or
command access.

- New applications require a foreground pairing approval.
- File access is limited to configured remote roots and canonical paths.
- Executable/editor paths remain local to Tunnel Manager and are never sent to
  the web application.
- The first edit in a browser session downloads current remote content.
- Repeated edits in that session reopen the same watched local working copy.
- A new browser session downloads a fresh copy.
- Conflicting remote changes require explicit overwrite approval; recovery
  copies are preserved when configured.
- Persisted grants are scoped to installation, application origin, SSH
  backend, tunnel, provider, and exact file. Any mismatch asks again.

Bridge working files and permissions are machine-local, not stored beside a
portable executable or in a shared tunnel configuration.
