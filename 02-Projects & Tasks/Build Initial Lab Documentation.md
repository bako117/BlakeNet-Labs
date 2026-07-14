---
date: 2026-07-13
status: planning
tags: [project]
---

# Project: Lab Documentation

## Summary
| Field | Value |
|-------|-------|
| **Status** | Complete |
| **Started** | 2026-07-13 |
| **Completed** | 2026-07-13 |
| **Tools/Services** | Obsidian (this vault) |

## Objective
> What problem am I solving, or what capability am I building? Why does it matter for my lab or skill set?

Create thorough documentation for the home lab infrastructure, runbooks, and detections so the lab's setup and history are captured and easy to reference or rebuild from.

## Background & Motivation
> Context for why this project started — an incident, a gap, something I wanted to learn.

I want to have public documentation for the work I am doing and references to go back to. I left my lab for a while to study for a certification and returned with a few knowledge gaps.

**Note:** This repo is public-facing — the goal is to show ongoing lab work to others (e.g. for interviews). Documentation must avoid real IPs/domains, credentials, exact vulnerable software versions, or anything else that gives away a true attack surface.

## Implementation Steps

- [x] Draft a short "what not to publish" checklist (no real IPs/domains/hostnames, no credentials, no exact exploitable versions, no physical location) to apply across all lab docs
- [x] Build a baseline inventory (hosts, VMs, network devices, services) from scratch via interview
- [x] Fill in `05-Infrastructure/Diagram of Lab.md` and `Network Infrastructure.md` from that inventory
- [x] Fill in tool runbooks one at a time (RunZero, Splunk, Uptime Kuma, Suricata, Pi-hole, N8N, others as identified)
- [x] Backfill `03-Detections/` — deferred: no detections exist yet, building them out is its own learning goal (see ideas list)
- [x] Capture day-to-day process — deferred: no real routine exists yet beyond occasionally checking Uptime Kuma/Splunk; patching/update process is now its own idea (see ideas list)
- [x] Decide on an upkeep habit so docs don't go stale again — added a trigger-based prompt to `Templates/Daily Templates.md` and a monthly/post-break review note in `00-ChangeLog/Daily Update Notes/README.md`

## Challenges & How I Solved Them
> Document blockers and the solutions. These are the most valuable interview talking points.

| Challenge | Root Cause | Resolution |
|-----------|-----------|------------|
|           |           |            |

## Results
> What does done look like? Screenshots, metrics, or a short description of the working outcome.

- I have a lot more documentation on each of the tools working in my environment
## Lessons Learned
> What would I do differently? What did I learn that I didn't know before?

-  I was able to find a lot of gaps I did not know existed while going through with this project. 

## Next Steps / Future Work
- [ ] Continue maintaining documentation
- [ ] Document new processes as they get defined

## References & Resources
- See `01-Runbooks & Docs` 

---
*Tags:* #project
