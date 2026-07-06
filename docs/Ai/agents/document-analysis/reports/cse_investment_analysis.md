# CSE Annual Report Investment Analysis

## Goal
Analyze the annual reports inside `cse/` and decide which company appears financially stronger as an investment candidate based only on the supplied annual reports.

## Scope
Companies reviewed:
- `cse/asiri` — Asiri Surgical Hospital PLC
- `cse/Jat` — JAT Holdings PLC

Reports reviewed:
- Asiri: FY2018/19 to FY2024/25
- JAT: FY2022/23 to FY2025/26

## Method used
Following the repo's agent/skill flow:
- **analysis_agent**: extracted financial statement and highlights data from each report
- **comparison_agent**: compared growth, profitability, balance sheet strength, leverage, and cash flow
- **decision_agent**: ranked investability based on the document set only

Primary criteria:
1. Revenue growth consistency
2. Profitability quality
3. Operating cash flow strength
4. Balance sheet / leverage
5. Return metrics and resilience
6. Red flags from management commentary and statements

---

## Company 1: Asiri Surgical Hospital PLC

### What looks good
- Revenue has grown strongly over the multi-year period, from about **Rs. 3.48bn (2019)** to **Rs. 7.18bn (2025)**.
- Total assets increased to **Rs. 14.44bn** in FY2024/25.
- Gross profit improved to **Rs. 3.00bn** in FY2024/25.
- Interest-bearing debt has been coming down from earlier peaks.

### What looks weak / risky
- Profitability is **very volatile**.
  - PAT moved from **Rs. 1.25bn (2022)** to **Rs. 640m (2023)**, then the latest report shows **restated FY2024 PAT of only Rs. 123m**, and FY2025 PAT of **Rs. 374m**.
- FY2025 operating cash flow turned **negative** at about **Rs. -123m**, despite reporting profit.
- FY2025 ROE is only **5.77%** and EPS is **Rs. 0.50**.
- The balance sheet includes a large amount of **loans granted to related parties** (**Rs. 3.74bn** in FY2025 current assets), which reduces balance-sheet quality for an outside investor.
- Working-capital pressure is visible:
  - trade receivables increased
  - other current assets increased sharply
  - operating cash conversion weakened materially
- Comparatives in the FY2024/25 report are marked **restated**, which lowers clean year-to-year comparability.

### Financial snapshot
| Metric | FY2023 | FY2024 (restated in FY2025 report) | FY2025 |
|---|---:|---:|---:|
| Revenue | Rs. 5.63bn | Rs. 6.62bn | Rs. 7.18bn |
| Profit after tax | Rs. 640m | Rs. 123m | Rs. 374m |
| Total assets | Rs. 13.25bn | Rs. 13.05bn | Rs. 14.44bn |
| Total equity | Rs. 6.20bn | Rs. 6.50bn | Rs. 6.89bn |
| Operating cash flow | Rs. 571m | Rs. 246m | **Rs. -123m** |

### Overall status
**Financial status: mixed / weak recovery.**
Asiri shows good top-line growth and market position, but the latest numbers show weak earnings quality, low ROE, and negative operating cash flow. It looks more like a **turnaround / watchlist** candidate than the strongest investment choice right now.

---

## Company 2: JAT Holdings PLC

### What looks good
- Revenue grew from **Rs. 10.17bn (FY2022/23)** to **Rs. 12.64bn (FY2025/26)**.
- Equity expanded from **Rs. 8.63bn** to **Rs. 12.08bn** over the same period.
- FY2025/26 operating cash flow jumped to about **Rs. 1.31bn**, much stronger than earlier years.
- FY2025/26 balance-sheet and liquidity metrics remain solid:
  - **Current ratio 2.19x**
  - **Gearing 21%**
  - **Debt / total assets 0.20x**
  - **Interest cover 5.84x**
- FY2025/26 still delivered strong absolute profits:
  - **PAT Rs. 1.525bn**
  - **ROE 13%**
  - **EPS Rs. 2.92**
- FY2024/25 was especially strong, with PAT of about **Rs. 1.78bn** and ROE of **17%**.

