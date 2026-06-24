
# Cleaning Log

**Dataset:** raw_orders.xlsx  
**Records:** 932 rows, 21 columns  
**Date:** June 2025

---

## Issues I Found

### Text Problems
- Many columns had extra spaces before/after text (e.g., "  Small Business ", "Corporate ")
- Some entries had double spaces in between words like "Small  Business" or "Standard  Class"
- Case was all over the place — some were uppercase ("OFFICE SUPPLIES"), some lowercase ("consumer"), some mixed

### Date Problems
- Dates were in 5 different formats — "21 Jul 2024", "08/31/2024", "28-11-2024", "2024-05-24", etc.
- 11 records had ship date before order date (doesn't make sense)
- 13 records had blank dates

### Missing Data
- 26 rows had no region
- 22 rows had no ship_mode
- 18 rows had no discount value

### Duplicates
- Found 20 rows that were exact copies (same data in every column)
- 31 order_ids appeared more than once but with different info (total 63 rows affected)

### Discount Problems
- 16 rows had negative discounts (like -0.19, -0.23) — probably typos
- 8 rows had discount written as text with % sign ("70%", "85%")
- 7 rows had discounts above 50% (0.55, 0.65)

### Order Status Mix
- Cancelled and failed payment orders were mixed in with completed ones
- Refunded orders were also present

---

## What I Did to Clean It

| Step | What I Did | How (Excel) | How Many |
|------|-----------|-------------|----------|
| 1 | Removed extra spaces at start/end | TRIM() | ~200 rows |
| 2 | Fixed double spaces inside text | SUBSTITUTE(cell,"  "," ") | ~30 rows |
| 3 | Made everything Title Case | PROPER() | ~150 rows |
| 4 | Made all dates same format (DD/MM/YYYY) | Format Cells > Date | All 932 |
| 5 | Put "Unknown" where region was blank | Find & Replace | 26 rows |
| 6 | Put "Unknown" where ship_mode was blank | Find & Replace | 22 rows |
| 7 | Changed "70%" to 0.70 (text to number) | SUBSTITUTE to remove %, then /100 | 8 rows |
| 8 | Put 0 for missing discount (if other fields ok) | IF(ISBLANK()) | 18 rows |
| 9 | Changed negative discounts to 0 | IF(cell<0, 0, cell) | 16 rows |
| 10 | Deleted exact duplicate rows | Remove Duplicates button | 20 removed |
| 11 | Flagged conflicting duplicates | COUNTIF on order_id | 12 flagged |
| 12 | Added calculated columns | Formulas | 912 rows |

---

## Business Rules I Followed

| Situation | What I Did | Why |
|-----------|-----------|-----|
| Region blank | Wrote "Unknown" + flagged | Can't delete the row, still need it for analysis |
| Ship_mode blank | Wrote "Unknown" + flagged | Same reason — keep the record |
| Discount blank | Set to 0 (only if qty and price were fine) | Assumed no discount was given |
| Negative discount | Set to 0 and marked Invalid | Clearly a data entry mistake |
| Discount > 50% | Kept the value but flagged it | Company max is 50%, so these need checking |
| Cancelled orders | Left out of sales totals | They didn't generate revenue |
| Failed payments | Left out of sales totals | Money was never received |
| Refunded orders | Showed separately in pivot | Need to track but not count as active sales |
| Ship date before order date | Flagged as Invalid | Impossible — must be wrong entry |

---

## Assumptions I Made

1. For dates like "06/08/2024" — I assumed it's MM/DD/YYYY (June 8th, not August 6th) because most entries followed US format
2. For exact duplicates — kept the first one, deleted the rest
3. For conflicting duplicates (same order_id, different data) — I did NOT delete them, just flagged for review
4. 50% is the max discount allowed (got this from the business_rules sheet)
5. Missing discount = 0 only when quantity, unit_price, and sales all had valid numbers
6. Used Title Case for all text (first letter capital)
7. For pivot tables — only used orders that were "Completed" with payment "Paid" or "Pending"

---

## Records Removed

| Reason | Count |
|--------|-------|
| Exact duplicates | 20 |
| **Total removed** | **20** |

I didn't delete anything else. Even invalid or suspicious records were kept in the file — just flagged.

---

## Records Flagged

| Flag | Count | What it means |
|------|-------|--------------|
| Clean | 638 | No problems found |
| Warning | 238 | Small issues — missing region, cancelled order, etc. |
| Invalid | 36 | Serious issues — negative discount, impossible dates |
| **Total** | **912** | |

---

## Limitations

1. Some dates are ambiguous — "06/08/2024" could be June 8 or Aug 6. I went with MM/DD/YYYY but might be wrong for some
2. The 12 conflicting duplicates need someone from the business team to check manually
3. Discounts of 55% and 65% — I flagged them but I'm not sure if they were actually approved by a manager
4. Can't recover the missing regions — would need access to the original system
5. Some original sales numbers don't match my formula (qty × price × (1-discount)) — I used my calculated version
6. Did everything manually in Excel so there's a small chance of human error

---

## Final Numbers

- Started with: 932 records
- After removing duplicates: 912 records
- Clean: 638 (69.9%)
- Warning: 238 (26.1%)
- Invalid: 36 (3.9%)
- New columns added: 8
- Final total columns: 29
