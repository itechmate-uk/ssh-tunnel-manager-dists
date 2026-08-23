# Platform Support

## Windows

Windows `amd64` is the current complete desktop release. Its archive contains:

- `tunnelm.exe`: no-console manager and tray application;
- `tunnelm-cli.exe`: PowerShell/CMD diagnostics;
- `tunnelm-webview.exe`: no-console WebView2 diagnostic; and
- `tunnelm-webview-cli.exe`: console WebView2 diagnostic.

The desktop supports manager/tray lifecycle, WebView2 app windows,
notifications, sounds, credential vault, file pickers, autostart, remote
services, and Local File Bridge workflows.

## Linux

Current Linux `amd64` and `arm64` archives are basic command builds. They can
validate configuration and run supported CLI workflows, but the native manager
window and tray adapter are not implemented in the current release.

Planned desktop parity includes a native manager/tray, supported WebView,
Secret Service, desktop notifications, sound, file pickers, autostart,
URL/file opening, DEB/RPM packages, and native interactive testing on
Debian/Ubuntu and Fedora/RHEL-compatible desktops.

## macOS

Current macOS Intel and Apple Silicon archives are basic command builds. The
native manager/menu-bar and app-window adapter are not implemented in the
current release.

Planned parity includes manager/menu-bar lifecycle, native WebView, Keychain,
notifications, sound, file dialogs, OpenSSH integration, autostart,
sleep/network recovery, Intel/Apple Silicon packaging, and real-hardware QA.

Cross-compilation and CI prove that code builds; they do not prove desktop
behavior. macOS release certification requires a real Mac because WebView,
Keychain, notification permission, menu-bar, resume, and packaging behavior
depend on macOS services. Standard-user hosted Mac access is sufficient for
ordinary application QA. Admin/root access is needed later for privileged
package-install and system-integration scenarios.
