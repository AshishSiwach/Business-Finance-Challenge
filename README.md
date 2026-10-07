# Power BI Business-Finance-Challenge Dashboard

## Business Finance Challenge

Power BI financial performance dashboard built on the Onyx Data / DataDNA Dataset Challenge (August 2024) dataset — a full P&L reporting suite for a multi-line consumer goods company, with business-line and monthly breakdowns and a dynamic, driver-level view of revenue, cost and margin.

## Power BI Financial Performance Dashboard
<img width="1915" height="836" alt="image" src="https://github.com/user-attachments/assets/2e785b48-c904-4ccb-9d81-df9016b90491" />

<img width="1910" height="833" alt="image" src="https://github.com/user-attachments/assets/b176e512-8c8c-4ea0-b65d-28541cf71670" />

## Overview

The brief: turn a raw, unpivoted transaction log of monthly revenue and expense entries into a report a finance stakeholder can actually use: a standard P&L statement, broken out by business line, with enough structure to go from "what happened" to "why."

The dataset covers FY2023 (580 transaction-level records, Jan–Dec) across three business lines: Nutrition & Food Supplements, Sports Equipment, and Sportswear. Each record is a single revenue or expense amount, tagged with a business line, a revenue/expense group (Sales, Consulting & Professional Services, Other Income, COGS, OPEX, Interest & Tax), and, for expenses, a subgroup (Labor, Materials, Marketing, Payroll, Rent, etc.).

## Key metrics (FY2023)

| Metric | Value |
|---|---|
| Total Revenue | $17.56M |
| COGS | ($6.71M) |
| **Gross Profit** | **$10.85M** (62% margin) |
| OPEX | ($5.60M) |
| **EBIT** | **$5.24M** |
| Interest & Tax | ($0.93M) |
| **Net Profit** | **$4.31M** (25% margin) |

### By business line

| Business Line | Revenue | COGS (% of sales) | Net Profit |
|---|---|---|---|
| Sports Equipment | $8.91M | 52% | $2.29M |
| Sportswear | $6.81M | 40% | $2.74M |
| Nutrition & Food Supplements | $1.84M | 66% | ($0.71M) |

## Key findings

- **Nutrition & Food Supplements is the only loss-making line.** It
  carries the thinnest margin of the three: COGS runs at 66% of sales,
  against 40–52% for the other two lines. Its OPEX base is nearly
  as large as Sportswear's despite generating a quarter of the revenue.
  The loss is a cost-structure problem, not a demand problem.
- **Labor is the single largest cost line company-wide** ($4.49M),
  more than double the next-largest (Payroll, $1.79M), and worth
  isolating from "COGS" as a whole when prioritising cost action.
- **Monthly net profit is seasonal**, ranging from $91.6K (September) to
  $694.5K (January), a 7.6x spread across the year, which the report
  surfaces as a trend rather than a single annual figure.

## Report design

The report follows a standard P&L structure (Revenue → COGS → Gross
Profit → OPEX → EBIT → Interest & Tax → Net Profit), driven by a row
dimension table (`P & L Rows`, see [`P & L Rows.xlsx`](./P%20&%20L%20Rows.xlsx))
rather than one DAX measure per line item. A single switching measure,
keyed on the row's display order (`MAX('P & L Rows'[Order])`), resolves
which underlying calculation to return for whichever P&L line is on
screen; the full measure is in
[`Profit and loss statement measures.txt`](./Profit%20and%20loss%20statement%20measures.txt).

This keeps the report maintainable: adding or re-ordering a P&L line is a
change to the `P & L Rows` table, not a new DAX measure, and the same
pattern extends to a benchmark/variance column (actual vs. a comparison
period, $ and % change) without duplicating the measure set.

## Data

- **Source:** Onyx Data / DataDNA Dataset Challenge, Business Financial
  Dataset, August 2024.
- **Grain:** one row per revenue or expense transaction (580 rows,
  FY2023, monthly).
- **Fields:** year, month, date, business line, amount ($), revenue/expense
  group, expense subgroup, and a revenue-or-expense flag. Full field
  descriptions are in [`Data Dictionary.xlsx`](./Data%20Dictionary.xlsx).
- **P&L row structure:** [`P & L Rows.xlsx`](./P%20&%20L%20Rows.xlsx)
  defines the statement's line items and their display order, consumed by
  the switching measure above.

## Repo contents

| File | Purpose |
|---|---|
| `Business Finance Challenge.pbix` | The Power BI report (data model, measures, visuals) |
| `Data Dictionary.xlsx` | Field-level definitions for the source dataset |
| `P & L Rows.xlsx` | P&L line-item order and labels, used to drive the report |
| `Profit and loss statement measures.txt` | DAX reference for the P&L measures |

## Tech stack

Power BI · DAX · Power Query

## Limitations

- Single fiscal year (2023): no year-over-year comparison is possible
  from this dataset alone.
- Monthly grain only; no daily/weekly view of intra-month variance.
- The model includes "benchmark" and "variance" measures built to
  support an actual-vs-comparison view, but the public dataset doesn't
  ship a second period or budget to compare against, so that comparison
  isn't populated in the published report.

