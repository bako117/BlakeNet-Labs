---
date: 2026-07-13
tags: [runbook, siem]
---

# Splunk

## Overview
Central SIEM for the lab — the destination for most log/telemetry sources across the environment.

## Deployment
- Runs on the `splunkserver` VM, hosted on `BK-DESKTOP`, up 24/7 alongside `BK-DESKTOP` itself.
- Single all-in-one instance (no separate indexer/search head/forwarder split).
- Developer license — 50GB/day ingest limit.

## Data Sources
- Performance/metrics data from the `BK-DESKTOP` hypervisor.
- Sysmon and Windows Event Logs from: the Windows laptop, the `BK-DESKTOP` hypervisor, and a dedicated test/victim VM used as a target for security testing.
- A Linux logs index (sources not fully catalogued yet).
- Suricata `eve.json` (see [Suricata](../Suricata/Suricata.md)).
- Pi-hole admin and query logs (see [PiHole](../PiHole/PiHole.md)).
- RunZero scan results (see [RunZero](../RunZero.md)).
- Ingestion method: Universal Forwarders installed on each source host.

## Dashboards
- Performance monitoring dashboard for the hypervisor / main control center (`BK-DESKTOP`).
- Suricata detections dashboard, covering all triggered rules.

## Integrations
- A Splunk MCP server is configured, allowing AI tooling to query the Splunk instance directly — a building block toward the "AI SOC agent" idea on the ideas list.
