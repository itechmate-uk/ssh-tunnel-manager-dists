# Security

Do not report credentials, private keys, production server addresses, or other
secrets in a public issue.

Verify every downloaded archive against the `SHA256SUMS.txt` attached to the
same GitHub release. Release artifacts are produced from reviewed version tags
in the Tunnel Manager source repository.

Tunnel Manager `v0.1.9` was withdrawn after Microsoft Defender reported
`Trojan:Win32/Wacatac.B!ml` for its Windows executable. Do not restore or
redistribute it. Current releases are gated by a Microsoft Defender scan of
the exact Windows package before publication.

Until a private vulnerability-reporting channel is published, open a public
issue containing only non-sensitive contact information and request a private
maintainer response.
