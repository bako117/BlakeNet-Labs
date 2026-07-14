---
date: 2026-07-13
tags: [runbook, asset-management]
---

# RunZero

## Overview
Provides asset inventory, network discovery, and light vulnerability scanning for the lab. Results are fed into Splunk for searching/correlation alongside other lab telemetry.

## Deployment
- Collector runs on `PROD-PI-01`.
- Installed as a native package (per RunZero's own recommended install method), not containerized.
- Free/community edition.

## Scan Configuration
- One full scan of the entire lab network per day.

## Use Cases
- Asset inventory / discovery — the primary use, and a natural companion to the manual infrastructure inventory captured in `05-Infrastructure/`.
- Light vulnerability scanning.
- Results forwarded into Splunk for correlation and search.
