# Ubuntu Server Baseline

[![HTML validation](https://github.com/wasiliy-strecker/ubuntu-server-minimal-conf/actions/workflows/html.yml/badge.svg)](https://github.com/wasiliy-strecker/ubuntu-server-minimal-conf/actions/workflows/html.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A compact, security-conscious Ubuntu and Apache deployment runbook. It documents not only setup commands but also their safety boundary, verification steps, operational checks, and rollback path.

**[Open the rendered runbook](https://wasiliy-strecker.github.io/ubuntu-server-minimal-conf/)**

## Topics covered

- Named administrator and SSH public-key hardening.
- UFW policy with an explicit lockout warning.
- Automatic security updates and maintenance considerations.
- Release directories with a reversible `current` symlink.
- Least-privilege Apache filesystem access and virtual-host configuration.
- Let's Encrypt issuance and renewal verification.
- Service, log, listener, certificate, and capacity checks.
- Atomic rollback and backup/restore expectations.

## Why a runbook instead of an install script?

Server configuration is environment-specific and can affect remote access. This repository intentionally keeps the commands reviewable rather than pretending that an untested one-click script is safe for every host. The sequence calls out the points where a second SSH session, DNS state, real paths, or a maintenance window are required.

## Local preview

Open `index.html` directly in a browser. The page has no runtime dependencies, external fonts, analytics, or build step.

## Validation

GitHub Actions validates the HTML document on every change. Commands in the runbook must still be reviewed and tested on the target Ubuntu LTS release before production use.

## License

[MIT](LICENSE)