### What looks weak / risky
- FY2025/26 PAT fell **14% YoY** from FY2024/25.
- Debtor days rose to **139 days** and inventory days remained high at **154 days**, so working capital still needs monitoring.
- Dividend per share was cut from **Rs. 0.80** to **Rs. 0.44**.
- The group is expanding internationally and completed the **Mirotone (NZ) acquisition**, so integration and execution risk exists.

### Financial snapshot
| Metric | FY2023 | FY2024 | FY2025 | FY2026 |
|---|---:|---:|---:|---:|
| Revenue | Rs. 10.17bn | Rs. 11.56bn | Rs. 11.62bn | Rs. 12.64bn |
| Profit after tax | Rs. 1.30bn | Rs. 1.02bn | Rs. 1.78bn | Rs. 1.52bn |
| Total assets | Rs. 13.01bn | Rs. 14.45bn | Rs. 16.02bn | Rs. 17.80bn |
| Total equity | Rs. 8.63bn | Rs. 9.12bn | Rs. 10.39bn | Rs. 12.08bn |
| Operating cash flow | Rs. 274m | Rs. 204m | Rs. 327m | **Rs. 1.31bn** |

### Overall status
**Financial status: strong.**
JAT looks financially healthier than Asiri in the supplied reports. It has stronger profitability, better ROE, better liquidity, cleaner leverage metrics, and much stronger latest operating cash flow.

---

## Direct comparison

| Area | Asiri | JAT | Better now |
|---|---|---|---|
| Revenue growth | Strong | Strong | Tie |
| Profit stability | Weak / volatile | Better | **JAT** |
| Latest ROE | 5.77% | 13% | **JAT** |
| Latest operating cash flow | Negative | Strong positive | **JAT** |
| Leverage / liquidity | Improving but still pressured | Stronger | **JAT** |
| Earnings quality | Questionable in latest year | Better | **JAT** |
| Red flags | Restatements, negative OCF, related-party loans | Working capital + acquisition risk | **JAT** |

---

## Investment decision

### Best candidate from the supplied reports
**JAT Holdings PLC** appears to be the better investment candidate.

### Why JAT ranks first
- stronger and more scalable earnings base
- better return metrics
- stronger latest cash generation
- healthier leverage and liquidity profile
- less severe red flags than Asiri

### View on Asiri
Asiri is **not a clear reject**, but it does **not** look like the best choice today. It needs to prove:
- sustained profit recovery
- positive operating cash flow
- better returns on equity
- improved balance-sheet quality

---

## Final ranking
1. **JAT Holdings PLC** — preferred investment candidate
2. **Asiri Surgical Hospital PLC** — watchlist / higher-caution candidate

---

## Confidence
**Moderate**

### Why not high confidence
- Only annual reports were used
- No market valuation comparison outside report disclosures
- No quarterly updates, sector multiples, or recent newsflow beyond the reports
- Asiri includes restated comparatives, which affects trend clarity

---

## Important limitation
This is a **document-based investment screen**, not personalized financial advice. Before buying, you should still check:
- latest quarterly results
- current share price vs earnings / book value
- sector outlook
- debt maturity profile
- any post-report material events

---

## Key evidence used
- `cse/asiri/513_1764148122811.pdf`
  - Financial highlights: annual report page 5
  - Statement of profit or loss: annual report page 70
  - Statement of financial position: annual report page 72
  - Cash flow: annual report page 75
- `cse/asiri/513_1693821538929.pdf`
  - Income statement: annual report page 45
  - Financial position: annual report page 47
  - Cash flow: annual report page 50
- `cse/Jat/2353_1780650899827.pdf`
  - Financial highlights: annual report page 16
  - Statement of profit or loss: annual report page 241
  - Statement of financial position: annual report page 242
  - Cash flow: annual report page 245
- `cse/Jat/2353_1749123203864.pdf`
  - Performance highlights: annual report page 28
  - Statement of profit or loss: annual report page 247
  - Statement of financial position: annual report page 248
  - Cash flow: annual report page 251
