---
layout: post
title: "Volt Typhoon: an actor profile built from open sources"
date: 2026-10-07
categories: threat-intelligence
---

*Every claim below is tied to a public source and graded for confidence. Where vendors disagree, I
say so. TLP:WHITE — open sources only.*

## How to read this

I use four confidence levels, and I apply them per claim, not per section:

- **Confirmed** — corroborated by two or more independent authoritative sources (a government
  advisory, MITRE ATT&CK, or multiple vendors).
- **Probable** — reported by a single credible source and consistent with the actor's known tradecraft.
- **Context only** — reported or self-claimed, but not independently corroborated.
- **Unverified** — could not be substantiated, stated so it does not get repeated as fact.

## Bottom line

Volt Typhoon is a People's Republic of China (PRC) state-sponsored intrusion set, active since at
least 2021, that targets critical infrastructure — above all **communications, energy, transportation
and water/wastewater** — in the continental United States, its territories (including Guam) and
Western-aligned countries. Its defining trait is not a tool but the absence of one: it prefers stolen
credentials, living-off-the-land (LOTL) binaries, hands-on-keyboard activity and web shells over
custom malware, and it hides behind chains of compromised SOHO routers and edge devices. U.S.
agencies assess the activity as **pre-positioning** for disruptive or destructive operations against
operational technology (OT) in the event of a crisis or conflict, not as espionage alone. Since 2025
the emphasis has moved from access to preparation: OT-focused reporting places a closely overlapping
cluster inside engineering workstations and cellular gateways — Stage 2 of the ICS Cyber Kill Chain —
with initial access increasingly brokered by a separate access cluster, SYLVANITE.

## 1. Identity and naming

The same activity is tracked under several designations:

| Designation | Attributed to |
|---|---|
| Volt Typhoon | Microsoft |
| BRONZE SILHOUETTE | Secureworks |
| Vanguard Panda | CrowdStrike |
| DEV-0391, Storm-0391 | Microsoft (initial designations) |
| UNC3236 | Mandiant / Google |
| Voltzite | Dragos |
| Insidious Taurus | Palo Alto Networks Unit 42 |
| Redfly | Symantec / Gen Digital |
| DazedToad | MITRE ATT&CK |
| G1017 | MITRE ATT&CK |

MITRE records these as **Associated Groups** [3], which is explicitly weaker than "same actor".
CISA lists Vanguard Panda, BRONZE SILHOUETTE, Dev-0391, UNC3236, Voltzite and Insidious Taurus as
aliases in its advisory [1].

**Caveat — Context only.** Not every tracking name is asserted to be the same actor. Dragos's
VOLTZITE is described as sharing "extensive technical overlaps" with Volt Typhoon rather than being
declared identical [7], and it is Dragos's analyst designation for the OT-facing activity. Read the
OT reporting as an **overlapping cluster**, not a single verified entity [3][7].

## 2. Assessment ledger

| Assessment | Confidence | Basis |
|---|---|---|
| PRC state-sponsored, active since at least 2021 | Confirmed | [1][3] |
| Targets communications, energy, transportation and water/wastewater in the US and its territories, including Guam | Confirmed | [1] |
| Assessed as pre-positioning for disruptive or destructive attacks against OT | Confirmed | [1][3] |
| Living-off-the-land tradecraft: stolen credentials, `wmic`/`ntdsutil`/`netsh`/PowerShell, minimal custom malware | Confirmed | [1][3] |
| Uses compromised SOHO routers (KV Botnet and successors) to proxy C2 and obscure origin | Confirmed | [1][6][8] |
| Exploited the Versa Director zero-day CVE-2024-39717 with the VersaMem web shell | Confirmed | [3][9] |
| Deployed the SockDetour backdoor in at least one intrusion | Probable | [10] |
| Initial access increasingly brokered by SYLVANITE and handed to Volt Typhoon/VOLTZITE | Probable | [3][7] |
| Reached Stage 2 of the ICS Cyber Kill Chain in 2025 (engineering workstations, cellular gateways) | Probable | [7] |
| Operated by the PLA Cyberspace Force | Context only | [11] |
| Dragos's VOLTZITE is the same actor as Volt Typhoon | Context only (overlap) | [3][7] |

## 3. Tradecraft

**Living off the land, by design.** The actor's signature is doing almost everything with tools that
are already on the host: `wmic`, `ntdsutil`, `netsh` and PowerShell, plus built-in remote-access
features, all under valid — usually stolen — accounts. The effect is that its traffic and its
process tree look like an administrator's. CISA's advisory documents the command lines and publishes
detection signatures precisely because there is no malware to fingerprint [1][3].

