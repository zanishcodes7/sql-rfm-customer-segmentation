# sql-rfm-customer-segmentation
SQL-based RFM (Recency, Frequency, Monetary) customer segmentation analysis on e-commerce transaction data, using window functions and CASE logic to classify customers into actionable business segments.
# Customer Segmentation using RFM Analysis (SQL)

## Overview
This project performs **RFM (Recency, Frequency, Monetary) analysis** on e-commerce transaction data using pure SQL — no external tools or Python required. RFM is a widely used technique in customer analytics and CRM to identify which customers are most valuable, which are at risk of churning, and which have likely been lost, so a business can target retention efforts effectively.

## Data
The dataset used is a **synthetically generated transactional dataset** (800 customers, ~20,000 order line items across 2023–2024), modeled on the structure of real-world e-commerce data (e.g. the UCI/Kaggle "Online Retail" dataset): invoice number, product, quantity, price, order date, customer ID, and country. The data intentionally includes realistic imperfections — a small percentage of missing customer IDs and negative quantities (representing returns) — to mirror the kind of messy data seen in real transactional systems.

## Approach

**1. Calculate RFM metrics per customer**
- **Recency** — days since the customer's last order (using `DATEDIFF` against a fixed reference date)
- **Frequency** — count of distinct orders per customer (`COUNT(DISTINCT InvoiceNo)`)
- **Monetary** — total amount spent per customer (`SUM(Quantity * UnitPrice)`)

**2. Score each metric (1–5) using window functions**
`NTILE(5)` splits customers into quintiles for each metric. Note that Recency is scored in reverse order (lower days = higher score), since a smaller gap since last purchase indicates a better customer.

**3. Combine into an RFM code**
The three scores are concatenated (e.g. `"543"`) into a single compact code representing each customer's overall standing.

**4. Segment customers using business rules**
A `CASE` statement translates score combinations into actionable labels:
- **Champions** — high Recency, Frequency, and Monetary scores (most valuable, most recent, most frequent buyers)
- **At Risk** — previously high Frequency/Monetary, but low Recency (used to be valuable, haven't purchased recently)
- **New Customers** — high Recency but low Frequency (recently acquired, not yet established)
- **Lost** — low across the board

**5. Rank output by segment priority**
Results are ordered so the most valuable customers (Champions) appear first, down to the least engaged (Lost).

## Results
Out of 800 customers:
| Segment | Customers |
|---|---|
| Champions | 79 |
| At Risk | 28 |
| New Customers | 96 |
| Lost | 352 |

This distribution highlights a large "Lost" segment, suggesting the business would benefit from a re-engagement campaign, alongside targeted retention outreach to the "At Risk" group before they fully churn.

## Skills Demonstrated
- Aggregate functions (`MAX`, `COUNT`, `SUM`)
- Window functions (`NTILE`)
- Subqueries and nested query structuring
- Conditional logic (`CASE WHEN`)
- Custom sort ordering
- Real-world data handling (nulls, returns/negative values)

## Files
- `rfm_analysis.sql` — full script: table creation, data load, and final RFM query

## Author
Zanish Rohail — [github.com/zanishcodes7](https://github.com/zanishcodes7)
