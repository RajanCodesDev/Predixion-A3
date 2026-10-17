# Management Memo — Dialer Campaign Analytics (August 2026)

**To:** Head of Collections
**From:** Dialer Analytics — Assignment 2 Part B
**Data:** `accounts.csv` (20,000 accounts) × `calls.csv` (70,235 dials), Aug 1–30 2026. All *observed* figures below come from `analysis.ipynb` (executed top-to-bottom); all *projected* figures are clearly labelled with assumptions.

## Executive summary

Overall connection rate was **25.66%** (18,022/70,235). Three levers stand out, and one severe data-integrity problem: (1) connection yield varies strongly by time of day, (2) promise-keeping depends heavily on how fast the customer must pay, and (3) a single dialer-list slice — **batch B7 in Bihar — reached a wrong party on 53.5% of answered calls vs 7.8% everywhere else**, wasting roughly 100 answered calls in the month. The three actions below are estimated (under stated assumptions) to add **roughly 200 kept promises per month (197–201 across the three estimates)** against the 1,766 kept in August.

## Key findings (observed)

- **Connection timing:** 10–11h ≈ 31.7–32.0%, 17–18h ≈ 33.3–33.6%, vs 21.64% (6,937/32,054) across 12–16h. Peak windows (10–12h + 17–18h) average **32.64%** (8,259/25,306) — ~11 pp above midday.
- **Weekend vs weekday:** **29.27%** (6,869/23,470) vs **23.85%** (11,153/46,765), +5.4 pp.
- **PTP conversion (per right-party contact):** overall **23.57%** (3,892/16,516), falling monotonically with delinquency: 28.11% (1,151/4,094) at DPD 1–30 → 19.15% (786/4,104) at DPD 91–120.
- **Kept-PTP:** overall **45.38%** (1,766/3,892). Promises due in ≤5 days are kept **64.90%** (908/1,399) of the time vs **34.42%** (858/2,493) for >5 days — a **30.5 pp** gap (relative risk 1.89×).
- **Strongest anomaly:** batch **B7 × Bihar**: wrong-number rate among connected calls **53.52%** (114/213) vs **7.82%** (1,392/17,809) for the rest of the data — **+45.7 pp, 6.85× relative risk**, z = 24.0, p ≈ 7×10⁻¹²⁷. The gap holds against the same batch elsewhere (+46.9 pp) and the same state elsewhere (+45.0 pp), so it is specific to that list slice, not to B7 or Bihar generally. The slice's RPC yield collapsed to 46.5% of answered calls vs ~92% elsewhere.

## Recommendations (prioritized)

### R1 — Quarantine and re-verify the B7 × Bihar list slice (highest priority)
**Evidence (observed):** 114 of 213 answered calls reached the wrong party; at the rest-of-B7 baseline rate (6.62%) only ~14 would be expected → **~100 wasted answered calls in one month**. Slice RPC yield 46.48% vs 93.38% (rest of B7).
**Estimated impact (projected):** if corrected/re-sourced numbers behave like the rest of batch B7, the slice recovers ~100 RPCs → **~22–30 additional PTPs/month** (100 × 21.8%–30.3% PTP-per-RPC) → **~10–14 kept PTPs/month** (× observed 45.4% keep rate).
**Assumptions:** cause is list quality (stale/recycled numbers); re-dialed numbers convert like the rest of B7; dialer capacity unchanged. **Limitations:** one month of data; the slice's own PTP-per-RPC rests on only 99 RPCs; monetary value cannot be computed — `outstanding` is balance, not recovery amount. Needed: vendor/list provenance and per-account recovery values.
**Monitor:** weekly wrong-number rate among connected calls for B7-BR with numerator/denominator shown (target <10%), plus RPC share of connected (target ≥90%).

### R2 — Rebalance dialing into 10–12h, 17–18h and weekend hours
**Evidence (observed):** peak windows convert at 32.64% vs 21.64% midday; weekends at 29.27% vs weekdays 23.85%.
**Estimated impact (projected):** shifting **10% of midday volume (≈3,205 dials)** to peak windows adds ≈352 connected calls (3,205 × (32.64% − 21.64%)) → **≈76 PTPs** (× observed 21.6% PTP-per-connected) → **≈35 kept PTPs/month**.
**Assumptions:** the rate gap is causal and stable; agent/dialer capacity exists in peak slots; the shifted accounts convert like those currently dialed midday. **Limitations:** in this dataset the time-of-day effect is built into the generator — in production, run a randomized schedule test before claiming causality; peak-hour staffing/overtime costs are not in the data.
**Monitor:** weekly connection rate by hour band and day type, with dial counts; track peak-window capacity utilization.

### R3 — Target promise deadlines of ≤5 days
**Evidence (observed):** kept rate 64.90% (908/1,399) for ≤5-day promises vs 34.42% (858/2,493) beyond, +30.5 pp.
**Estimated impact (projected):** moving **20% of >5-day promises (≈499/month)** to ≤5 days adds ≈152 kept PTPs/month (499 × 30.5 pp) — the largest single lever.
**Assumptions:** customers accept shorter deadlines without fewer PTPs overall; the keep-rate gap is causal. **Limitations:** correlational — short-deadline customers may differ systematically; a randomized deadline-offer test is needed before rollout; value per kept PTP is not in the data.
**Monitor:** weekly kept-PTP rate by days-to-promise bucket, plus overall PTP incidence to guard against cannibalization.

*Impact figures are projections based on observed August 2026 rates held constant; they are planning estimates, not commitments, and each depends on the assumptions above.*
