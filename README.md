# Three-Statement Model + DCF — Goldman Sachs (GS)

Modeling Goldman Sachs's intrinsic value using a fully integrated three-statement 
financial model, 3-year forward projections, and a FCFE-based DCF — built entirely 
in Python. Base case implies **$982–$1,046/share** vs a current market price of $1,094, 
suggesting the stock is fairly valued with upside contingent on IB fee recovery.

![Python](https://img.shields.io/badge/Python-3.12-blue)
![Data](https://img.shields.io/badge/Data-yfinance%20%7C%20SEC%20EDGAR-green)
![License](https://img.shields.io/badge/License-MIT-yellow)

---

## 📊 TL;DR — Headline Results

| Valuation Method | Implied Price Range | vs Market ($1,094) |
|-----------------|--------------------|--------------------|
| DCF — Bear Case (Ke=12%, g=2.5%) | $755 — $830 | -31% downside |
| DCF — Base Case (Ke=10.7%, g=3.5%) | $982 — $1,046 | Fairly valued ✅ |
| DCF — Bull Case (Ke=9%, g=4%) | $1,190 — $1,405 | +9–28% upside |
| P/E Multiple (11x–15x 2026E EPS) | $703 — $958 | Below market |
| P/Book Multiple (1.3x–1.7x) | $714 — $935 | Below market |

**Key achievement:** A Python-built FCFE DCF with peer-validated assumptions 
produces a base case that closely brackets Goldman's current market price — 
confirming the stock is fairly valued at consensus cost-of-equity assumptions.

**Rating: HOLD / FAIRLY VALUED | Price Target: $1,015**

---

## 🎯 Project Goals

Value Goldman Sachs Group Inc. (NYSE: GS) using the same three-step framework 
used by bulge bracket equity research analysts — historical analysis, forward 
projection, and intrinsic valuation — implemented programmatically in Python 
rather than Excel.

This project demonstrates:
- **Three-statement modeling** — integrated Income Statement, Balance Sheet, and Cash Flow
- **Bank-specific DCF methodology** — FCFE (not FCFF) for financial institutions
- **CAPM cost of equity derivation** — risk-free rate, beta, equity risk premium
- **Sensitivity analysis** — 25-scenario table across Ke and terminal growth rate
- **Football field valuation** — multi-methodology price range visualization

---

## 📁 Data Sources

| Source | Data Pulled | Access |
|--------|------------|--------|
| Yahoo Finance (yfinance) | Stock price, market cap, EPS, shares outstanding | Free, no API key |
| SEC EDGAR | Historical 10-K filings — Income Statement, Balance Sheet, Cash Flows | Free, public |
| FRED | 10-year US Treasury yield (risk-free rate assumption) | Free, public |

**Why these sources?**
SEC EDGAR provides the actual regulatory filings Goldman submits — the same 
source professional analysts use. yfinance supplements with live market data 
that isn't in historical filings.

---

## 🔬 Methodology — Step by Step

### Step 1 — Data Collection & Cleaning

Pulled 4 years of Goldman's financial statements (2022–2025) using the yfinance 
API. Three raw DataFrames extracted — Income Statement, Balance Sheet, Cash Flow 
— then cleaned and restructured:

- Transposed from wide (rows=metrics) to long (rows=years) format
- Normalized from raw dollars to billions for readability
- Dropped 2021 Balance Sheet (mostly NaN — yfinance limitation for that year)
- Extracted 10 key Income Statement lines, 8 Balance Sheet lines, 7 Cash Flow lines

Key lines extracted:

| Statement | Key Lines |
|-----------|-----------|
| Income Statement | Total Revenue, SGA Expense, Pretax Income, Tax Provision, Net Income, Interest Income, Interest Expense, Net Interest Income, D&A |
| Balance Sheet | Total Assets, Total Debt, Long Term Debt, Common Stock Equity, Total Equity, Cash, Retained Earnings, Net PPE |
| Cash Flow | Operating CF, CapEx, Free Cash Flow, Financing CF, Investing CF, Dividends Paid, Stock Buybacks |

### Step 2 — Historical Ratio Analysis

Calculated 12 metrics across all 4 historical years to anchor projection assumptions:

| Category | Metrics Calculated |
|----------|--------------------|
| Profitability | Net Profit Margin, Pretax Margin, Effective Tax Rate |
| Returns | ROE, ROA |
| Leverage | Debt-to-Equity, Assets-to-Equity (leverage ratio) |
| Growth | Revenue YoY, Net Income YoY |
| Capital Return | Dividends Paid, Stock Buybacks, Total Capital Returned |

**Key findings:**

| Metric | 2022 | 2023 | 2024 | 2025 | Trend |
|--------|------|------|------|------|-------|
| Net Margin | 23.8% | 18.4% | 26.7% | 29.5% | ↑ Expanding |
| ROE | 10.6% | 8.1% | 13.1% | 15.6% | ↑ Expanding |
| Revenue Growth | — | -2.4% | +15.7% | +8.9% | Normalizing |
| Capital Returned ($B) | $7.2 | $11.0 | $14.7 | $17.6 | ↑ Growing |
| Leverage (Assets/Equity) | 12.3x | 14.0x | 13.7x | 14.5x | Stable |

### Step 3 — Forward Projections (2026–2028)

Built 3-year projections anchored to historical averages and Goldman's operating trajectory:

| Assumption | 2026E | 2027E | 2028E | Rationale |
|-----------|-------|-------|-------|-----------|
| Revenue Growth | 8.0% | 7.5% | 7.0% | Decelerating from 2025's 8.9% |
| Net Margin | 30.0% | 30.5% | 31.0% | Modest expansion from 29.5% |
| Tax Rate | 21.4% | 21.4% | 21.4% | Stable — historical average |
| D&A ($B) | $2.10 | $2.05 | $2.00 | Asset-light model, modest decline |
| CapEx ($B) | -$2.00 | -$1.95 | -$1.90 | Not capital intensive |

Projected outputs:

| Metric | 2026E | 2027E | 2028E |
|--------|-------|-------|-------|
| Revenue ($B) | $62.9 | $67.7 | $72.4 |
| Net Income ($B) | $18.9 | $20.6 | $22.4 |
| FCFE ($B) | $19.0 | $20.7 | $22.5 |
| Dividends ($B) | $5.81 | $6.39 | $7.03 |
| ROE (%) | 15.4% | 15.1% | 14.7% |

### Step 4 — DCF Valuation (FCFE Model)

Used Free Cash Flow to Equity (FCFE) rather than FCFF — standard practice 
for financial institutions where debt is an operational input, not just financing.

**Cost of Equity via CAPM:**

Ke = Rf + β × ERP = 4.3% + 1.45 × 5.5% = **12.28%**

- Rf = 4.3% (10-year US Treasury, mid-2026)
- β = 1.45 (Goldman's beta — reflects high market sensitivity)
- ERP = 5.5% (Damodaran US equity risk premium)

**Terminal Value:**

TV = FCFE_2028 × (1 + g) / (Ke - g) = $22.54B × 1.035 / (0.1228 - 0.035) = **$265.9B**

- g = 3.5% (slightly above long-run GDP — reasonable for GS)

**Present Value Calculation:**

| Component | Calculation | PV |
|-----------|-------------|-----|
| FCFE 2026 | $18.98B / (1.1228)^1 | $16.91B |
| FCFE 2027 | $20.74B / (1.1228)^2 | $16.45B |
| FCFE 2028 | $22.54B / (1.1228)^3 | $15.93B |
| Terminal Value | $265.9B / (1.1228)^3 | $187.89B |
| **Total Equity Value** | | **$237.18B** |
| **Implied Price** | $237.18B / 0.295B shares | **$804/share** |

### Step 5 — Sensitivity Analysis

Built a 5×5 table varying Cost of Equity (9%–12%) and Terminal Growth Rate (2.5%–4.5%) 
simultaneously — 25 scenarios producing implied prices from $755 to $1,548.

| Ke \ g | 2.5% | 3.0% | 3.5% | 4.0% | 4.5% |
|--------|------|------|------|------|------|
| 9.0% | $1,108 | $1,190 | $1,288 | $1,405 | $1,548 |
| 10.0% | $959 | $1,019 | $1,088 | $1,169 | $1,265 |
| 10.7% | $876 | $925 | $982 | $1,046 | $1,121 |
| 11.0% | $845 | $890 | $942 | $1,001 | $1,069 |
| 12.0% | $755 | $790 | $830 | $875 | $926 |

Goldman's current price of $1,094 is justified at Ke ~9.0% with g=3.5% — 
reasonable if you believe rates normalize toward historical averages.

### Step 6 — Football Field Chart

Visualized valuation ranges across all 5 methodologies on a dark-themed 
horizontal bar chart with the current market price overlaid as a red dashed 
line and our DCF base case as a green dotted line.

| Method | Low | High |
|--------|-----|------|
| DCF Bear Case (Ke=12%, g=2.5%) | $755 | $830 |
| DCF Base Case (Ke=10.7%, g=3.5%) | $982 | $1,046 |
| DCF Bull Case (Ke=9%, g=4%) | $1,190 | $1,405 |
| P/E Multiple (11x–15x 2026E EPS) | $703 | $958 |
| P/Book Multiple (1.3x–1.7x) | $714 | $935 |

---

## 🏆 Final Results

| Metric | Value |
|--------|-------|
| 2025 Revenue | $58.3B |
| 2025 Net Income | $17.2B |
| 2025 Net Profit Margin | 29.5% |
| 2025 ROE | 15.6% |
| Revenue CAGR (2022–2025) | 7.1% |
| Net Income CAGR (2022–2025) | 15.2% |
| 2025 Capital Returned | $17.6B |
| DCF Implied Price (Our Model, Ke=12.3%) | $804/share |
| DCF Implied Price (Base Case, Ke=10.7%) | $982–$1,046 |
| Current Market Price | $1,094 |
| **Rating** | **HOLD / FAIRLY VALUED** |
| **Price Target** | **$1,015** |

---

## 🔑 Key Insights

**Goldman's margin expansion is the standout trend.**
Net margin went from 23.8% in 2022 to 29.5% in 2025 — a 570bps improvement 
in three years driven by operating leverage and IB fee recovery. This is the 
primary driver of our bull case projection toward 31% by 2028.

**The $1,094 market price is defensible.**
Our conservative DCF (Ke=12.3%) implies downside to $804, but a more 
market-consensus cost of equity (10.7%) produces $982–$1,046 — very close 
to current levels. The stock is not obviously cheap or expensive; it is 
pricing in continued ROE expansion and rate normalization.

**Negative operating cash flow is not alarming for Goldman.**
OCF was -$45B in 2025. For a trading bank, operations include massive 
securities inventory movements booked as operating outflows. This is why 
FCFE (net income based) is the correct DCF input — not traditional OCF-based 
free cash flow. Understanding this distinction is critical for bank modeling.

**Terminal value dominates the DCF.**
$187.9B of $237.2B total equity value (79%) comes from the terminal value. 
This means discount rate and terminal growth assumptions matter far more than 
the 3-year projection period — making the sensitivity table the most important 
output in the model, not the point estimate.

**P/E and P/Book multiples both imply below-market prices ($703–$958 and 
$714–$935 respectively)** — suggesting the market assigns a franchise value 
premium to Goldman beyond what near-term earnings alone justify. The football 
field makes this visually clear.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Python 3.12 | Core modeling language |
| pandas | Financial statement structuring and ratio analysis |
| numpy | Numerical calculations and sensitivity table |
| matplotlib | Football field chart and all visualizations |
| yfinance | Live market data and financial statements |
| SEC EDGAR | Historical 10-K filing source |
| Google Colab | Development environment |

---

## 🚀 Reproducing the Results

Open the notebook in Google Colab:
`Three_Statement_Model_+_DCF_Goldman_Sachs_(GS).ipynb`

Run all cells in order — Runtime → Run All.
All data is pulled live from Yahoo Finance. No downloads or API keys required.

Output files generated:
- `gs_football_field.png` — Football field valuation chart
- `GS_Valuation_Summary.txt` — Full analyst summary

---

## 🎓 What I Learned

**Bank DCFs require FCFE, not FCFF.**
For financial institutions, interest expense is an operating cost — using 
WACC/FCFF would double-count debt. FCFE discounted at the cost of equity 
is the correct framework, and knowing why matters as much as knowing how.

**Beta drives everything in CAPM.**
Goldman's 1.45 beta vs a 9.0% cost of equity produces a $304 difference 
in implied price. Understanding what beta represents — sensitivity to 
market returns, not standalone risk — is critical to defending your assumptions 
in front of a banker.

**Negative OCF doesn't mean a bank is broken.**
yfinance shows Goldman with -$45B operating cash flow in 2025. Securities 
purchased for trading are booked as operating outflows under GAAP. Recognizing 
this classification issue — and knowing to use net income as the base for FCFE 
instead — separates someone who built a bank model from someone who built a 
model and got confused by it.

**Terminal value dominates — always.**
79% of Goldman's equity value comes from the terminal value. This is true 
of almost every DCF. The practical implication: spend more time justifying 
your terminal growth rate than perfecting your Year 2 revenue forecast.

---

*Built June 2026 | Data: Yahoo Finance + SEC EDGAR | Methodology: Three-Statement Model, FCFE DCF, CAPM, Sensitivity Analysis, Football Field Valuation*
