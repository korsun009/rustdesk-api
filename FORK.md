# Maintained fork

This repository is the `korsun009` fork of
[`lejianwen/rustdesk-api`](https://github.com/lejianwen/rustdesk-api).
It integrates the stable [`alonginwind v4.1.8`](https://github.com/alonginwind/rustdesk-api/releases/tag/v4.1.8)
as its maintained base and is the source of local fixes for our RustDesk API deployment.
The upstream Go module path is intentionally retained for compatibility.

## Native login compatibility

RustDesk clients expect the payload after `common-oidc/` in
`GET /api/login-options` to decode to a JSON array. Upstream v2.7 serializes
an uninitialized slice as `null` when no OIDC provider or web SSO is enabled.
The same defect is present in the alonginwind v4.1.8 base; our fix is retained.
This causes `Null is not a subtype of FutureOr<List<dynamic>>` before the
user submits their password.

`TestLoginOptionsClientCompatibility` exercises the actual HTTP handler with
SQLite for password-only login, web SSO, and a configured external provider.
It verifies both the common provider list and legacy `oidc/` entries.

## Development and builds

The `Finland CI` workflow runs the regression and related unit tests and builds
a static Linux amd64 backend, including SQLite. Its artifact contains `apimain`,
the exact source commit, and SHA256SUMS. It does not publish images, deploy,
or require production credentials. Existing upstream release workflows remain
available for deliberate use; this fork's CI does not call them.

The Redis integration tests inherited from upstream reference a developer LAN
address. They are not part of the selected CI packages; this workflow does not
claim a full Redis integration pass.

Deployment configuration, passwords, database files, certificates, live device
identifiers, and backup archives must remain outside the public repository.
Deploy a tested backend alongside the matching alonginwind v4.1.8 resources, retain
the previous binary for rollback, back up the database before replacement,
and verify login options, authentication, and address-book operations afterward.

The redundant self-module require/replace directives from alonginwind are
removed: builds must use this checkout, rather than refer to a moving main branch.
The committed go.sum locks downloaded dependencies.

For future updates, compare stable tags from the `alonginwind` remote and the `upstream`
remote before merging. Preserve the regression test and avoid replacing the
production database or its server identity during a binary upgrade.
