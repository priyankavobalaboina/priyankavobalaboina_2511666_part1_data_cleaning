# priyankavobalaboina_2511666_part1_data_cleaning
# Part 1 — Data Cleaning & Excel Reporting

## What This Project Is About

I got a raw sales dataset (raw_orders.xlsx) from a retail company. It had a lot of problems — wrong formatting, duplicate rows, missing values, messed up dates, and invalid discounts. My job was to clean it up, fix the issues, and create summary reports in Excel.

---

## About the Dataset

- **File:** raw_orders.xlsx
- **Rows:** 932
- **Columns:** 21
- **Period:** 2024–2025
- **Main fields:** order_id, order_date, ship_date, customer_name, segment, region, category, sub_category, quantity, unit_price, discount, sales, cost, profit, order_status, payment_status

---

## Tools I Used

Everything was done in **Microsoft Excel**. Here are the functions and features I used:

**Functions:**
- TRIM() — to remove extra spaces
- PROPER() — to fix capitalization
- SUBSTITUTE() — to remove double spaces
- IF() and nested IF() — for business rules and flags
- COUNTIF() — to find duplicate order_ids
- DATEVALUE() and TEXT() — to fix date formats
- MONTH() and YEAR() — to pull month/year from dates
- ISNUMBER() and ISBLANK() — for validation

**Features:**
- Remove Duplicates
- Find & Replace
- Conditional Formatting
- Format Cells (for dates)
- Sort & Filter (for pivots)

---

## Steps I Followed

1. Used TRIM + PROPER + SUBSTITUTE on all text columns to fix spaces and casing
2. Converted all the different date formats into one standard format (DD/MM/YYYY)
3. Filled 26 blank regions and 22 blank ship_modes with "Unknown"; set 18 missing discounts to 0
4. Removed 20 exact duplicate rows using the Remove Duplicates button
5. Found 12 conflicting duplicates (same order_id, different data) and flagged them with COUNTIF
6. Fixed 8 percentage discounts ("70%" → 0.70) and set 16 negative discounts to 0
7. Added 8 new calculated columns — cleaned_discount, calculated_sales, calculated_profit, profit_margin, shipping_delay_days, order_month, order_year, data_quality_flag
8. Marked each row as Clean, Warning, or Invalid based on the rules

---

## Business Rules I Applied

| Rule | What I Did |
|------|-----------|
| Region blank | Put "Unknown", marked as Warning |
| Ship_mode blank | Put "Unknown", marked as Warning |
| Discount blank | Set to 0 (only if qty and price were fine) |
| Negative discount | Set to 0, marked as Invalid |
| Discount > 50% | Flagged as Invalid |
| Cancelled orders | Didn't include in sales totals |
| Failed payments | Didn't include in sales totals |
| Refunded orders | Showed separately |
| Ship date before order date | Flagged as Invalid |
| Exact duplicates | Deleted (kept first one) |
| Conflicting duplicates | Flagged only, didn't delete |

---

## Data Quality Issues — Quick Summary

| Issue | Count |
|-------|-------|
| Missing regions | 26 |
| Missing ship_modes | 22 |
| Missing discounts | 18 |
| Exact duplicate rows | 20 |
| Conflicting duplicate order_ids | 12 (63 rows total) |
| Negative discounts | 16 |
| Percentage-format discounts | 8 |
| Discounts above 50% | 7 |
| Ship date before order date | 11 |
| Different date formats | 5 types |
| Text formatting issues | ~200 rows |

---

## Pivot Report Findings

### Sales by Region
- West region had the highest sales
- East and North were close behind
- "Unknown" region barely contributed (only 26 records)

### Sales by Category & Sub-Category
- Technology made the most money — especially Phones and Copiers
- Office Supplies had the most variety in sub-categories
- Furniture did well in Bookcases and Chairs

### Orders by Ship Mode
- Standard Class was the most used — about 47% of all orders
- Same Day was the least popular — only around 7%

### Profit Margin by Segment
- Corporate customers had the best margins
- Small Business had the lowest
- Consumer had the most orders overall

### Non-Completed Orders
- West region had the most cancellations and returns
- Cancellation was the #1 reason for non-completion

### Monthly Trend
- Sales kept going up through the year
- December was the highest month (holiday season probably)
- Clear upward trend from Q1 to Q4

---

## Key Insights

1. **West region sells the most** but also has the most cancellations — needs attention
2. **Technology is the money-maker** — Phones and Copiers bring in the most revenue
3. **Standard Class shipping** is what almost half the customers pick
4. **Corporate customers** give better profit margins even though they order less than Consumers
5. **December is the peak month** — probably because of year-end/holiday demand
6. **26% of records had issues** — the company needs to fix their data entry process

---

## Assumptions & Limitations

**Assumptions:**
- Dates like "06/08/2024" — I treated as MM/DD/YYYY (US format)
- For exact duplicates, I kept the first row and removed the rest
- Conflicting duplicates were only flagged, not deleted (someone needs to check manually)
- Max allowed discount is 50% (from the business_rules sheet)
- Only "Completed" orders with "Paid" or "Pending" payment went into pivot summaries

**Limitations:**
- Some dates might be wrong because of the format confusion
- The 12 conflicting duplicates still need manual checking by the team
- High discounts (55%, 65%) — not sure if they were actually approved or not
- Can't get back the missing regions without checking the original system
- Since I did everything manually in Excel, small mistakes are possible

---
