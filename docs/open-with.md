# Opening A Server In Your Programs (Open With)

Since v0.1.29.

Tunnel Manager does not have a file manager of its own. Instead, it opens the
profile's server in programs you already use, such as WinSCP, an rclone drive or
VS Code, and signs them in for you where it can.

## Open with ▾

The **Open with ▾** button is in each profile's row in the manager window, next
to **Terminal**.

**The menu:**
- **Configure programs for PROFILE…**, where you add, change, remove and order
  that profile's programs;
- the profile's programs, in the order you set, each opening that profile.

Each profile has its own list (since v0.1.34): a program added under one
profile does not appear under another. The lists belong to this computer only.
Before v0.1.34 one list served every profile; your first change copies it to
every profile, so remove the programs a profile does not need.

## Configure programs (Windows)

Since v0.1.32 **Configure programs…** opens a list of your programs, in the order
of the Open with menu. Select a row, then:
- **Change** (or double-click the row) to edit it;
- **Remove** to take it out of the list (you are asked first);
- **Move Up** / **Move Down** to change the menu order.

**Add Supported Program** offers the supported programs you have not added yet.
**Link a Program** adds any other program. Each button saves straight away;
**Close** just closes the list.

## Two kinds of program

| Kind | Sign-in |
|---|---|
| **Supported program** | Tunnel Manager signs it in with the profile's login: its own key (managed key), or the password it holds for this session or in the OS credential vault. |
| **Linked program** | Any other program. It gets the server's address and asks for the login itself. |

Tunnel Manager never puts a password on a command line, in an address, or in a
file the program keeps.

## Supported programs

| Program | Computers | With Tunnel Manager's key | With your system OpenSSH keys | With a saved password |
|---|---|---|---|---|
| **WinSCP** | Windows | Signs in with a PuTTY copy of the key (`.ppk`) | Signs in with a PuTTY copy of your key (since v0.1.32) | Signs in. The password goes through a private file that is deleted after 2 minutes |
| **rclone mount** | Windows, macOS, Linux | Signs in | Signs in with your key (since v0.1.32) | Signs in. The password goes to rclone directly, never to a file |
| **VS Code (Remote-SSH)** | Windows, macOS, Linux | Signs in, once the key is in the system SSH config (Auth...) | Signs in through your SSH config | Not supported: VS Code asks |
| **File manager** | Linux | Signs in, once the key is in the system SSH config (Auth...) | Signs in through your SSH config | The file manager asks |

**System OpenSSH keys:** for a profile set to "Use system OpenSSH config and
keys", Tunnel Manager asks your OpenSSH which server, user and key it would use
(`ssh -G`), so a `Host` name from your own `.ssh\config` works. WinSCP gets a
PuTTY copy of the first key without a passphrase, kept in
`.ssh\tunnel-manager\system-keys\`; nothing is written beside your key. A key
with a passphrase can't be used this way: WinSCP then asks for the password, and
Tunnel Manager says why.

Not yet supported:
- WinSCP and rclone mount with profiles that use a jump host; use the Terminal or
  VS Code for those.
- WinSCP with a key that has a passphrase.

### WinSCP

**Opens:** the server in WinSCP.

**Settings:**
- **Remote folder:** blank opens your home folder; `/` opens the server's root.
- **SFTP server command** (optional): for example `sudo /usr/lib/sftp-server`
  lets you work as root, if `sudo` needs no password on that server.

### rclone mount

**What it does:**
- Shows the server as a drive letter on Windows, or as a folder on macOS and
  Linux.
- Runs in a small terminal window. Keep it open while you use the files.
- Closing the window, or pressing Ctrl+C in it, disconnects the drive.

**Settings:**
- **Remote folder:** blank is your home folder; `/` is the server's root. You do
  not need an `alias` remote for the root folder as in Rclone Browser.
- **SFTP server command:** as for WinSCP.
- **Mount at:**
  - Windows: a drive letter such as `X:`, or blank for the next free one.
  - macOS and Linux: an absolute folder, or blank for
    `~/TunnelManager/mounts/PROFILE`.

**You need:**
- [rclone](https://rclone.org/downloads/).
- On Windows: [WinFsp](https://winfsp.dev/rel/).
- On macOS: [macFUSE](https://osxfuse.github.io) or
  [FUSE-T](https://www.fuse-t.org).
- On Linux: FUSE (the `fuse3` package).

**Server identity:** rclone checks the server against your `~/.ssh/known_hosts`.
If it says the server is unknown, open the profile's Terminal once and accept the
server's key.

You do not need an `rclone.conf` for this. If your `rclone.conf` holds passwords
(`pass = ...`), know that they are only encoded, not encrypted.

### VS Code (Remote-SSH)

**Opens:** a folder of the server in VS Code with the Remote-SSH extension.

**Needs one of:**
- a Tunnel Manager key on the profile, added with **Auth... > Add managed key to
  system SSH config**;
- a profile on your system OpenSSH config and keys, without its own SSH port (or
  with a `Host` entry for it in your `.ssh\config`).

**Folder:** blank opens `/root` for root, or `/home/USER` otherwise.

### File manager (Linux)

**Opens:** the server in your desktop's file manager (GNOME Files, Dolphin, ...)
as `sftp://`.

**Sign-in:** with the profile's key in the system SSH config (Auth...), it uses
Tunnel Manager's key; a profile on your system OpenSSH config uses your keys.
Otherwise it asks for the password.

## Linked programs

Add any program in **Configure programs… > Link a Program**: FileZilla, Rclone
Browser, Cyberduck, and so on. You can link as many as you like.

**Arguments** can include the server's address:
- `{url}` (`sftp://user@host:port/folder`)
- `{host}`, `{user}`, `{port}`
- `{target}` (`user@host`)
- `{folder}`, `{profile}`
- `{alias}` (since v0.1.33): the profile's name in your SSH config

For example, FileZilla with the arguments `{url}`. The program asks for the
password itself.

**A program that reads your OpenSSH config** (for example one built on `ssh`)
can sign in with Tunnel Manager's key: choose **Auth... > Add managed key to
system SSH config** for the profile, and pass `{alias}` to the program.

## Logins without file access

Some logins only carry tunnels, for example a supervised AgentW account. For
those, supported programs explain that the login has no file access. Linked
programs still open.

## From the command line (all computers)

macOS and Linux have no manager window, so there the list is set up with
`tunnelm`:

```text
tunnelm open-with catalog                       # what can be added here
tunnelm open-with --profile PROFILE list        # the profile's programs
tunnelm open-with --profile PROFILE add rclone-mount --folder /
tunnelm open-with --profile PROFILE add linked --name FileZilla --path /usr/bin/filezilla --args "{url}"
tunnelm open-with --profile PROFILE edit ID --mount-point /mnt/server
tunnelm open-with --profile PROFILE remove ID
tunnelm open-with --profile PROFILE move ID up  # or down: the menu order
tunnelm open-with PROFILE ID                    # open it
```

From the command line, a password is used only if it is saved in the OS
credential vault.

## More programs

Supported programs are added release by release. A program is supported only
when Tunnel Manager can give it the login without the password appearing on a
command line. Until then it can always be added as a linked program.
