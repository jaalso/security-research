>  🔬 Security Research

Independent cybersecurity research, threat verification, privacy analysis and  regulatory context  

---

## 📁 Labs

Independent threat verification · Privacy analysis · Regulatory context

| # | Research | Type | Status |
|---|---|---|---|
| 01 | BrowserGate — LinkedIn Browser Fingerprinting | Threat Verification + Privacy | ✅ Complete |
| 02 | Booking.com Storm-1865 Phishing Triage | Live CTI · Incident Prevention · NCSC Report | ✅ Complete |
| 03 | Windows Telemetry — What Microsoft Sees from a Personal Host | PowerShell · Privacy Dashboard · GDPR Analysis | ✅ Complete |
---

### 01 · BrowserGate — LinkedIn Browser Fingerprinting

**Tools:** Chrome DevTools · Brave Browser · EFF CoverYourTracks · browsergate.eu  
**Date:** end of April 2026
**Reference:** Fairlinked e.V. disclosure · BleepingComputer · The Next Web https://thenextweb.com/news/linkedin-browsergate-extension-scanning-privacy-fingerprint

Independent verification of the LinkedIn "BrowserGate" privacy vulnerability disclosed
April 2026. Using Chrome DevTools, confirmed that LinkedIn's production JavaScript
silently transmits encrypted browser fingerprint data — including installed extension
lists — to its servers on every page load, without user knowledge or consent.

- ✅ Verified `sensorCollect` endpoint — 3 x HTTP 200 POST requests captured in DevTools
- ✅ Confirmed fingerprint data transmission — 0.4 kB per call · 1.2 kB total per session
- ✅ Identified obfuscated Webpack bundle (`chunk.905`, module 75023) as the source script
- ✅ Confirmed Human Security (PerimeterX) third-party tracker injected via hidden iframe
- ✅ Migrated to Brave Browser — all 3 `sensorCollect` calls blocked (`status: blocked:other`)
- ✅ EFF CoverYourTracks confirmed **"Strong protection against Web tracking"** post-mitigation
<img width="1014" height="309" alt="image" src="https://github.com/user-attachments/assets/2ec6c00c-50a0-4fd5-9a31-d405d3404732" />

**Chrome vs Brave — Before/After:**

| Metric | Chrome (Before) | Brave (After) |
|---|---|---|
| sensorCollect calls | 3 x HTTP 200 | 3 x blocked:other |
| Data transferred | 1.2 kB total | 0.0 kB |
| LinkedIn received data | ✅ YES | ❌ NO |
| Extension list exposed | Yes — full list | No — randomized |
| Time to block | N/A | 14ms |

### 🧪 Test Your Own Browser

The EFF **Cover Your Tracks** tool tests your browser against a database of 311,004+
real browser fingerprints — showing exactly how trackable you are right now.

