>  🔬 Security Research

Independent cybersecurity research, threat verification, privacy analysis and  regulatory context  

---

## 📁 Labs

Independent threat verification · Privacy analysis · Regulatory context

| # | Research | Type | Status |
|---|---|---|---|
| 01 | BrowserGate — LinkedIn Browser Fingerprinting | Threat Verification + Privacy | ✅ Complete |

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