**Infrastructure obfuscation and proxying.** Rather than host its own command-and-control, it
builds chains of compromised small office/home office (SOHO) routers, mostly end-of-life Cisco and
Netgear devices, and proxies through them so that its traffic originates from U.S. IP addresses. The
FBI named the best-known instance the **KV Botnet** [6]; vendors and OT reporting have observed
successor botnets such as the JDY botnet after the takedown [7][8].

**Initial access through edge devices.** Volt Typhoon reaches victims by exploiting internet-facing
edge infrastructure — VPN gateways, firewalls and SD-WAN management — rather than user inboxes. In
mid-2024 it exploited a zero-day in Versa Director, CVE-2024-39717, tracked by MITRE as campaign
C0039, dropping a custom web shell (VersaMem) whose purpose was credential capture from managed
service providers and ISPs, to pivot into their downstream customers [3][9]. MITRE also records a
separate initial-access cluster, SYLVANITE, that exploits edge devices and hands established
footholds to Volt Typhoon for follow-on operations [3][7].

**Persistence and credential theft.** Persistence leans on web shells and valid accounts, with at
least one documented use of **SockDetour**, a custom backdoor designed to survive the removal of a
primary access method [10]. The collection priority is credentials and the material needed to
understand and reach the target's operational environment.

## 4. Targeting

CISA's advisory is explicit about sectors — **Communications, Energy, Transportation Systems, and
Water and Wastewater Systems** — across the continental and non-continental United States, including
Guam [1]. Microsoft's tracking adds manufacturing, utilities, construction, maritime, government,
information technology, education, defense and media [4]. OT-focused reporting adds the operational
data itself: geographic information system (GIS) data, OT network diagrams and OT operating
instructions exfiltrated from ICS organisations [7][8]. In 2025, Dragos reported activity reaching
U.S. midstream pipeline operations through compromised Sierra Wireless AirLink cellular gateways,
and electric and oil-and-gas environments [7].

## 5. From access to pre-positioning (2025–2026)

This is the assessment that makes Volt Typhoon different from a data-theft crew, and it is the one
to hold carefully. The U.S. government's stated view is that the PRC is positioning itself to
launch destructive attacks that would threaten physical safety and impede military readiness in a
crisis or conflict [1] — an assessment about **intent and preparation**, not a report of observed
destruction.

What changed in 2025 is that OT reporting moved the activity further along that path. Dragos placed
VOLTZITE at **Stage 2 of the ICS Cyber Kill Chain**: manipulating engineering workstation software
to extract configuration files and alarm data, and specifically investigating what operational
conditions would trigger process shutdowns — reconnaissance of *how to disrupt*, not disruption
itself [7]. Initial access is increasingly brokered: SYLVANITE weaponises edge-device vulnerabilities
and hands footholds over for the deeper OT intrusion [3][7]. No publicly confirmed disruptive effect
has been observed. **Confirmed** here is only that the actor has the access and is collecting the
data; the disruptive end state remains an assessed intent [1][7].

## 6. Disruption and current status

- **KV Botnet disrupted.** Following a December 2023 court-authorised operation, the FBI announced on
  31 January 2024 that it had remotely removed the KV Botnet malware from hundreds of U.S.-based
  SOHO routers and severed their command-and-control [6].
- **Temporary, and replaced.** The FBI noted the remediation was temporary — a router reboot could
  reverse it — and that the devices remained vulnerable [6]. Dragos observed the botnet being
  rebuilt and successor infrastructure in use afterwards [7][8].
- **Not deterred.** Activity continued through 2025 and, by OT reporting, deepened [7].
- Assessment: **infrastructure degraded, intent unchanged**. Takedowns cost the actor proxies, not
  access to its objectives [6][7].

## 7. What this means for defenders

Ordered by how directly they attack this actor's playbook:

1. **Inventory and patch the edge.** VPN gateways, firewalls and SD-WAN managers are the front door.
   Known-exploited CVEs here, and end-of-life devices, are the single highest-value fix [9].
2. **Assume stolen credentials are in play.** Phishing-resistant MFA, credential hygiene and
   privilege tiering — the actor's whole method depends on valid accounts that look legitimate [1].
3. **Log the identity and remote-access planes.** Authentication, VPN and RDP events, plus process
   telemetry (EDR/Sysmon), because the malicious activity is native behaviour [1].
4. **Hunt the LOTL command lines.** `ntdsutil` credential extraction, `wmic`/`netsh` used by
   unexpected parents, and PowerShell that does not match administrative baselines [1][3].
