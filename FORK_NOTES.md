# Fork notes (levoniust/bumper, branch `tim-main`)

Personal fork of [MVladislav/bumper](https://github.com/MVladislav/bumper) used to run a
self-hosted Ecovacs server for a DEEBOT T90 PRO OMNI (`twunby`) on a TrueNAS app.

## Base
`main` @ 9d24c3f (2026-08-27), newer than release v0.4.1 (2026-08-03): refreshed Ecovacs
product configs (`productIotMap.json` includes `twunby`), codepush data, htmx.

## Changes
| Change | Source | Why |
|---|---|---|
| PyJWT 2.13.0 → 2.15.0 | upstream renovate branch `renovate/pypi-pyjwt-vulnerability` (security) | Pending security fix not in main yet |
| cryptography 50.0.0 → 50.0.2 | patch release, `uv lock --upgrade-package cryptography` | Patch-level update; only this package changed in `uv.lock` |

Deliberately **not** taken: `renovate/python-dependencies` (bumps amqtt 0.11 → 0.12, a breaking
upgrade upstream is handling in `feature/amqtt-012-upgrade`).

Tests: `uv run pytest` → 350 passed (2026-10-14).

## Known risk for this robot
Ecovacs firmware from mid-2025 on verifies server certificates (see upstream discussion #90 and
`docs/internals/certificate-unpinning-bot.md`). Bumper's DNS doc lists
`jmq-ngiot-*.dc.robotww.ecouser.net` (the newer MQTT endpoint) under "TLS errors". A T90 PRO
may refuse Bumper's self-signed certificate unless the bot's CA store is changed over UART.

## Build
Image is built locally on the TrueNAS host (`docker build -t bumper:tim-main .`); see the
deployment notes in the homelab handoff docs. Secrets (certs, `.env`, `data/`) stay out of git
(the repo's `.gitignore` is allow-list based).
