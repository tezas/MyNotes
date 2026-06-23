# 🔎 Stock Screening Queries (Stage 1 — Identify)

> These hand you a **shortlist of ~15–30 names**, not buy signals. Screens are
> backward-looking filters. The real work (moat, management, earnings quality,
> valuation vs fair value) is Stages 2–4 on each survivor.
>
> Profile: **quality-compounder, long-term**. Start loose; tighten if you get
> 200 names, loosen if you get 3. Target a shortlist of ~15–30.

## How to use this file
- **Screener.in** (🇮🇳 India-only): screener.in → "Create a stock screen" → paste query → Run.
  Market cap is in **₹ crore**. Field names autocomplete — if a line errors, delete & retype.
- **Finviz** (🇺🇸 US-only): paste the URL, or Screener → "All filters". Code pattern is
  `fa_<metric>_<o|u><value>` (o=over, u=under). Industry codes occasionally get renamed —
  if a URL errors, pick the sector/industry from the dropdown manually.
- **⚠️ No screener captures everything.** Each sector below lists **"Can't be screened —
  check manually"** items. A screen that passes is necessary, not sufficient.

---

## 0️⃣ GENERIC quality-compounder (sector-agnostic baseline)

Use when you don't have a target sector. **Do NOT use as-is on banks or cyclicals** (see those sections).

**🇮🇳 Screener.in:**
```
Market Capitalization > 2000 AND
Return on capital employed > 15 AND
Return on equity > 15 AND
Debt to equity < 1 AND
Sales growth 5Years > 10 AND
Profit growth 5Years > 10 AND
OPM > 15 AND
Current ratio > 1.5 AND
Price to Earning < 30 AND
Pledged percentage < 1
```

**🇺🇸 Finviz:**
```
https://finviz.com/screener.ashx?v=111&f=cap_midover,fa_curratio_o1.5,fa_debteq_u1,fa_eps5years_o10,fa_grossmargin_o30,fa_opermargin_o15,fa_pe_u30,fa_peg_u2,fa_pfcf_u20,fa_roe_o15,fa_roi_o15,fa_sales5years_o10
```

**Can't be screened — check manually:** OCF ≥ net income (earnings quality), moat durability,
management quality, customer concentration. Free Finviz has no FCF/OCF filter — `P/FCF` is the proxy.

---

## Reference ranges (no single "ideal" — always relative to sector + growth)
- **Cash flow:** OCF ≥ Net Income; FCF positive; FCF margin ~5%+ solid, 10%+ excellent; P/FCF <15–20.
- **P/E bands:** <15 cheap or struggling · 15–25 typical · 25–40 priced for growth · >40 high expectations.
- **PEG = P/E ÷ growth%:** ~1 fair, <1 cheap, >2 expensive for the growth.

---
---

# 🏦 1. BANKS & NBFCs

**Why special:** money is the product (borrow cheap, lend dear). **Normal rules break:**
D/E and ROCE are meaningless (deposits *are* the raw material). Value on **P/B, not P/E**;
profitability via **ROA**; the whole game is **asset quality (NPAs)** and **funding cost (CASA/NIM)**.

