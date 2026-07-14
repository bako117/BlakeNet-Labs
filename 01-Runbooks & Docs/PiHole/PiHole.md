---
date: 2026-07-13
tags: [runbook, dns, dhcp]
---

# Pi-hole

## Overview
Provides DNS resolution/filtering and DHCP for the entire flat network — lab devices and personal/household devices alike.

## Deployment
- Runs as a Docker container on `PROD-PI-01`.

## Role
- **DNS** — resolves and filters DNS for every device on the network.
- **DHCP** — sole DHCP server for the entire network (not lab-only).

## Blocklists
- Uses a standard/default blocklist (no custom curation at this time).

## Local DNS
- A handful of friendly local DNS names are configured for easier resolution of lab hosts (rather than remembering IPs).

## Logging
- Both admin logs and actual DNS query logs are shipped to Splunk for analysis.
