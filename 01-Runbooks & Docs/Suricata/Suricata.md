---
date: 2026-07-13
tags: [runbook, ids]
---

# Suricata

## Overview
Network intrusion detection for the lab, watching traffic mirrored from the router's uplink port.

## Deployment
- Runs on `PROD-PI-02` as a native package (not containerized).
- Deployed in **IDS mode** (detect/alert only, not inline blocking).
  - Deliberate choice: inline (IPS) blocking felt too risky for disrupting the home network, and the Pi doesn't have the resources to support IPS mode reliably.

## Ruleset
- Standard/community ruleset, with a handful of noisy rules disabled to cut down on false positives.
- See [Disabling an IDS rule](Disabling%20an%20IDS%20rule.md) for the process used to disable a rule by SID.

## Log Pipeline & Splunk Integration
- `eve.json` logs are shipped into Splunk for further analysis.
- A Splunk dashboard visualizes detections triggered by the ruleset.

## Related
- [Disabling an IDS rule](Disabling%20an%20IDS%20rule.md) — runbook for suppressing a specific noisy SID.
