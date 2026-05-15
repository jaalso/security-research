>  🔬 Security Research

Independent cybersecurity research, threat verification, privacy analysis and  regulatory context  

---

## 📁 Labs

Independent threat verification · Privacy analysis · Regulatory context

| # | Research | Type | Status |
|---|---|---|---|
| 01 | BrowserGate — LinkedIn Browser Fingerprinting | Threat Verification + Privacy | ✅ Complete |
| 02 | Booking.com Storm-1865 Phishing Triage | Live CTI · Incident Prevention · NCSC Report | ✅ Complete |

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
**Short ummary:**
<br> A relative in CH received three WhatsApp messages (French, German, and English) impersonating a hotel reservation team and demanding to "verify" a booking via a malicious link. Using a structured seven-layer CTI reading workflow plus a triage chain (Inoreader feed search → Google research → URLscan.io → Have I Been Pwned → NCSC.ch advisories), the campaign was identified as Storm-1865's "I Paid Twice" variant — a Russian-origin threat group exploiting Booking.com's April 2026 partner-portal data breach.

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

**Triage Chain:**
Phase 1: Inoreader Feed Search
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
