---
date: 2026-07-13
tags: [runbook, monitoring]
---

# Uptime Kuma

## Overview
Synthetic/uptime monitoring for the lab. Provides peace of mind rather than active incident response — outages are rare (roughly monthly) and typically self-resolve before any action is needed.

## Deployment
- Runs as a Docker container on `PROD-PI-01`.

## What It Watches
- Every Raspberry Pi in the fleet
- The services running on each Pi
- The Splunk VM (`splunkserver`)

## Check Configuration
- All monitors poll on a 2-minute interval.

## Alerting
- All alerts route to Discord (see [Uptime Kuma Alerting](../02-Projects%20&%20Tasks/Tasks/Uptime%20Kuma%20Alerting.md) for how this was set up and tested).

## Notes
- No meaningful operational overhead — set-and-forget monitoring. No runbook steps needed beyond the initial setup.
