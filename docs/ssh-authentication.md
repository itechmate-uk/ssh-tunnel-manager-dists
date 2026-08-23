# SSH Authentication And Managed Keys

## Authentication Order

Tunnel Manager first uses the current user's normal system OpenSSH setup:

- `~/.ssh/config` or the Windows OpenSSH equivalent;
- private keys and certificates;
- `ssh-agent`;
- `known_hosts`;
- `ProxyJump` and enterprise policy.

If OpenSSH reports that authentication is required, the desktop manager asks
for a password and retries. Connection refusal, DNS failure, or a stopped SSH
server does not trigger a misleading password prompt.

## Password Retention

- **One attempt:** use the password once, then discard it.
- **Current session:** retain only in Tunnel Manager process memory. This is
  the default.
- **OS credential vault:** persist only after successful authentication using
  Windows Credential Manager, macOS Keychain, or Linux Secret Service.

There is no plaintext-file fallback. Passwords never belong in `config.toml`,
command arguments, logs, or support bundles.

## Managed Key

After a successful password login, Tunnel Manager can offer to create a
dedicated Ed25519 key for the profile. It installs only the public key on the
remote account and verifies key-only login before changing the profile.

The key belongs to the current local OS user and profile. Tunnel Manager does
not overwrite default SSH keys, disable password login, edit `sshd_config`, or
change the user's OpenSSH config automatically.

The Authentication panel can show a profile-specific OpenSSH stanza for using
the managed key outside Tunnel Manager. If a matching `Host` block already
exists, add or update only `IdentityFile` and `IdentitiesOnly`; do not create a
conflicting duplicate block.

## Shared Configurations

A shared/mounted tunnel TOML contains only portable tunnel definitions. Every
computer keeps authentication choices and managed private-key paths in its own
application-data directory. OS-vault secrets remain local to that computer.
