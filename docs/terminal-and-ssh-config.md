# Opening A Terminal And Using Managed Keys Everywhere

Since v0.1.27.

## Open Terminal

Each profile has a **Terminal** button in the manager window, next to
**Auth...**. The profile's tray menu also has **Open Terminal**. It opens the
profile's SSH login in a terminal, already signed in where possible:

| Profile signs in with | The terminal |
|---|---|
| Tunnel Manager's key (managed key) | Signs in without a password. |
| A password or key passphrase Tunnel Manager holds (for this session, or saved in the OS credential vault) | Tunnel Manager supplies it once. It is never shown or put on a command line. |
| A password Tunnel Manager does not hold | ssh asks for it in the terminal. |
| Your own OpenSSH keys | Uses them, as any terminal does. |

**When ssh asks something else**, such as whether to trust a new server's host key
or for a verification code, the question appears in the terminal for you to answer.

**When the connection fails**, the window stays open until you press Enter, so you
can read the message.

**Logins without a shell:** some logins only carry tunnels and have no shell, for
example a supervised AgentW account. For those, the button explains that a
terminal cannot open.

### Choosing the terminal

Choose it in **SSH Settings > Terminal**. The choice applies to this computer only.

- **Automatic:** on Windows, Windows Terminal if it is installed, else PowerShell.
  On macOS, Terminal. On Linux, the default terminal, then GNOME Terminal,
  Konsole, Xfce Terminal or xterm.
- **Windows:** Windows Terminal, PowerShell window, Command Prompt window, PuTTY.
- **macOS:** Terminal, iTerm, PuTTY.
- **Linux:** default terminal, GNOME Terminal, Konsole, xterm, PuTTY.
- **Custom command:** any terminal. Put `{command}` where the SSH session goes,
  and optionally `{title}` for the window title. For example:
  `alacritty -T {title} -e {command}`.

### PuTTY

**Finding PuTTY:** set the location of `putty.exe` in SSH Settings > Terminal.
Left blank, Tunnel Manager looks on PATH and in `Program Files\PuTTY`.

**How PuTTY signs in:**
- **Profiles with a managed key:** PuTTY cannot read OpenSSH key files, so Tunnel
  Manager makes a PuTTY copy of the key next to it (`...\.ssh\tunnel-manager\
  PROFILE_ed25519.ppk`). It updates the copy when the key changes, and deletes it
  when you remove the key.
- **Password profiles:** PuTTY asks for the password itself. Tunnel Manager does
  not pass it, because PuTTY could only take it on its command line, where other
  programs on the computer can read it.

**Not supported with PuTTY:**
- **Profiles through a jump host:** choose another terminal for those.
- **A managed key with a passphrase:** it gets no PuTTY copy. Choose another
  terminal, or convert the key with PuTTYgen.

## Add Profiles To SSH Config

**What it does:** your own terminals, `scp`, VS Code Remote-SSH and WinSCP don't
know about Tunnel Manager's keys, so they ask for a password. Turn on **SSH Settings
> Terminal > Add profiles to SSH config** and they use the keys too:
- `ssh proxmox` connects with the profile's server, user, port, jump host and key.
  The name is the profile name in lower case; SSH Settings shows the names after
  you save.
- `ssh root@203.0.113.10` (the same server and user) also uses the key.

**What it changes on your computer:**
- Tunnel Manager writes its entries to its own file, `.ssh\tunnel-manager\
  ssh_config`.
- It adds two lines at the top of your `.ssh\config`: a comment, and `Include
  tunnel-manager/ssh_config`.
- The first time it changes an existing `.ssh\config`, it keeps a copy as
  `.ssh\config.tunnel-manager-backup`.
- Nothing else in your file changes. If your own config already uses a name,
  Tunnel Manager picks another (`proxmox-tm`).

**It follows your profiles:**
- Setting up a key adds the profile.
- Removing the key, or switching the profile to a password, removes it.
- Renaming or deleting the profile renames or removes its entry.
- Replacing the key needs no change.

Don't edit Tunnel Manager's file; it is rewritten. Keep your own entries in
`.ssh\config`.

**Turning it off** removes the two lines and Tunnel Manager's file, so your
`.ssh\config` is back as it was.
