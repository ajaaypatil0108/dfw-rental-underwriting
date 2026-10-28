# DFW Rental Underwriting Model: Single-Family Rental Screen & 10-Year Pro Forma

A screen-then-underwrite study of the Dallas–Fort Worth single-family rental market, built in Excel and Power Query. The model ranks DFW ZIP codes on rental economics, then underwrites the best conventional pick, ZIP **76110 (Fort Worth)**, over a full 10-year hold. The question it answers is whether buying a rental at today's prices with a 7% mortgage earns a return.

**[View the project website →](https://ajaaypatil0108.github.io/dfw-rental-underwriting/)**  ·  [Read the case study (PDF)](DFW_Rental_Underwriting_Model.pdf)  ·  [Download the workbook (Excel)](DFW_Rental_Underwriting_Model.xlsx)

Ajay Patil · MS Finance, The University of Texas at Dallas · Data: Zillow ZHVI & ZORI (June 2026)

## Key findings

**Verdict: pass at 7%.** Even the strongest risk-adjusted ZIP doesn't clear the bar.

| Metric | Result |
|---|---|
| Entry cap rate | **4.3%** |
| Levered IRR (10-yr) | **1.1%** |
| Unlevered IRR (10-yr) | **5.0%** |
| Equity multiple (shortfalls counted as equity) | **1.10x** |
| Cash-on-cash, Year 1 | **–6.1%** |
| DSCR, Year 1 → Year 10 | **0.71 → 0.93** |
| Year-1 cash flow | **–$3,718** |
| Breakeven rent (Year-1 cash flow = 0) | **~$2,144/mo** vs. $1,744 actual (~23% gap) |

1. **Negative leverage.** The asset yields about 4.3% unlevered, but the debt costs 7%, so borrowing lowers the return. The levered IRR (1.1%) comes in below the unlevered IRR (5.0%).
2. **The mortgage is never covered.** DSCR stays below 1.0 for all ten years, and cash flow is negative every year.
3. **Property tax drives the gap.** At the 2.24% non-homestead rate, Year-1 tax is $4,878. That is about 52% of NOI, and it is the main reason a 9.6% gross yield becomes a 4.3% cap rate.
4. **What would have to change.** Against an 8% levered hurdle, the deal works only if the price falls to about $185K (–15%) at 7%, or to about $196K (–10%) with a 5–6% mortgage.

## What's in this repository

| File | Description |
|---|---|
| [`DFW_Rental_Underwriting_Model.xlsx`](DFW_Rental_Underwriting_Model.xlsx) | The full workbook: ZIP screen, assumptions, sensitivity tables, 360-month amortization, 10-year pro forma, and the Zillow data |
| [`DFW_Rental_Underwriting_Model.pdf`](DFW_Rental_Underwriting_Model.pdf) | 3-page case study covering method, results, and recommendation |
| [`docs/`](docs/) | Source for the project website (GitHub Pages) |

**Workbook sheets**

| Sheet | Purpose |
|---|---|
| `Screen` | Ranks every DFW ZIP on gross yield (70%) and 3-year rent CAGR (30%) |
| `Assumptions` | Named inputs plus two live Data Tables: exit cap × rent growth, and price × mortgage rate |
| `Amortization` | 360-month loan schedule that pays down to $0 |
| `ProForma` | 10-year income, expenses, NOI, debt service, cash flow, exit, IRR, DSCR |
| `ZORI Data`, `ZHVI Data` | Zillow rent and home-value series for the DFW metro, loaded and unpivoted with Power Query |

## How the model works

1. **ETL.** Power Query loads Zillow's national ZHVI (home value) and ZORI (rent) CSVs, filters them to the Dallas–Fort Worth–Arlington metro, and unpivots the wide monthly columns into a long table (ZIP · City · County · Date · Value).
2. **Screen.** 254 ZIPs have home values and 220 have rents. 216 have both, and 166 have the 3-year rent history needed to rank them. The composite score weights gross yield 70% and 3-year rent CAGR 30%.
3. **Deliberate override.** The top three ZIPs (South Dallas and Lancaster, 10–12% "yields") fit the profile of distressed-area risk that the data can't see. I selected 76110 (rank #4), which has a 9.6% yield and the strongest rent growth on the shortlist.
4. **Underwriting.** Gross rent → vacancy → EGI → five escalating expense lines → NOI → debt service → cash flow. The exit applies a terminal cap to forward NOI, net of 7% selling costs and the loan payoff.
5. **Stress testing.** Two Data Tables and a Goal Seek for breakeven rent.

**Base-case assumptions:** $217,756 price (the ZIP's June 2026 ZHVI), 25% down, 3% closing costs, 7.0% 30-year fixed mortgage, $1,744/mo rent, 3% rent growth, 5% vacancy, 2.24% property tax, $2,000/yr insurance, 8% management, 5% maintenance, 5% CapEx reserve, 3% expense growth, 5.0% exit cap, 7% selling costs.

**Assumptions to read with care:**
- **Insurance.** $2,000/yr is below Fort Worth market averages (about $5,500/yr for $300K of dwelling coverage). Each additional $1,000/yr lowers the levered IRR by about 5 points, so a higher figure only strengthens the pass.
- **Exit cap.** The 5.0% exit cap is 70bp above the 4.3% entry cap, which is conservative. Exiting at the entry cap gives a levered IRR of about 5.1%, still below the hurdle.
- **Rent data.** ZORI covers all rental types, including apartments, so actual single-family rents in 76110 may differ.

**Refreshing the data:** the Power Query steps point to local copies of the Zillow CSVs. To refresh, download the current ZHVI and ZORI ZIP-level files from Zillow Research, then update the file path in each query's `Source` step. The workbook already stores the June 2026 data, so every result works without a refresh.

## Sources

- [Zillow Research Data: ZHVI and ZORI](https://www.zillow.com/research/data/)
- [Tarrant Appraisal District: 2024 tax rates](https://el8.tad.org/content/rates/2024TaxRates.pdf)
- [Insurify: Fort Worth homeowners insurance costs](https://insurify.com/homeowners-insurance/texas/fort-worth/)

---

Ajay Patil · MS Finance, The University of Texas at Dallas

*Educational portfolio project, not investment advice.*
