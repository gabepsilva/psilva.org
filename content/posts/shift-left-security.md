+++
date = '2026-05-22T00:00:00-04:00'
draft = false
title = 'Shift-left security (and shift-left without the security part)'
tags = ['devops', 'devsecops', 'security', 'platform']
categories = ['engineering']
image = "/images/shift-left-security-banner.png"
featuredImage = "/images/shift-left-security-banner.png"
+++

If you build pipelines, own Terraform, or get pulled into release-week fire drills, you have probably heard "shift-left" in a standup, a job post, or a security review. It is not a product name. It is a way of saying: do this work earlier, while you are still in design, a PR, or CI, instead of handing it to security (or ops) at the door to production.

That framing shows up a lot in DevSecOps and platform roles. This post is what people usually mean by it, plus the same idea when the topic is not security.

<!--more-->

Shift-left security means doing security work earlier in how you build and ship software. Not mainly at the end, right before release, or after an incident.

Drop the word "security" and you get the same idea: shift-left. Move work earlier in the lifecycle so problems are cheaper and faster to fix.

---

### The timeline (left → right)

A typical delivery line looks like this:

**Plan / design → build → test → deploy → operate**

Shift left means moving work toward design and build. Shift right means leaving it for deploy, operate, audit, or incident response.

"Shift-left security" is security moved left. "Shift-left testing" or "shift-left reliability" is the same motion for a different concern.

---

### What shift-left security looks like

| Traditional ("shift right") | Shift-left |
| --- | --- |
| Security review right before production | Threat modeling and security requirements during design |
| Annual pen tests | SAST/SCA in CI on every PR |
| Manual access reviews after the fact | RBAC and least privilege in onboarding and IaC |
| Security team approves at the end | Developers get guardrails, scanners, and standards up front |

The point: catch security problems in design, code, CI, access requests, and vendor intake. Not only in production audits.

---

### Why teams bother

Fixing something in a design doc or a PR is usually cheaper than fixing it in production or during an incident. You also get fewer last-minute scrambles before release and fewer compliance surprises. Security becomes part of how you deliver instead of a gate at the end.

---

### Shift-left without "security"

Same pattern, different topic. Fix and validate early instead of late, close to prod.

| Shift-left (general) | Old "shift right" pattern |
| --- | --- |
| Testing in CI on every PR | Big manual QA right before release |
| Design reviews before coding | Architecture debates after the build is done |
| Observability designed in (metrics, logs, SLOs) | Add monitoring after outages |
| Reliability (capacity, failure modes) in design | Harden only after production pain |
| Cost reviews when choosing architecture | FinOps cleanup after the bill spikes |
| Docs and runbooks with the feature | Write ops docs during an incident |

Shift-left: fix and validate early. Shift-right: fix and validate late.

---

### Same pattern, different domain

| Focus | Shift-left in practice |
| --- | --- |
| Security | SAST, RBAC, vendor trust gates early |
| Quality | Tests and lint in CI |
| Reliability | SLOs and failure design up front |
| Operations | IaC, observability, runbooks from day one |

If you say "shift-left" without a qualifier, people often mean shift-left engineering, or shift-left testing, shift-left reliability, and so on. Same motion, different concern.

---

### Process example: vendor intake

Putting requirements early in a vendor lifecycle is shift-left in a process sense. Standards and gates land before procurement, not after the tool is already bought and running.

That is the same idea as shift-left security (gates and checks early), applied to third-party trust and procurement instead of code scanning. Still "move work left" on the timeline.

---

### How to talk about it on a resume or profile

"Shift-left security" fits when your work actually happened early: threat modeling in design, scanners on every PR, RBAC in IaC, vendor requirements before procurement.

In a profile line, it can read as part of a DevSecOps or platform focus: security in design, CI, and access from the start, not only at release.

When you write "shift-left security," you are saying you do not wait for production or an audit to care about security. You build it into planning, code, the pipeline, and onboarding.

When you write "shift-left" alone, you mean the same for whatever domain you name: quality, reliability, cost, or operations. Earlier in the lifecycle, not mainly at the end.
