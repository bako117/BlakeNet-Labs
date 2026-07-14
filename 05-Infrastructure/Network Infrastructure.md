---
date: 2026-07-13
tags: [infrastructure]
---

# Network Infrastructure

## Overview
A flat home network shared between the lab and general household devices — no VLANs or segmentation in place yet.

## Edge / Perimeter
- Consumer-grade router providing routing/NAT/firewall duties for the lab and home network.
- Two things are exposed externally:
  - A Cloudflare Tunnel — the sole ingress path for the lab's automation host (see Compute Hosts below); no inbound port forward required.
  - One inbound port forward for a low-criticality personal game server (details intentionally omitted — see Redaction Notes).

## Compute Hosts
- `BK-DESKTOP` — primary hypervisor; hosts the Splunk VM, FlareVM, Kali VM, an internal-only Minecraft VM, and a dedicated test/victim VM used as a target for security testing.
- Raspberry Pi fleet (`PROD-PI-01` through `PROD-PI-04`, plus `PI-KALI`) — see the tool docs under `01-Runbooks & Docs/` for what runs on each.

## Switching
- A single managed switch connects all wired lab devices (AP, main compute host, Raspberry Pi fleet) to the router.
- The switch mirrors the router's uplink port to a dedicated port feeding the lab's IDS sensor, giving it visibility into all router-bound traffic.
- No VLANs configured currently. A guest network / segmentation is being considered but not yet implemented.

## Wireless
- One access point, wired into the managed switch.
- Single SSID; no guest network or segmentation yet.

## Endpoints (non-infrastructure)
- Smart TV (wired)
- Personal phone
- Windows laptop (rarely used)
- Linux Mint laptop (rarely used — candidate for repurposing as a server)

## Redaction Notes
> This document is public-facing. Deliberately omitted: router make/model and firmware version, real IP ranges and hostnames, the specific externally exposed port/service, and any tunnel or credential identifiers.
