# Changelog

All notable changes to the Nginx Proxy Manager app will be documented here.

## [0.4.1] - 2026-09-26

### Changed

- Bump upstream Nginx Proxy Manager to `2.16.0` (was `2.15.1`)

---

## [0.4.0] - 2026-06-10

### ⚠️ Repository Moved

This app now lives in its own dedicated repository:

**➜ https://github.com/SlopSync-Labs/ha-app-nginx-proxy-manager**

To continue receiving updates, add the new repository source in Home Assistant:

> **Settings → App Store → ⋮ → Repositories**
> Add: `https://github.com/SlopSync-Labs/ha-app-nginx-proxy-manager`

The old source (`home-assistant-apps`) will continue to work via redirect for now,
but will not receive future updates. You can keep both sources in HA — no need to
remove the old one immediately.

> **💡 Before making any changes**, we recommend using the
> [NPM Export Import](https://github.com/SlopSync-Labs/ha-app-npm-export-import) app
> to export and back up all your Nginx Proxy Manager host settings. If anything goes
> sideways during migration, you can use it to fully reinstate your configuration.

### Changed

- Repository renamed from `home-assistant-apps` to `ha-app-nginx-proxy-manager`
- Updated container image registry path to match new repo name

---

## [0.3.6] - 2026-06-03

### Changed

- Bump upstream Nginx Proxy Manager to `2.15.1` (was `2.15.0`)

---

## [0.3.5] - 2026-06-02

### Changed

- Bump upstream Nginx Proxy Manager to `2.15.0` (was `2.14.0`)

---

## [0.3.4] - 2026-05-08

### Fixed

- Drop `armv7` from supported architectures — `jc21/nginx-proxy-manager` does not
  publish an `armv7` image, causing CI builds to fail with "no match for platform in manifest"

---

## [0.3.3] - 2026-05-08

### Fixed

- Add `image` field to `config.json` pointing to pre-built ghcr.io images so HA Supervisor
  pulls instead of building locally — fixes install failure on systems running Docker 29.x
  where `docker:29.x.x-cli` build runner images are not published to Docker Hub

### Added

- GitHub Actions workflow to build and publish per-arch images to ghcr.io on every push to `main`

---

## [0.3.0] - 2026-03-13

### Added

- Let's Encrypt certificates now stored at `/ssl/nginxproxymanager`, making them
  accessible to other HA add-ons that map the `ssl` volume
- `ssl:rw` added to add-on volume map
- Buy Me a Coffee badge linking to SlopSync-Labs

### Changed

- Symlink target for `/etc/letsencrypt` changed from `/data/letsencrypt` to
  `/ssl/nginxproxymanager`

---

## [0.2.0] - 2026-03-13

### Added

- `init: false` in `config.json` — prevents HA Supervisor from injecting a Docker
  init wrapper, allowing s6-overlay to run as PID 1 (fixes startup crash)
- `webui` field in `config.json` — adds "Open Web UI" button in HA pointing to port 81
- `backup_exclude` to omit logs from HA snapshots
- Official NPM icon (`icon.png`, 256×256) with attribution
- shields.io badges: version, project stage, maintained, community forum, Buy Me a Coffee
- Patched NPM's `prepare` s6 service to accept a symlink for `/etc/letsencrypt`
  instead of requiring a Docker volume mount point
- `cont-init.d/01-ha-setup.sh` to create required `/data` subdirectories before
  NPM services start

### Fixed

- `s6-overlay-suexec: fatal: can only run as pid 1` crash on startup
- `cont-init.d` script failing with "not found" due to missing `with-contenv`
  interpreter — changed shebang to `#!/bin/sh`
- `ERROR: /etc/letsencrypt is not mounted` — patched NPM's mountpoint check

---

## [0.1.0] - 2026-03-13

### Added

- Initial scaffold for the Nginx Proxy Manager add-on
- `FROM jc21/nginx-proxy-manager:2.14.0` — uses the official upstream Docker image
  directly rather than building from source
- Ports: 80 (HTTP), 81 (admin UI), 443 (HTTPS)
- Multi-arch support: `amd64`, `aarch64`, `armv7`
