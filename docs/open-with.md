# Opening A Server In Your Programs (Open With)

Since v0.1.29.

Tunnel Manager does not have a file manager of its own. Instead, it opens the
profile's server in programs you already use, such as WinSCP, an rclone drive or
VS Code, and signs them in for you where it can.

## Open with ▾

The **Open with ▾** button is in each profile's row in the manager window, next
to **Terminal**.

**The menu:**
- your programs, each opening the selected profile;
- **Configure programs…**, where you add, change or remove programs.

The list belongs to this computer only.

## Two kinds of program

| Kind | Sign-in |
|---|---|
| **Supported program** | Tunnel Manager signs it in with the profile's login: its own key (managed key), or the password it holds for this session or in the OS credential vault. |
| **Linked program** | Any other program. It gets the server's address and asks for the login itself. |

Tunnel Manager never puts a password on a command line, in an address, or in a
file the program keeps.

## Supported programs

| Program | Computers | With Tunnel Manager's key | With a saved password |
|---|---|---|---|
| **WinSCP** | Windows | Signs in with a PuTTY copy of the key (`.ppk`) | Signs in. The password goes through a private file that is deleted after 2 minutes |
| **rclone mount** | Windows, macOS, Linux | Signs in | Signs in. The password goes to rclone directly, never to a file |
| **VS Code (Remote-SSH)** | Windows, macOS, Linux | Signs in, with "Add profiles to SSH config" turned on | Not supported: use a key |
| **File manager** | Linux | Signs in, with "Add profiles to SSH config" turned on | The file manager asks |

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

**Needs:**
- a Tunnel Manager key on the profile;
- **SSH Settings > Terminal > Add profiles to SSH config** turned on.

**Folder:** blank opens `/root` for root, or `/home/USER` otherwise.

### File manager (Linux)

**Opens:** the server in your desktop's file manager (GNOME Files, Dolphin, ...)
as `sftp://`.

**Sign-in:** with "Add profiles to SSH config" on, it uses Tunnel Manager's key.
Otherwise it asks for the password.

## Linked programs

Add any program in **Configure programs… > Add Other program (link)**: FileZilla,
Rclone Browser, Cyberduck, and so on.

**Arguments** can include the server's address:
- `{url}` (`sftp://user@host:port/folder`)
- `{host}`, `{user}`, `{port}`
- `{target}` (`user@host`)
- `{folder}`, `{profile}`

For example, FileZilla with the arguments `{url}`. The program asks for the
password itself.

## Logins without file access

Some logins only carry tunnels, for example a supervised AgentW account. For
those, supported programs explain that the login has no file access. Linked
programs still open.

## From the command line (all computers)

macOS and Linux have no manager window, so there the list is set up with
`tunnelm`:

```text
tunnelm open-with catalog                       # what can be added here
tunnelm open-with list                          # this computer's programs
tunnelm open-with add rclone-mount --folder /
tunnelm open-with add linked --name FileZilla --path /usr/bin/filezilla --args "{url}"
tunnelm open-with edit ID --mount-point /mnt/server
tunnelm open-with remove ID
tunnelm open-with PROFILE ID                    # open it
```

From the command line, a password is used only if it is saved in the OS
credential vault.

## More programs

Supported programs are added release by release. A program is supported only
when Tunnel Manager can give it the login without the password appearing on a
command line. Until then it can always be added as a linked program.