5. **Watch egress to SOHO and edge ranges.** Encrypted sessions to compromised consumer routers, and
   the non-standard TCP-then-HTTPS pattern MITRE documents for C0039, are high-signal [3][9].
6. **Segment and monitor OT.** Engineering-workstation configuration and alarm data, and cellular
   gateways, are the current collection targets — monitor reads, not just writes [7].
7. **Treat "no alerts" as "no evidence".** For an actor whose method is native tools and valid
   accounts, absence of detections is not absence of the actor [1][3].

## 8. Detection opportunities

What to look for, not a validated ruleset (consistent with the rule-provenance policy I use elsewhere):

- `ntdsutil` invoking `ac i ntds` to create an IFM (Install From Media) copy — credential dumping that
  needs no external tool.
- `wmic`, `netsh` or PowerShell spawned by service, web-server or backup processes rather than an
  interactive admin session.
- A new web shell or unexpected file on an SD-WAN manager, VPN appliance or firewall.
- Long-lived encrypted sessions from a host to consumer/SOHO router address space, and the
  non-standard TCP handshake followed by HTTPS command-and-control from campaign C0039 [3][9].
- On OT networks: reads of GIS data, OT network diagrams, alarm configuration or engineering
  workstation configuration files by accounts outside the engineering baseline [7].

## 9. Intelligence gaps and what I could not verify

- Whether VOLTZITE (Dragos) and Volt Typhoon (Microsoft and the U.S. government) are one actor or an
  overlapping ecosystem; the naming is explicitly "technical overlap", not identity [3][7].
- Unit-level attribution — the claim that the group is run by the PLA Cyberspace Force is public
  reporting, not a government confirmation, and is treated as Context only [11].
- Whether the pre-positioning has translated into any confirmed disruptive or destructive effect.
  None is public; the disruptive end state is an assessed intent [1][7].
- Victim enumeration is inherently incomplete — CISA's own framing is that what has been found is
  likely "the tip of the iceberg" [1].
- Nothing here is validated against private telemetry, and I did not analyse malware samples for this
  profile. This is an open-source assessment, and it is bounded by that.

## Sources

1. CISA, NSA, FBI and international partners — **Joint Cybersecurity Advisory AA24-038A: PRC
   State-Sponsored Actors Compromise and Maintain Persistent Access to U.S. Critical Infrastructure**
   (7 Feb 2024). https://www.cisa.gov/news-events/cybersecurity-advisories/aa24-038a
2. CISA, NSA, FBI and partners — **Joint Cybersecurity Advisory AA23-144A: PRC State-Sponsored
   Cyber Actor Living off the Land to Evade Detection** (24 May 2023).
   https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-144a
3. MITRE ATT&CK — **Group G1017: Volt Typhoon** (v2.0, last modified 31 Jul 2026).
   https://attack.mitre.org/groups/G1017
4. Microsoft Threat Intelligence — **Volt Typhoon (VANGUARD PANDA)**.
   https://www.microsoft.com/en-us/security/security-insider/threat-landscape/volt-typhoon
5. CISA, NSA and partners — **Identifying and Mitigating Living Off the Land Techniques** (joint
   guidance, Feb 2024). https://www.cisa.gov/resources-tools/resources/identifying-and-mitigating-living-land-techniques
6. U.S. Department of Justice — **U.S. Government Disrupts Botnet PRC Used to Conceal Hacking of
   Critical Infrastructure** (31 Jan 2024).
   https://www.justice.gov/opa/pr/us-government-disrupts-botnet-peoples-republic-china-used-conceal-hacking-critical
7. Dragos — **OT/ICS Cybersecurity Report 2026: Year in Review** (17 Feb 2026).
   https://www.dragos.com/resources/press-release/dragos-2026-year-in-review-new-ot-threats-ransomware
8. Dragos — **OT/ICS Cybersecurity Report 2025: Year in Review** (25 Feb 2025).
   https://www.dragos.com/resources/press-release/dragos-reports-ot-ics-cyber-threats-escalate-amid-geopolitical-conflicts-and-increasing-ransomware-attacks
9. MITRE ATT&CK — **Campaign C0039: Versa Director Zero Day Exploitation** (June–August 2024), and
   Lumen Black Lotus Labs' exploitation report.
   https://attack.mitre.org/campaigns/C0039
10. Palo Alto Networks Unit 42 — **Threat Brief: Attacks on Critical Infrastructure Attributed to
    Insidious Taurus (Volt Typhoon)** (Feb 2024). https://unit42.paloaltonetworks.com/volt-typhoon-threat-brief/
11. Public reporting on unit-level attribution of the group (secondary source; treated as Context
    only).

---

*All analysis is open-source. Views are my own.*
