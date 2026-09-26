# Nginx Proxy Manager

![Version][version-shield]
![Project Stage][project-stage-shield]
![Maintained][maintenance-shield]

[![Community Forum][forum-shield]][forum]
[![Buy me a coffee][buymeacoffee-shield]][buymeacoffee]

Expose your services easily and securely with a beautiful web GUI for Nginx.
This app (v0.4.0) runs the **latest upstream** [jc21/nginx-proxy-manager](https://github.com/NginxProxyManager/nginx-proxy-manager) image (currently v2.16.0).

## Features

- Free SSL via Let's Encrypt (HTTP-01 and DNS-01 challenges)
- Reverse proxy with custom locations, websocket support, and access lists
- Redirect and 404 hosts
- Beautiful web UI on port 81

## Ports

| Port | Protocol | Description         |
| ---- | -------- | ------------------- |
| 80   | TCP      | HTTP proxy traffic  |
| 81   | TCP      | Admin web UI        |
| 443  | TCP      | HTTPS proxy traffic |

## Initial access

After the app starts, open the admin UI at `http://<your-ha-ip>:81` and follow
the onboarding/login flow shown by the current upstream Nginx Proxy Manager
release.

## Data persistence

NPM's database and configuration are stored in the app's `/data` directory and
persist across restarts and updates.

Let's Encrypt certificates are stored at `/ssl/nginxproxymanager` — HA's shared SSL
directory. This means certificates issued by NPM are accessible to other HA apps
that map the `ssl` volume (e.g. the HA core or other proxy apps). Certificate paths
follow the standard Let's Encrypt layout:

```text
/ssl/nginxproxymanager/live/npm-1/fullchain.pem
/ssl/nginxproxymanager/live/npm-1/privkey.pem
```

## Upgrading

Update the app through the Home Assistant UI when a new version is published.

Your Nginx Proxy Manager data, configuration, and certificates are stored under
`/data`, so they persist across normal app upgrades and restarts.

After upgrading, verify that:

1. The app starts successfully.
2. The admin UI is reachable on port 81.
3. Existing proxy hosts, certificates, and settings are still present.

## Notes

- Ports 80 and 443 must be free on the host — disable HA's built-in nginx if it occupies them.
- This add-on does **not** use a HA base image; it uses the official NPM Docker image directly.

## Logo

The `icon.png` used by this add-on is the official Nginx Proxy Manager logo,
sourced from the [NginxProxyManager/nginx-proxy-manager](https://github.com/NginxProxyManager/nginx-proxy-manager)
repository. All logo rights belong to the Nginx Proxy Manager contributors.

[version-shield]: https://img.shields.io/badge/version-0.4.1-blue.svg
[project-stage-shield]: https://img.shields.io/badge/project%20stage-experimental-yellow.svg
[maintenance-shield]: https://img.shields.io/maintenance/yes/2026.svg
[forum-shield]: https://img.shields.io/badge/community-forum-brightgreen.svg
[forum]: https://community.home-assistant.io
[buymeacoffee-shield]: https://img.shields.io/badge/buy%20me%20a%20coffee-donate-yellow.svg
[buymeacoffee]: https://www.buymeacoffee.com/slopsynclabs
