# CLAUDE.md — HK Trip 2026

## Project Overview

A self-contained, single-file travel itinerary website for a 7-day Hong Kong / Shenzhen / Guangzhou trip (July 16–22, 2026) departing from Kaohsiung, Taiwan. The UI is in Traditional Chinese (zh-TW). Same architecture as the `seoul-trip-2026` repo.

**Single file**: `index.html` — all HTML, CSS, and JavaScript live here. No build system, no dependencies.

## Architecture

- **Left panel** (`.left`): day-tab navigation (D1–D7) + timeline content, scrollable
- **Right panel** (`.right`): sticky panel with Google Maps iframe and a hand-coded key-routes diagram, `position:sticky;top:0`
- Responsive breakpoint at `900px`: stacks to single column, right panel becomes `380px` fixed height

### Day Color Scheme
```
--d1: #00888A  (teal   — 抵港→深圳水療)
--d2: #C0392B  (red    — 深圳→廣州)
--d3: #EF7C1C  (orange — 廣州老城)
--d4: #00A650  (green  — 足球日)
--d5: #9A3B26  (brown/屯馬線 — 廣州→香港・車公廟)
--d6: #007DC5  (blue   — 香港自由日・書展)
--d7: #D4A017  (gold   — 黃大仙→回程)
```

### Tag Classes
`.t-food` `.t-tp` `.t-act` `.t-hotel` `.t-tip` `.t-shop` plus trip-specific `.t-spa` (teal, 水療) and `.t-temple` (amber, 廟宇).

### Route Diagram (right panel, replaces Seoul's metro diagram)
- Line badge classes: `.line-eal`(東鐵 #53B7E8) `.line-tml`(屯馬 #9A3B26) `.line-ktl`(觀塘 #00AB4E) `.line-ael`(機場快線 #00888A) `.line-hsr`(高鐵 #C0392B) `.line-bus`(#E67E22) `.line-szm`(深圳地鐵 #009943)
- Route step badges: `.rs.eal` `.rs.tml` `.rs.ktl` `.rs.hsr` `.rs.bus` `.rs.walk`
- `.xing` table lists border-crossing opening hours
- Three `.route-box` cards cover the key transfer days (D1 airport→福田口岸, D5 廣州東→車公廟, D7 黃大仙→airport)

## JavaScript Features (same as seoul-trip-2026)

- **Cloud sync (JSONBlob)**: `BLOB = https://jsonblob.com/api/jsonBlob/019f38a2-3ef5-70e0-9b2f-2aae2c14dfd3`, keys `notes` ("1"–"7") and `edits` ("c0"…). Debounced 800ms PUT; localStorage cache key is `hk_cloud`.
- **Edit mode**: FAB toggles `contentEditable` on `.ic-title`/`.ic-desc`, identified by `data-eid`.
- **Day/map switching**: `switchDay(n)` (n = 1–7, `DAYS = 7`), `switchMap('map'|'metro')`.
- **Notes**: injected `<textarea id="noteN">` per day on DOMContentLoaded.

## Trip Reference Data
| Item | Value |
|------|-------|
| Outbound flight | UO133 (HK Express), KHH→HKG, 15:00→16:35, T1, 7/16 |
| Return flight | UO136, HKG→KHH, 12:45→14:15, departs **T2**, 7/22 |
| Baggage | No free checked bag, 7kg carry-on only, counters close 60 min before departure |
| D1 spa (overnight stay) | 湯崎 TENZ 福田口岸店 (backup: 皇室假期, 羅湖) |
| D4 | Football match in Guangzhou, kickoff TBD — user edits time via edit mode |
| D5 temple | 沙田車公廟, MTR 車公廟站 exit B |
| D7 temple | 黃大仙祠, opens 07:30; must board E23 bus by 08:40 (taxi fallback) |
| HK Book Fair | 7/15–21 Wanchai HKCEC, last day 7/21 closes 17:00 |
| Documents | 台胞證 covers both HK entry and mainland China |

## Development Notes
- Open `index.html` directly in a browser — no server required
- Deployed via GitHub Pages (`.github/workflows/deploy.yml`, auto-enables Pages)
- If the JSONBlob ID is rotated, update the `BLOB` constant in the `<script>` block
