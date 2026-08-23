# Unsigned Build Safety

Current preview binaries are not signed by Microsoft, Apple, or a Linux
package repository. Platform reputation and trust systems may therefore warn
even when a file is unchanged from the published release.

## Before Bypassing A Warning

1. Download only from this repository's GitHub Releases page.
2. Verify the archive against the release `SHA256SUMS.txt`.
3. Confirm the archive name, version, OS, and architecture.
4. Do not continue if the checksum differs or the download came from another
   site.

## Windows

After verification, SmartScreen may offer **More info > Run anyway**. This is
expected for an unsigned preview binary. Do not install a self-signed root
certificate supplied by an unknown party and do not disable SmartScreen
globally.

## macOS

After verification, use Finder **Control-click > Open**, or approve the exact
application in **System Settings > Privacy & Security**. Do not disable
Gatekeeper globally.

## Linux

Archives do not carry an executable permission on every extraction path. Set
it only on the verified binary:

```sh
chmod +x tunnelm
```

Desktop environments may also ask whether to trust/launch a downloaded
executable. Approve only the checksum-verified file.

Future Microsoft Store, signed Linux repository, and Apple signing/notarization
channels are planned. Until then, the checksum and release manifest are the
public integrity records.
