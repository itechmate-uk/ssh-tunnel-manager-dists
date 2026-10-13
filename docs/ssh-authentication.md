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
not overwrite default SSH keys, disable password login, or edit `sshd_config`.
It changes your OpenSSH config only if you choose **Auth... > Add managed key to
system SSH config** for the profile.

To use the managed key outside Tunnel Manager:
- **Terminal button:** opens the profile in a terminal, already signed in.
- **Auth... > Add managed key to system SSH config:** lets `ssh`, `scp` and VS
  Code use the key.
- **By hand:** the Authentication panel shows a profile-specific OpenSSH stanza.
  If a matching `Host` block already exists, add or update only `IdentityFile` and
  `IdentitiesOnly`; do not create a conflicting duplicate block.

See [Opening A Terminal And Using Managed Keys Everywhere](terminal-and-ssh-config.md).

## Tunnel-Only Accounts (Key Handoff)

Some accounts may only open tunnels and have no shell over SSH, for example a
supervised AgentW account. Tunnel Manager cannot install a key on such an
account by itself. The application behind the tunnel tells Tunnel Manager
what its session supports:

- If the application **offers a key handoff**, managed-key setup works as
  usual. Tunnel Manager creates the key on this computer and hands only the
  public key to the application through the tunnel. The application installs
  it, limited to tunnels, and Tunnel Manager verifies key-only login before
  switching the profile. The private key never leaves this computer. If the
  application says the key waits for approval, the profile keeps its password
  until you run key setup again after approval.
- If it does **not**, Tunnel Manager does not offer a dedicated key for that
  profile, and the Authentication panel shows the application's reason.

Removing the dedicated key from the Authentication panel asks the application
to remove it too.

## Shared Configurations

A shared/mounted tunnel TOML contains only portable tunnel definitions. Every
computer keeps authentication choices and managed private-key paths in its own
application-data directory. OS-vault secrets remain local to that computer.
