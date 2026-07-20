# Day 2 Homework — Clean, Model, Measure

**Time: about 45 minutes · Do this before Day 3**

---

## The brief

NovaTech Retail has opened four stores across southern and eastern Africa. Two quarterly sales exports have landed on your desk — both **messy**, in the way real exports always are.

Build a small but correct Power BI model from them, and produce four measures the regional manager can trust.

This uses everything from Day 2: Power Query cleaning, append, merge, relationships and DAX.

### Your files

| File | Rows | What it is |
|---|---|---|
| `sales_q1.csv` | 51 | Q1 2026 sales — **messy** |
| `sales_q2.csv` | 51 | Q2 2026 sales — **messy** |
| `stores.csv` | 4 | Store lookup — clean |
| `products.csv` | 6 | Product lookup — clean |

---

## Part 1 — Clean and combine *(Module 5)*

Load `sales_q1.csv` and `sales_q2.csv` with **Transform Data**, not Load.

Turn on **Column quality** and **Column distribution** (View tab) before you start — you'll want to see the problems shrink.

1. **Append** the two quarters into one query
2. Remove the **blank rows**
3. Remove the **duplicate rows** (each file has several exact repeats)
4. **Trim** and **uppercase** the `SKU` column — some values arrived as `  nt-005  `
5. Fix the `SaleDate` column — it contains **two different date formats**
6. Fix the `Sales` column — some values arrived as text like `$ 1,043.96`
7. Decide what to do about the **missing quantities**, and be able to justify your choice
8. Remove the `LogRef` column — nobody needs it

### Checkpoint — your cleaned query must give exactly this

| Check | Expected |
|---|---|
| Rows after removing blanks and duplicates | **90** |
| Distinct SKUs (after trimming and uppercasing) | **6** |
| Rows with a missing Quantity | **8** |
| **Total Sales** | **154,387.48** |

**If your total isn't 154,387.48, stop and fix it before going further.** The most likely causes: duplicates not removed, or the text-formatted `$` values silently dropped to null when you set the type.

## Part 2 — Merge in the lookups *(Module 5)*

9. Load `stores.csv` and `products.csv` as **Connection Only**
10. **Merge** your sales query to `products.csv` on `SKU` (Left Outer), expanding `Product` and `Category`
11. **Merge** to `stores.csv` on `StoreID`, expanding `StoreName`, `City` and `Country`
12. Run a **Left Anti Join** as a check — it should return **zero rows**. If it doesn't, your SKU trimming missed something.

## Part 3 — Model it *(Module 6)*

13. Load `stores` and `products` as proper tables too (you need them as dimensions)
14. Build a **date table** covering 2026, and **Mark as date table**
15. Create relationships: sales → products (`SKU`), sales → stores (`StoreID`), sales → date (`SaleDate`)
16. Confirm every relationship is **one-to-many** with **single** filter direction
17. Hide the ID/key columns from report view

## Part 4 — Write measures *(Module 6)*

18. Write these four:

```
Total Sales      = SUM(Sales[Sales])
Total Quantity   = SUM(Sales[Quantity])
Distinct Sales   = DISTINCTCOUNT(Sales[SaleID])
Avg Sale Value   = DIVIDE([Total Sales], [Distinct Sales])
```

19. Add one **time-intelligence** measure:

```
Sales YTD = TOTALYTD([Total Sales], 'Date'[Date])
```

20. Build a quick visual check: `Category` on rows with `Total Sales`. You should see:

| Category | Total Sales |
|---|---|
| Laptops | 73,217.39 |
| Phones | 62,208.07 |
| Tablets | 16,643.97 |
| Accessories | 2,318.05 |
| **Total** | **154,387.48** |

---

## Stretch goals *(optional)*

- Add a `Country` slicer and confirm the visuals cross-filter
- Add a measure for each store's share of total: `DIVIDE([Total Sales], CALCULATE([Total Sales], ALL(Stores)))`
- Build a Category → Product hierarchy and drill into it
- Add a line chart of `Total Sales` by month and note the Q1/Q2 shape

---

## How to check your work

There is no `.pbix` answer key for this one — **the checkpoint numbers above are the answer key**, and they are more useful. If your figures match, your cleaning, your merges and your model are all correct. If they don't, the number that's wrong tells you which step to revisit:

| Symptom | Look at |
|---|---|
| Row count above 90 | Duplicates or blank rows not removed |
| Row count below 90 | You removed too much — check the missing-quantity rows are still there |
| Total Sales too low | The `$ 1,043.96` text values became null when you set the type |
| More than 6 distinct SKUs | Trim/uppercase didn't catch the `  nt-005  ` variants |
| Anti join returns rows | Same cause — unmatched SKUs |
| Category totals wrong but grand total right | The product merge matched the wrong rows |
| Time intelligence blank | Date table missing, not marked, or doesn't cover 2026 |

### On the missing quantities

There are 8 rows with no quantity but a valid sales amount. There isn't one correct answer, and that's deliberate — you should be able to say what you did and why:

- **Leave them null** — honest; `Total Quantity` will simply exclude them
- **Replace with 0** — misleading, since a sale clearly happened
- **Remove the rows** — loses $ from your revenue total, which is worse
- **Derive from `Sales / ListPrice`** — reasonable, but the prices vary by a discount factor, so it's an estimate

Whatever you choose, it should be a deliberate step in Applied Steps, not an accident. That's the real lesson.

---

## What you should be able to say afterwards

You took two dirty files, made them trustworthy, joined them to reference data, modelled them properly, and calculated on them — with every step recorded and repeatable.

That is the job. Day 3 makes it look good and gets it in front of people.

---

**Back to:** [Day 2 index](../README.md) · **Last module:** [Module 6](../06_data_modeling/LESSON.md) · **Next:** [Day 3](../../day3_visualizing_publishing_automating/README.md)
