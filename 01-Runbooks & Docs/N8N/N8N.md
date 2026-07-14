---
date: 2026-07-13
tags: [runbook, automation]
---

# n8n

## Overview
Automation platform for the lab. Currently in an experimental phase — no active production workflows are running today, but there's a clear intent to build out much more automation here.

## Deployment
- Runs as a Docker container on `PROD-PI-04`.
- Reachable externally via a Cloudflare Tunnel (no inbound port forward).

## Current Workflows
- One test workflow: a Splunk-triggered webhook that kicks off an n8n workflow. Proof of concept, not tied to any real action yet.
- No other active workflows at the moment.

## History
- Previously had a working Discord-triggered workflow to start/stop the Minecraft server VM. Lost when the SD card in `PROD-PI-04` failed/corrupted, taking the workflow with it. Never rebuilt.

## Triggers Explored
- **Schedule trigger** — used
- **Webhook trigger** — used (Splunk integration)
- **Discord trigger** — preferred notification/command path
- **Slack trigger** — configured at one point, but Discord was preferred in practice

## Notes
- See the ideas list for planned expansion of n8n automation and for hardening against another SD-card-loss incident.
