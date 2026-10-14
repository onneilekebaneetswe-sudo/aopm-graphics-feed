# AOPM Graphics Feed Security Policy

This repository is a feed skeleton, not yet a production update channel.

## Required client behavior
- Fetch only from an explicitly trusted HTTPS origin.
- Reject plain HTTP, invalid TLS certificates, unexpected hosts, and insecure redirects.
- Verify a detached digital signature over the exact manifest bytes using a public key embedded in the installed AOPM application.
- Verify each downloaded asset's SHA-256 digest and declared byte length before installing it.
- Allow only approved image formats (PNG, JPEG, WebP, SVG only if sanitized) and enforce strict size limits.
- Never execute downloaded graphics or treat assets as code.
- Stage updates, preserve the last-known-good assets, and roll back automatically if validation or application fails.
- Keep an audit record of the update attempt, version, result, and failure reason.
- Keep the application usable offline with bundled graphics.

## Publisher key handling
The signing private key must be generated and kept offline or in a protected signing service. **Never commit a private key, token, password, or signing secret to this repository.** The application should ship only with the corresponding public verification key. Rotate keys through a separately designed, authenticated trust-update process.

## Manifest rules
- A production manifest must be signed; a SHA-256 checksum alone does not authenticate the publisher.
- Use immutable, versioned asset paths and HTTPS URLs on an allow-listed origin.
- Include a monotonically increasing release number or equivalent anti-rollback control.
- Reject unknown fields or unsupported manifest versions unless the client explicitly supports them.
- Do not publish a production manifest until signature generation and verification have been implemented and tested end to end.

## Status
The example manifest in this repository is documentation only. It is not a live release and must not be treated as an update instruction.