**🇮🇳 Screener.in (screen on what's available; verify the rest manually):**
```
Market Capitalization > 5000 AND
Return on assets > 1.2 AND
Return on equity > 15 AND
Net NPA % < 1.5 AND
Price to book value < 4 AND
Profit growth 5Years > 12 AND
Pledged percentage < 1
```

**🇺🇸 Finviz (banks have NO NIM/NPA fields — screen on P/B, ROE, ROA, then check the rest):**
```
https://finviz.com/screener.ashx?v=111&f=cap_midover,fa_pb_u2,fa_roe_o12,fa_roa_o1,sec_financial,ind_banksregional
```
Swap `ind_banksregional` ↔ `ind_banksdiversified` for large/national banks.

**🔑 Multi-filter decision logic (one filter is NOT enough here):**
- **ROA + ROE together:** high ROE *with* low ROA = the bank is just over-leveraged, not great.
  Want **ROA > ~1.2%** *and* **ROE > 15%** — quality, not just leverage.
- **NPA + growth together:** fast profit growth + *rising* NPAs = reckless lending that will
  blow up later. Demand **low NPA AND stable/falling NPA trend** alongside growth.
- **P/B vs ROE:** a high P/B is only justified by a high ROE. Cheap P/B + low ROE + high NPA = value trap.

**Can't be screened — check manually (Screener company page / annual report):**
- **NIM** (net interest margin) trend — the spread; not a reliable Screener query field.
- **CASA ratio** — % low-cost deposits; funding-cost moat. Check on the company page.
- **Provision coverage ratio (PCR)** — how much of bad loans are already buffered.
- **CAR** (capital adequacy) — safety buffer vs regulatory minimum.
- **Slippage / restructured loans** — hidden future NPAs.

**Red flags:** profits rising while NPAs rise; very high ROE driven by thin equity; low CASA;
aggressive unsecured-loan growth.

---

# 💻 2. IT / SOFTWARE SERVICES

**Why special:** asset-light, cash-rich, often net-cash (no debt), export-driven (currency-sensitive).
High ROCE/ROE is normal; the battle is growth + margins + people (attrition).

**🇮🇳 Screener.in:**
```
Market Capitalization > 5000 AND
Return on capital employed > 20 AND
Return on equity > 18 AND
Debt to equity < 0.1 AND
Sales growth 5Years > 10 AND
Profit growth 5Years > 10 AND
OPM > 18 AND
Dividend Payout Ratio > 25 AND
Pledged percentage < 1
```

**🇺🇸 Finviz:**
```
https://finviz.com/screener.ashx?v=111&f=cap_midover,fa_debteq_u0.2,fa_grossmargin_o50,fa_opermargin_o18,fa_roe_o18,fa_sales5years_o10,sec_technology,ind_informationtechnologyservices
```
For product/SaaS use `ind_softwareapplication` or `ind_softwareinfrastructure` (higher margins, higher multiples — loosen P/E).

**🔑 Multi-filter decision logic:**
- **High ROCE + low D/E + high payout** together = a true asset-light cash machine returning capital.
- **Margin + growth together:** falling OPM *with* growth often means buying revenue cheaply (bad);
  want **stable/rising OPM alongside growth**.

**Can't be screened — check manually:** attrition rate, client concentration (top-5 client %),
deal pipeline / TCV, $-revenue growth (strip out currency), exposure to one vertical (e.g. BFSI).

**Red flags:** margin erosion, rising attrition, over-reliance on one client/geography, growth only from acquisitions.

---

# 🛒 3. FMCG / CONSUMER STAPLES (defensive)

**Why special:** stable, brand-driven, low debt, high ROCE, **always looks "expensive" on P/E**
(stability commands a premium — that's normal, don't reject on P/E alone). Volume growth > price growth.

**🇮🇳 Screener.in:**
```
Market Capitalization > 5000 AND
Return on capital employed > 25 AND
Return on equity > 20 AND
Debt to equity < 0.3 AND
OPM > 15 AND
Sales growth 5Years > 8 AND
Profit growth 5Years > 8 AND
Dividend Payout Ratio > 30 AND
Pledged percentage < 1
```
Note: **no P/E cap** — quality FMCG rarely trades below 40 P/E. Filtering on P/E<30 would wrongly exclude the best names.

**🇺🇸 Finviz:**
```
https://finviz.com/screener.ashx?v=111&f=cap_midover,fa_debteq_u0.5,fa_grossmargin_o40,fa_opermargin_o15,fa_roe_o18,fa_div_o2,sec_consumerdefensive
```

**🔑 Multi-filter decision logic:**
- **High ROCE + high gross margin + dividend payout** = the classic compounder fingerprint.
- Judge value on **PEG and P/E-vs-own-history**, never absolute P/E.

**Can't be screened — check manually:** volume vs price-led growth, brand/market-share trend,
distribution reach, rural/monsoon exposure (India), raw-material (commodity) input cost cycle.

**Red flags:** growth purely from price hikes (no volume), market-share loss to challengers, margin squeeze from input costs.

---

# 💊 4. PHARMA / HEALTHCARE

**Why special:** R&D-driven, regulatory-gated (USFDA), mix of stable (domestic/branded) and
volatile (US generics) earnings. One regulatory action can swing a stock.

**🇮🇳 Screener.in:**
```
Market Capitalization > 5000 AND
Return on capital employed > 15 AND
Return on equity > 15 AND
Debt to equity < 0.5 AND
OPM > 18 AND
Sales growth 5Years > 10 AND
Profit growth 5Years > 10 AND
Pledged percentage < 1
```

**🇺🇸 Finviz:**
```
https://finviz.com/screener.ashx?v=111&f=cap_midover,fa_debteq_u0.5,fa_grossmargin_o50,fa_roe_o15,fa_sales5years_o8,sec_healthcare,ind_drugmanufacturersspecialtygeneric
```
Use `ind_drugmanufacturersgeneral` for big pharma.

**🔑 Multi-filter decision logic:**
- Screen for quality (ROCE, margins) but **the moat is the pipeline + approvals — not on any screen.**
- Stable-vs-volatile earnings mix matters more than a single growth number.

**Can't be screened — check manually:** USFDA inspection status / warning letters / import alerts,
R&D as % of sales, ANDA/product pipeline, domestic vs US-generics vs API mix, patent cliffs, pricing pressure.

**Red flags:** USFDA warning letter on a key plant, over-reliance on one molecule/market, declining R&D, pending litigation.

---

# 🚗 5. AUTO / 🏭 INDUSTRIALS / 🔩 CYCLICALS

**Why special:** earnings swing with the economic cycle. **⚠️ Same P/E trap as semis:**
low P/E at the cycle TOP (peak earnings) = danger; high P/E at the BOTTOM = often opportunity.
**Do NOT use a tight P/E cap.** Judge on **mid-cycle (normalized) earnings**.

**🇮🇳 Screener.in (no low-P/E filter; lean on ROCE quality + safe balance sheet):**
```
Market Capitalization > 5000 AND
Return on capital employed > 15 AND
Debt to equity < 0.75 AND
Interest Coverage Ratio > 4 AND
OPM > 10 AND
Sales growth 5Years > 8 AND
Pledged percentage < 1
```

**🇺🇸 Finviz:**
```
https://finviz.com/screener.ashx?v=111&f=cap_midover,fa_debteq_u1,fa_roe_o12,fa_sales5years_o8,sec_consumercyclical,ind_autotruckmanufacturers
```
(Industrials: use `sec_industrials`; pick exact industry from dropdown if the code errors.)

**🔑 Multi-filter decision logic (critical for cyclicals):**
- **D/E + Interest Coverage together:** a cyclical MUST survive the downturn. Want **low D/E AND
  interest coverage > 4×** so fixed interest doesn't sink it when sales drop.
- **ROCE through-cycle:** check ROCE in a *bad* past year, not just the latest boom year.
- **Operating leverage:** high fixed costs amplify both up- and down-swings — size positions smaller.

**Can't be screened — check manually:** where we are in the cycle, capacity utilization,
mid-cycle/normalized earnings, order book, commodity input exposure, EV-transition risk (autos).

**Red flags:** high debt + cyclical demand (deadly combo), buying at peak-margin year, low interest coverage.

---

# 💡 6. UTILITIES / POWER (defensive, capital-heavy, income)

**Why special:** regulated, stable, **high debt is NORMAL** (don't reject on D/E), bought mainly
for **dividends** + steady returns. Low growth by nature.

**🇮🇳 Screener.in (dividend + coverage matter more than growth):**
```
Market Capitalization > 5000 AND
Return on capital employed > 10 AND
Interest Coverage Ratio > 2.5 AND
Dividend Yield > 2 AND
Profit growth 5Years > 5 AND
Pledged percentage < 1
```

**🇺🇸 Finviz:**
```
https://finviz.com/screener.ashx?v=111&f=cap_midover,fa_div_o3,fa_roe_o8,sec_utilities
```

**🔑 Multi-filter decision logic:**
- **Dividend yield + payout + interest coverage together:** a high yield is only safe if payout
  isn't stretched and interest is well covered. High yield + thin coverage = dividend-cut risk.
- Debt is fine **only if interest coverage is comfortable** and the return is regulated/predictable.

**Can't be screened — check manually:** regulatory framework / tariff structure, debt maturity profile,
dividend sustainability, asset base quality.

**Red flags:** dividend yield "too good" (market pricing in a cut), weak interest coverage, regulatory overhang.

---

# 🛢️ 7. ENERGY / OIL & GAS · 🪨 8. METALS & MINING (commodity cyclicals)

**Why special:** earnings driven by **commodity prices** the company can't control — pure cyclicals.
P/E trap applies hard. **Balance-sheet survival** is the whole game (low-cost producer wins the cycle).

**🇮🇳 Screener.in (prioritize survival: low debt, strong coverage; no P/E cap):**
```
Market Capitalization > 5000 AND
Return on capital employed > 12 AND
Debt to equity < 0.75 AND
Interest Coverage Ratio > 4 AND
Free cash flow 3years > 0 AND
Pledged percentage < 1
```

**🇺🇸 Finviz:** Energy → `sec_energy` (e.g. `ind_oilgasintegrated`, `ind_oilgasemp`);
Metals → `sec_basicmaterials`.
```
https://finviz.com/screener.ashx?v=111&f=cap_midover,fa_debteq_u0.75,fa_roe_o10,sec_energy
```

**🔑 Multi-filter decision logic:**
- **Cost position is the moat** — the lowest-cost producer survives when prices crash. Not directly
  screenable; infer from **margins vs peers** at the same commodity price.
- **D/E + interest coverage + positive multi-year FCF** together = it can survive a price trough.

**Can't be screened — check manually:** position on the cost curve, commodity price assumptions,
reserve life (mining), capex cycle, hedging policy, geopolitical/regulatory risk.

**Red flags:** high debt into a price downturn, high-cost producer, buying at peak commodity prices.

---

# 🏢 9. REAL ESTATE / REITs

**Why special:** debt-heavy, cash-flow-lumpy (project cycles); valued on **NAV / pre-sales**, not P/E.
REITs valued on yield + occupancy.

**🇮🇳 Screener.in:**
```
Market Capitalization > 5000 AND
Debt to equity < 1 AND
Return on equity > 12 AND
Interest Coverage Ratio > 3 AND
Pledged percentage < 1
```

**🇺🇸 Finviz (REITs — yield + FFO matter):**
```
https://finviz.com/screener.ashx?v=111&f=cap_midover,fa_div_o3,fa_debteq_u1.5,sec_realestate
```

**🔑 Multi-filter decision logic:** debt + interest coverage together (leverage is inherent — coverage
proves it's safe). For REITs, judge on **yield + payout (FFO) + occupancy**, not earnings.

**Can't be screened — check manually:** pre-sales/bookings trend, inventory & debt maturity, NAV,
execution track record, occupancy/rental escalation (REITs), location quality.

**Red flags:** high debt + slowing pre-sales, stretched payout for a REIT, governance issues (common in RE).

---

# 🧪 10. CHEMICALS / SPECIALTY MANUFACTURING

**Why special:** spectrum from **commodity** (cyclical, thin margin) to **specialty** (sticky, high margin).
The margin tells you which you're holding.

**🇮🇳 Screener.in (specialty-tilted: demand quality margins + capital efficiency):**
```
Market Capitalization > 3000 AND
Return on capital employed > 18 AND
Debt to equity < 0.5 AND
OPM > 15 AND
Sales growth 5Years > 12 AND
Profit growth 5Years > 12 AND
Pledged percentage < 1
```

**🇺🇸 Finviz:** `sec_basicmaterials` → pick `ind_specialtychemicals`.

**🔑 Multi-filter decision logic:** **gross/operating margin separates specialty from commodity** —
high stable OPM + high ROCE = specialty (a moat); low/volatile OPM = commodity (treat as cyclical above).

**Can't be screened — check manually:** customer stickiness, China+1 / import-substitution tailwind,
environmental/regulatory compliance, raw-material backward integration, capex plans.

**Red flags:** margins that swing with commodity prices (it's commodity, not specialty), env. compliance issues.

---

## ⚙️ Quick reference — which lever to pull per sector

| Sector | Profitability | Risk gate | Valuation | P/E cap? |
|---|---|---|---|---|
| Generic | ROCE, ROE | D/E < 1 | P/E, PEG | Yes |
| Banks | ROA, ROE | NPA, CAR | **P/B** | No (use P/B) |
| IT | ROCE, OPM | D/E ~0 | P/E, PEG | Loose |
| FMCG | ROCE, gross margin | low D/E | PEG, P/E-vs-history | **No** |
| Pharma | ROCE, OPM | D/E < 0.5 | P/E, EV/EBITDA | Loose |
| Auto/Cyclical | through-cycle ROCE | **D/E + Int. coverage** | mid-cycle P/E, EV/EBITDA | **No** |
| Utilities | ROCE, yield | **Int. coverage** | yield, P/B | n/a (income) |
| Energy/Metals | ROCE, cost position | **D/E + coverage + FCF** | mid-cycle, P/B | **No** |
| Real estate | ROE, NAV | **D/E + coverage** | NAV / pre-sales / FFO | No |
| Chemicals | ROCE, OPM | D/E < 0.5 | P/E, PEG | Depends (specialty vs commodity) |
| Semiconductors | gross margin, ROE | capex, cycle | EV/EBITDA, normalized P/E | **No** (see below) |

---

# 🔬 11. SEMICONDUCTORS (US) — cyclical, R&D-heavy

```
https://finviz.com/screener.ashx?v=111&f=cap_midover,fa_grossmargin_o40,fa_roe_o15,fa_sales5years_o10,fa_debteq_u1,ind_semiconductors
```

| Code | Filter | Setting |
|---|---|---|
| `ind_semiconductors` | Industry = Semiconductors | the key filter |
| `cap_midover` | Market Cap | Over $2B |
| `fa_grossmargin_o40` | Gross Margin | Over 40% (moat proxy) |
| `fa_roe_o15` | Return on Equity | Over +15% |
| `fa_sales5years_o10` | Sales growth 5Y | Over 10% |
| `fa_debteq_u1` | Debt/Equity | Under 1 |

**⚠️ THE CYCLICAL P/E TRAP:** boom → peak earnings → P/E looks LOW → "cheap" → most DANGEROUS
time to buy. Bust → collapsed earnings → P/E looks HIGH → often the BEST time to buy. Signal is
**inverted** — value on **mid-cycle (normalized) earnings**, use EV/EBITDA & P/FCF. P/E & PEG caps deliberately dropped.

**🔑 Multi-filter logic:** gross margin (moat) + low D/E (survives downturn) + sales growth (real demand).
**Sub-industries:** `ind_semiconductors` (chipmakers) · `ind_semiconductorequipmentmaterials`
(equipment/materials — "picks & shovels," often higher-quality, less cyclical).

**Can't be screened — check manually:** fabless vs IDM vs foundry, process-node leadership, R&D scale,
ecosystem lock-in, customer/end-market concentration, capex intensity, geopolitical/export-control risk.

---

## 🧭 Final reminders
1. **A passing screen is a shortlist, never a verdict** — Stages 2–4 decide.
2. **Match the yardstick to the sector** — using a generic P/E cap on banks/cyclicals selects the wrong stocks.
3. **One filter is rarely enough** — combine profitability + risk + valuation, and read the *combinations*
   (e.g. high ROE *with* low ROA on a bank = leverage, not quality).
4. **Verify the "can't be screened" items manually** — that's where the real edge and the real risks live.
5. Field names/codes drift — if a query errors, use autocomplete (Screener) or the dropdown (Finviz).