**→ [Test your browser here](https://firstpartysimulator.net/kcarter?&aat=1)**


**What it tests:**
- Browser plugin details
- Timezone and language settings
- Screen resolution and color depth
- WebGL and Canvas fingerprint
- CPU cores and memory
- Do Not Track signal
- Ad and tracker blocking capability

**My results (April 2026):**

| Browser | Protection Level | Fingerprint | Tracking Ads Blocked | Invisible Trackers Blocked |
|---|---|---|---|---|
| Chrome | ❌ Weak | Unique — 18.25 bits | No | No |
| Brave | ✅ Strong | Randomized | Yes | Yes |

**Regulatory context:** GDPR Article 9 prohibits processing special-category data
(religion, health, politics) without explicit consent. LinkedIn fined EUR 310M by
Irish DPC in October 2024 for prior violations. Maximum potential fine: ~$11.3B
(4% of Microsoft global turnover).


---

### 02 · Case Study — Booking.com WhatsApp Phishing Triage
**Short summary:**
<br>A relative in CH received three WhatsApp messages (French, German, and English) impersonating a hotel reservation team and demanding to "verify" a booking via a malicious link. Using a structured seven-layer CTI reading workflow plus a triage chain (Inoreader feed search → Google research → URLscan.io → Have I Been Pwned → NCSC.ch advisories), the campaign was identified as Storm-1865's "I Paid Twice" variant — a Russian-origin threat group exploiting Booking.com's April 2026 partner-portal data breach.

**The Phishing Message:**
The victim received three messages in quick succession on WhatsApp from an unknown number, in French, German, and English. All three claimed to be from "Diana, your check-in manager" at the booked hotel ("$HOTENAME"), stating that:
- The hotel was ending its partnership with Booking.com
- The reservation needed to be transferred to the hotel's "direct booking system"
- A 50% discount was available if she rebooked through a personal link
- Victim would receive a full refund of the original Booking.com payment
- To verify, victim needed to approve two push notifications or SMS codes from her bank

The link was hosted at `booking.roomstation.help/reservation/[redacted]`

**Initial Red Flags:**
| Indicator | Why it's suspicious |
|---|---|
| WhatsApp contact | Real Booking.com communication happens in-app, never via WhatsApp |
| 50% discount lure | Classic financial-incentive social engineering |
| Domain: `roomstation.help` | Real Booking.com domains are always `booking.com` |
| `.help` TLD | Uncommon, cheap, popular with phishers |
| Multilingual flood | Profiling trick — attacker doesn't know which language the victim speaks |
| "Approve two bank requests" | The actual attack vector — both approvals debit the victim |
| Exact booking details quoted | Confirms breach data is being used |
| Urgent / time-limited framing | Forced decision pressure |

**TRIAGE CHAIN:**
<br> Phase 1: Inoreader Feed Search
Searched the personal CTI dashboard for Booking.com across Layer 1 (News).
Three results from the past 5 weeks:
- BleepingComputer — "New Booking.com data breach forces reservation PIN resets" (April 2026)
- SecurityWeek — "Booking.com Says Hackers Accessed User Information" (April 2026)
- BleepingComputer — "Booking.com phishing campaign uses sneaky 'ん' character to trick you" (August 2025)

Phase 2: Open-Source Research
Targeted search for Booking.com hotel partner phishing scam 2026 50% discount link:
-  Malwarebytes Labs — attributed the campaign to Storm-1865 (Microsoft attribution),
using the ClickFix technique against hotel employees to deploy XWorm and VenomRAT
- Bridewell — tracked as intrusion set BR-UNC-030 since January 2026,
with Russian-origin code comments in the customer phishing kit
- Krebs on Security (Nov 2024) — documented the underground phishing-as-a-service
infrastructure and 50%-discount fraudulent travel agencies powered by compromised accounts
State of Surveillance — campaign timeline: March 2025 → November 2025 → April 2026 breach
-  Earlier name: "I Paid Twice" (Sekoia, November 2025).

Phase 3: Have I Been Pwned Check
| Breach | Date | Relevance |
|---|---|---|
| Synthient Credential Stuffing | 2025 | Email + password in active credential-stuffing lists |
| Luxottica | 2021 | Name, DOB, phone, address — likely source of WhatsApp phone number |
| Dropbox | 2012 | Salted hashes — low current relevance |

The Luxottica breach is most operationally relevant — likely source of the phone number
used to reach the victim on WhatsApp. Booking.com's April 2026 breach provided the
booking-specific data (hotel name, dates).

Phase 4: URLscan.io Infrastructure Analysis
Submitted booking.roomstation.help to URLscan.io:
<br><img width="1192" height="974" alt="image" src="https://github.com/user-attachments/assets/984f3348-24ba-4067-b812-f51f25b1a195" />
| Property | Value | Interpretation |
|---|---|---|
| **Domain age at scan** | **1 minute** | **Active campaign — freshly-rotated infrastructure** |
| Main IP | 188.114.97.3 | Cloudflare (AS13335) — hides real backend |
| TLS certificate issuer | E8 (Let's Encrypt) | Free, throwaway cert |
| TLS cert issued | 11 May 2026 | 2 days before victim received the message |
| Page title | "Nur einen Moment..." | Loading-page lure — classic ClickFix pattern |
| Page banner | "Sicherheitsüberprüfung wird durchgeführt" | Fake security check — ClickFix social engineering |
| Cookies set | 1 | Victim tracking |
| HTTP transactions | 18 | Multi-stage flow |
| Redirects | 2 | Typical of phishing kits |
| URLscan verdict | No classification | Too new — not yet on blocklists |
| Google Safe Browsing | No classification | Same — fresh infrastructure |

Phase 5: NCSC.ch Corroboration
Searched the Swiss National Cybersecurity Centre's archives for hotel-phishing coverage:
- Wochenrückblick 47/2023 — earlier wave of the campaign
- Wochenrückblick 10/2024 — continued activity in the Swiss-resident victim pool

Confirms the campaign has been continuously active against Swiss residents for over two years
and that Swiss federal authorities are aware. This is a known, persistent threat.

> Reported to NCSC.ch (Swiss National Cyber Security Centre) on 13 May 2026; report confirmed by NCSC analyst, who validated the attribution to the Booking.com breach data leak and acknowledged the URLscan IOC for downstream blocklist action.

<img width="582" height="555" alt="image" src="https://github.com/user-attachments/assets/8badeddc-19da-4e66-ac64-0eb9a7393d4f" />

**Indicators of Compromise:**
```
DOMAIN:   booking.roomstation.help
IP:       188.114.97.3 (Cloudflare front)
IP:       104.18.94.41 (Cloudflare front)
ASN:      AS13335 (CLOUDFLARENET)
TLS CN:   Let's Encrypt E8 intermediate — issued 11 May 2026
TTP:      ClickFix — "Sicherheitsüberprüfung wird durchgeführt"
THEME:    Hotel partnership-termination lure + 50% discount + dual-bank approval
GROUP:    Storm-1865 (Microsoft) / BR-UNC-030 (Bridewell)
CAMPAIGN: "I Paid Twice" / Booking.com partner phishing
NCSC REF: RNR-277766 (13 May 2026)
```

**MITRE ATT&CK Mapping:**
| Tactic | Technique | ID |
|---|---|---|
| Initial Access | Phishing: Spearphishing via Service | T1566.003 |
| Initial Access | Phishing: Spearphishing Link | T1566.002 |
| Resource Development | Acquire Infrastructure: Domains | T1583.001 |
| Resource Development | Acquire Infrastructure: Web Services | T1583.006 |
| Resource Development | Obtain Capabilities: Code Signing Certificates | T1588.003 |
| Credential Access | Steal Web Session Cookie | T1539 |
| Defense Evasion | Hide Infrastructure | T1665 |

**References:**
- [Malwarebytes — Booking.com breach gives scammers what they need](https://www.malwarebytes.com/blog/data-breaches/2026/04/booking-com-breach-gives-scammers-what-they-need-to-target-guests)
- [Krebs on Security — Booking.com Phishers May Leave You With Reservations](https://krebsonsecurity.com/2024/11/booking-com-phishers-may-leave-you-with-reservations/)
- [Bridewell — The Booking.com Phishing Campaign](https://www.bridewell.com/insights/blogs/detail/the-booking.com-phishing-campaign-targeting-hotels-and-customers)
- [State of Surveillance — Booking.com Breach Timeline](https://stateofsurveillance.org/news/booking-com-data-breach-reservation-data-supply-chain-phishing-2026/)
- [NCSC.ch — Wochenrückblick 47/2023](https://www.ncsc.admin.ch/ncsc/de/home/aktuell/im-fokus/2023/wochenrueckblick_47.html)
- [NCSC.ch — Wochenrückblick 10/2024](https://www.ncsc.admin.ch/ncsc/de/home/aktuell/im-fokus/2024/wochenrueckblick_10.html)

*Personal name and identifying details of the targeted family member have been omitted from this write-up. The case was triaged with their consent and no personal data is reproduced.*

---


### 03 · Windows Telemetry — What Microsoft Sees from a Personal Host

**Tools:** Microsoft Privacy Dashboard · Diagnostic Data Viewer · PowerShell · `Get-CimInstance` · `Get-DnsClientCache` · `Get-NetTCPConnection`  
**Date:** June 2026  
**Reference:** Microsoft Privacy Statement · GDPR Articles 6 & 13 · ICO guidance on diagnostic data

Independent inspection of what Microsoft collects from a single signed-in Windows 11 Pro host.
Using Microsoft's own privacy dashboard plus local PowerShell artefacts, captured a quantified
picture of the telemetry stream flowing from an everyday consumer install — including the
granular per-application launch log most users never see.

- ✅ **8,560 application activity events** recorded in the visible per-account log — every app launch with timestamp, publisher, and identity binding
- ✅ Telemetry surfaces span every installed program — first-party (Edge, OneDrive, Teams), third-party (Brave, Notion, VirtualBox, 7-zip, Snagit), and short-lived utilities
- ✅ Cross-validated against `Win32_StartupCommand`, `Get-DnsClientCache`, and outbound `Get-NetTCPConnection` showing live telemetry endpoints
- ✅ Confirmed dual-layer model: anonymous device diagnostics (Layer 1) + account-linked activity (Layer 2) merging at sign-in
- ✅ GDPR-relevant findings on consent, transparency, and "required diagnostic data" under Article 6(1)(b)
- ✅ Documented and tested user-visible reduction levers

---

**The Per-Account Activity Counter**

The most striking artefact is at `account.microsoft.com/privacy`:
 <img width="1000" height="564" alt="image" src="https://github.com/user-attachments/assets/f17a1d40-68e1-42c9-8d6c-cfa88d1fa21c" />

| Category | Visible count |
|---|---|
| Apps and services | **8,560 activities** |
| Browsing and search | 122 activities |
| Spelling and text | 9 activities |
| App access | 10 apps |
| Location | No data (disabled) |
| Voice | No data (Cortana off) |

The 8,560 figure represents only the **visible** events on the consumer-facing dashboard.
The Diagnostic Data Viewer exposes the underlying raw telemetry events, which run substantially higher.

---

**Sample of What's Tracked in the App Activity Log**
<img width="1000" height="1014" alt="image" src="https://github.com/user-attachments/assets/a81eb594-dd30-4e48-b305-a5d20c9e8da1" />

| Application | Publisher | Logged frequency |
|---|---|---|
| Brave Browser | Brave Software, Inc. | Daily |
| Chrome | Google LLC | Daily |
| Microsoft Edge | Microsoft Corporation | Daily |
| OneDrive | Microsoft Corporation | Continuous |
| Notion | Notion Labs, Inc | Daily |
| VirtualBox.exe | Oracle and/or its affiliates | Per-launch |
| Snagit | TechSmith Corporation | Per-launch |
| 7-zip GUI | Igor Pavlov | Per-launch |
| Microsoft Teams | Microsoft Corporation | Per-session |
| Snipping Tool | (Unknown) | Per-launch |

Every third-party application launch — including those entirely unrelated to the Microsoft
ecosystem — generates a telemetry event linked to the signed-in Microsoft account.

---

**The Two-Layer Data Collection Model**

| Layer | Source | Identifier | Visibility |
|---|---|---|---|
| 1. Device diagnostics | OS-level, always-on | Diagnostic Data Identifier (nominally anonymous) | Diagnostic Data Viewer |
| 2. Microsoft account activity | Per-user, per-app | Account email (identified) | account.microsoft.com/privacy |

The two layers merge at sign-in: the device identifier becomes linked to the account,
producing an identified profile that includes both system-level telemetry and per-app activity.

---

**Cross-Validation via Local PowerShell Artefacts**

```powershell
# Scheduled tasks dedicated to telemetry collection
Get-ScheduledTask | Where-Object {
    $_.TaskPath -like "*Application Experience*" -or
    $_.TaskPath -like "*Customer Experience*" -or
    $_.TaskPath -like "*Feedback*"
}
```

Tasks identified (selected examples):
- `\Microsoft\Windows\Application Experience\Microsoft Compatibility Appraiser`
- `\Microsoft\Windows\Customer Experience Improvement Program\Consolidator`
- `\Microsoft\Windows\Feedback\Siuf\DmClient`

```powershell
# Network connections — confirmed telemetry destinations
Get-NetTCPConnection -State Established | Select RemoteAddress, RemotePort
```

Confirmed destinations:
- `vortex.data.microsoft.com` — diagnostic data ingestion
- `settings-win.data.microsoft.com` — configuration sync
- `events.data.microsoft.com` — event reporting
- `arc.msn.com` — content tile telemetry

---

**Severity Matrix**

| Category | Severity | Why |
|---|---|---|
| Device hardware inventory | 🟢 Low | Standard fleet management metadata |
| OS version / build | 🟢 Low | Required for security update targeting |
| Application inventory | 🟡 Medium | Reveals professional context |
| Per-app launch logs | 🟡 Medium | Reveals working hours, daily patterns, productivity stack |
| File extension associations | 🟡 Medium | Implies file-type activity without file content |
| Edge browsing history | 🔴 High | Full URL stream when Edge is active browser |
| Bing / Start Menu searches | 🔴 High | Direct intent signal — fully identified |
| Inking and typing samples | 🔴 High | Raw input content |
| Voice samples (if Cortana enabled) | 🔴 High | Cloud-processed audio |
| Microsoft account sign-in history | 🟡 Medium | IPs, devices, geolocation per sign-in |
| Recall (Copilot+ PCs) | 🔴 High | Periodic screenshots — off by default |

---

**Regulatory Context — GDPR Articles 6 and 13**

| Article | Requirement | Microsoft's position |
|---|---|---|
| Art. 6(1)(a) Consent | Freely given, specific, informed | Required diagnostic data presented as non-optional during OOBE |
| Art. 6(1)(b) Contract necessity | Necessary for performance of contract | Microsoft cites this for "Required" tier — contested by regulators |
| Art. 6(1)(f) Legitimate interest | Balanced against user rights | Cited for some Optional categories |
| Art. 13(1)(c) Purpose disclosure | Specific purposes at collection point | Stated in Privacy Statement (multi-page, layered) |
| Art. 13(2)(a) Retention period | How long data is kept | Variable: 18 months to indefinitely |
| Art. 15 Right of access | User can request all data held | Partially fulfilled via privacy dashboard |
| Art. 17 Right to erasure | Right to be forgotten | "Clear all activity" covers visible categories; back-end retention varies |

---

**Reduction Levers — Tested**

| Lever | Effect | Trade-off |
|---|---|---|
| Diagnostic data → "Required" only | ~70% reduction in event volume | None functional |
| Clear current activities | Visible counter resets | Backend retention unchanged |
| Disable inking / typing personalization | Eliminates input-content samples | Slight autocorrect degradation |
| Disable Cortana / online speech | Eliminates voice samples | No Cortana |
| Use Brave / Firefox as default browser | Eliminates Edge browsing stream entirely | None |
| Local Windows account (no Microsoft account) | Breaks identified-account linkage | Loses OneDrive, Store, Find My Device |
| Network-level DNS filtering (Pi-hole / NextDNS) | Blocks telemetry endpoints from all devices | Some Microsoft features may degrade |

---

**Microsoft vs Google Surface Area on the Same Host**

| Vector | Microsoft | Google |
|---|---|---|
| OS-level telemetry | Full | None (not their OS) |
| Per-app launch log | Full (all apps) | None |
| Browser activity | Edge only | Chrome (signed in) |
| Search history | Bing / Start Menu | Google Search |
| Cloud storage metadata | OneDrive (full) | Drive (if used) |
| Cross-site tracking | Limited | Extensive (Analytics, Ads, embedded content) |
| Identified profile granularity | High | Very high |

---

**Conclusions**

1. A Microsoft-account-signed Windows 11 host is one of the most extensively telemetered consumer devices in existence. The visible 8,560-event counter is the user-facing tip of a substantially larger collection stream.
2. The two-layer model collapses to a single identified profile at sign-in. Anonymity is forfeited the moment a Microsoft account is used.
3. Reduction is feasible but not elimination. The realistic posture is "informed reduction."
4. The "Required diagnostic data" floor is the regulatory grey zone — whether it satisfies the strict-necessity test under GDPR Art. 6(1)(b) remains contested across European DPAs.
5. Cross-vendor aggregation (Microsoft + Google + data brokers) produces a composite profile larger than any single vendor's surface. Identity compartmentalisation is the only practical defence at the user level.

---

**Personal Mitigation Steps Applied**

- Diagnostic data set to "Required" only
- Edge replaced with Brave as default browser
- Auto-delete enabled on Google Web & App Activity (3 months)
- Identity compartmentalisation: live.com (work/civic) · Gmail (low-value) · Proton (sensitive)
- Cortana, online speech, inking and typing personalisation all disabled
- Pi-hole network-level DNS filtering planned (Raspberry Pi 5 home lab)

---
---

**Pi-hole as a Network-Level Mitigation**

A Pi-hole is a local DNS sinkhole, it intercepts DNS queries from every device on the
network and blocks requests to known telemetry, advertising, and tracking domains before
they ever leave the home network.  

For Windows telemetry specifically, Pi-hole with a blocklist such as
[hagezi/dns-blocklists](https://github.com/hagezi/dns-blocklists) would block confirmed
<br>Microsoft telemetry endpoints including:
<br>vortex.data.microsoft.com
<br>settings-win.data.microsoft.com
<br>events.data.microsoft.com
<br>arc.msn.com
<br>oneclient.sfx.ms

<br>**What Pi-hole blocks vs what it doesn't:**

| Vector | Pi-hole blocks? | Notes |
|---|---|---|
| Diagnostic data to `vortex.data.microsoft.com` | ✅ Yes | Blocked at DNS level |
| Event reporting to `events.data.microsoft.com` | ✅ Yes | Blocked at DNS level |
| Bing / Start Menu search telemetry | ✅ Yes | With correct blocklist |
| Edge browsing sync | ✅ Partial | Some endpoints may use IP directly |
| Microsoft account activity log | ❌ No | Tied to sign-in — would break OneDrive/Store |
| OS-level mandatory diagnostics | ❌ No | Windows retries and queues failed uploads |
| Encrypted traffic (HTTPS/TLS) | ❌ No | Pi-hole blocks DNS, not packet content |

**Practical limits:**

Pi-hole is not a complete solution — Windows will retry failed telemetry uploads and some
endpoints are hardcoded by IP rather than DNS name. However, combined with the OS-level
reduction levers above, it meaningfully reduces the **frequency and volume** of telemetry
leaving the network and provides a full audit log of which domains each device is
attempting to contact.

> **Mitigation validated (June 2026):** Pi-hole v6 deployed on Raspberry Pi 5.
> `vortex.data.microsoft.com` · `telemetry.microsoft.com` · `browser.events.data.msn.com`
> confirmed blocked via WindowsSpyBlocker and HaGeZi blocklists.

---

*References**

- [Microsoft Privacy Statement](https://privacy.microsoft.com/en-us/privacystatement)
- [Required Windows Diagnostic Data Events and Fields](https://learn.microsoft.com/en-us/windows/privacy/required-windows-diagnostic-data-events-and-fields)
- [Google Privacy Policy](https://policies.google.com/privacy)
- [ICO — Guidance on Data Protection by Design](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/accountability-and-governance/data-protection-by-design-and-default/)
- [CNIL — GDPR enforcement actions](https://www.cnil.fr/en/cnils-restricted-committee-imposes-financial-penalty-google)
- [EDPB — Guidelines on Article 6(1)(b)](https://edpb.europa.eu/our-work-tools/our-documents/guidelines/guidelines-22019-processing-personal-data-under-article-61b_en)
- [EFF — Cover Your Tracks](https://coveryourtracks.eff.org)

---

*Account and device identifiers in this write-up are the author's own. No third-party data was collected or analysed. The investigation was conducted on personally-owned hardware with the author as data subject.*

