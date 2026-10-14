# AOPM Secure Graphics Feed

Starter repository for **graphics-only updates** for AOPM Professional Edition.

## What belongs here
- Approved application logos, icons, and background images
- A versioned manifest describing approved assets
- Security and publishing documentation

Graphics updates must not change application menus, layout, labels, permissions, workflows, or application logic. The desktop app must keep bundled graphics available for offline use.

## Repository files
- `SECURITY.md` — client-side validation, signing, rollback, and key-handling requirements
- `manifest.schema.json` — template schema for validating manifest structure
- `manifest.example.json` — example only; deliberately unsigned and must be rejected by a production updater
- `graphics/README.md` — asset publishing rules

## Production readiness
**This repository is not yet a live production feed.** No production graphics assets or signed release manifest have been published, and the AOPM desktop updater has not been integrated or tested from this repository.

Before enabling automatic updates, implement and test all of the following in the desktop app:
1. HTTPS-only requests to an allow-listed host, with valid TLS and no insecure redirects.
2. Ed25519 signature verification over the exact manifest bytes using a public key embedded in the installed app.
3. SHA-256 and byte-size verification for every asset before installation.
4. Strict file-type and size validation; SVG must be sanitized or disallowed.
5. Staging, backup, rollback, audit logging, anti-rollback checks, and offline fallback.
6. Explicit user approval and a clear distinction between **Graphics Only** and **Full System Update**.

A checksum verifies file integrity, but does not prove who published the file. Do not treat SHA-256 alone as publisher authentication.

## Signing-key rule
Never commit a private signing key, access token, password, or other secret. Keep the signing key offline or in a protected signing service. Only the public verification key belongs in the installed application.

## Publishing
Use immutable versioned asset paths. After assets are uploaded, compute SHA-256 hashes from the exact final files, generate the manifest, sign the exact manifest bytes, then verify the signature and hashes with the same checks the desktop app will use. Do not point production clients at `manifest.example.json`.

Maintainer: AOPM Professional Edition project.
