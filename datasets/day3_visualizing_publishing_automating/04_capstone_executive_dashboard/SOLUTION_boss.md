---
title: "Day 3 · Module 4 — Answer Key: Boss Level (23-sheet capstone)"
---

# Answer Key — Boss Level

> **This is one defensible solution, not the only one.** With 23 sheets there are several right answers; what matters is that yours is *deliberate* — you can say why each table is in the model and why each is out. Read this after your own attempt, and compare it against the reference build.

**Data:** [raw_tables.xlsx](raw_tables.xlsx) (23 sheets) · **Reference build:** [Project Clean Model.pbix](../../../powerbi_files/day3_visualizing_publishing_automating/Project%20Clean%20Model.pbix) · **Exercise:** the Boss Level section of [LESSON.md](LESSON.md)

Every figure below is computed from `raw_tables.xlsx`.

---

## Stage 1 — Plan before you click

**The questions the executives actually need answered:**

1. How is revenue tracking against target, and which way is it moving?
2. Where is the money — by region, by subcategory — and where is margin leaking?
3. Are we collecting and delivering on time?

**The fact table:** `order_line_items` (200 rows), at **line grain** — one row per product per order. Revenue and cost both live here.

**The dimensions worth keeping (≈9 tables):**

| Keep | Role |
|---|---|
| `order_line_items` | Fact — line grain |
| `ORDERS_2025` + `ORDERS_2026` → `Orders` | Order header dimension (after append) |
| `products` | Product dimension |
| a built `Date` table | Time |
| `CUST_MASTER` | Customer dimension |
| `Geography` (Address + cities + regions merged) | One clean location dim |
| `subcategories` (split) | Category → Subcategory lookup |
| `sales_targets` | Target measure, related by date only |

**What to leave out and why:** `Sheet1` (duplicate of `shipments`), `order_lines_BACKUP`-style noise, `customer_contacts` (multi-row, not a dimension), `campaign_skus`/`CAMPAIGN_LOG`/`exchange_rates`/`user_details`/`inventory` unless your story needs them, and `security` (that's an RLS input, not a model table). **A good model here is 8–10 tables, not 23.** Deciding what to drop *is* the exercise.

> Optional extras if you have time and a story for them: `shipments` (days-to-deliver), `payments` vs `INVOICES` (days-to-pay), `inventory` (unpivoted, stock cover). None are required for a strong pass.

---

## Stage 2 — Extract & clean

Load only the sheets you're keeping. **Transform Data**, then:

### Append the two order years
`ORDERS_2025` has **15** columns; `ORDERS_2026` has **13** (2025 carries `LegacyRef` and `SourceFile`). Remove those two from 2025, then **Append Queries as New** → `Orders`. ✅ **80 order rows**, matching columns.

### Kill the duplicate sheet
`Sheet1` is a byte-for-byte copy of `shipments` (both 85 rows). Don't load it. If you already did, remove the query — loading both would double every shipment metric.

### Split the jammed category column
`subcategories` holds `CategorySubcategory` as `electronics|phones`. Select it → **Split Column → By Delimiter → Custom `|`** → two columns `Category`, `Subcategory`. This becomes your category lookup for the hierarchy.

⚠️ **Case mismatch — an easy trap.** The split gives lowercase subcategories (`phones`), but `products[SubcategoryName]` is title-case (`Phones`). Left as-is they **won't join** — the relationship would produce all blanks. On the `Subcategory` column: **Transform → Format → Capitalize Each Word** (do the same to `Category` if you'll show it). All 18 subcategories then match `products` exactly.

### Clean products
Remove `hash_key`, `source_id`, and the long `ProductDescription`. Filter out the poison rows: `Brand = TEST` (the `DO NOT USE` row) and the two rows whose `SubcategoryName` is `SAP-099001`/`SAP-099002` (duplicate product names that would break the relationship). ✅ **products 63 → 60 rows**, 60 distinct `ProductName`.

### Clean customers & the rest
`CUST_MASTER`: remove `hash_key`, `source_id`. **Trim and clean** every text key — orders join on `CustomerName`, not an ID, so leading spaces and case differences silently break joins. `Transform → Format → Trim`, then `Format → lowercase`→compare, or standardise casing.

### Build the geography dimension
Merge the three geography sheets into one, rather than snowflaking:
- `Address` (AddressID, Street, CityName) **Merge** with `cities` (CityName, RegionName) on `CityName` → expand `RegionName`.
- `regions` is just the five region names — a validation list; you don't need it as a table once `RegionName` is on `Geography`.
- Result: one `Geography` table keyed by `AddressID`, carrying City and Region. Relate it to `CUST_MASTER[AddressID]`.

### Filter the contacts (if you use them at all)
`customer_contacts` has up to 4 rows per customer. If you want a primary contact, **filter `IsPrimary = True`** so it's one row per customer — otherwise leave it out. It is *not* a dimension as it stands.

### Unpivot inventory (only if using it)
`inventory` is wide — one column per month (`2025-01` …). Select `ProductName` → **Unpivot Other Columns** → rename to `Month`, `Units`. Optional for the core story.

### Find the orphans before trusting a total
**Merge Queries** → `order_line_items` ⟕ `products` on `ProductName` → **Left Anti** join. ✅ **3 orphan rows** — `Retired Cable Z1`, `Legacy Widget X9`, `Old Model A0` — carrying **$687.50**. These are discontinued products; flag them, don't invent catalogue rows. **Close & Apply.**

---

## Stage 3 — Model

**The star.** `order_line_items` in the centre; every relationship **one-to-many, single direction**, arrow from the "one" side to the fact.

| From (one) | To (many) | On |
|---|---|---|
| `products[ProductName]` | `order_line_items[ProductName]` | product |
| `Orders[OrderID]` | `order_line_items[OrderID]` | order |
| `CUST_MASTER[CustomerName]` | `Orders[CustomerName]` | customer |
| `Geography[AddressID]` | `CUST_MASTER[AddressID]` | location |
| `subcategories[Subcategory]` | `products[SubcategoryName]` | category lookup |
| `Date[Date]` | `Orders[OrderDate]` | date |
| `Date[Date]` | `sales_targets[TargetDate]` | month (grain gap — see below) |

Build the `Date` table with `CALENDAR` + `ADDCOLUMNS` and **Mark as Date Table**. `sales_targets[Period]` is text (`2025-02`) — add `TargetDate = Date.FromText([Period] & "-01")` and relate by date, so the monthly target and the line-level fact never share a grain.

**Hierarchy.** In `products`, build **Category → Subcategory → Product** (Category and Subcategory come from the split `subcategories` lookup) so the dashboard drills cleanly.

**Tidy.** Hide all key columns and raw numeric columns you won't display. In Model view, confirm no ambiguous paths and no unintended bidirectional filters.

> Two facts (`order_line_items` and, say, `shipments` or `INVOICES`) must **never** be related directly — join each to the shared dimensions (`Orders`, `Date`) and let the model fan out. Relating facts to facts is the classic wrong-totals trap.

---

## Stage 4 — Measures

```
Total Revenue   = SUM(order_line_items[LineTotal])
Total Cost      = SUMX(order_line_items, order_line_items[Quantity] * order_line_items[UnitCost])
Gross Profit    = [Total Revenue] - [Total Cost]
Gross Margin %  = DIVIDE([Gross Profit], [Total Revenue])
Order Count     = DISTINCTCOUNT(order_line_items[OrderID])
Avg Order Value = DIVIDE([Total Revenue], [Order Count])
Revenue Target  = SUM(sales_targets[TargetRevenue])
Variance        = [Total Revenue] - [Revenue Target]
Achievement %   = DIVIDE([Total Revenue], [Revenue Target])
Revenue YTD     = TOTALYTD([Total Revenue], 'Date'[Date])
Revenue LY      = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR('Date'[Date]))
YoY Growth %    = DIVIDE([Total Revenue] - [Revenue LY], [Revenue LY])
```

**`SUMX` for cost, always** — quantity × unit cost is a per-row product. `SUM(Quantity) * SUM(UnitCost)` is nonsense and takes margin down with it.

### Verify — the whole model

| Measure | Value |
|---|---|
| Total Revenue | **$526,643.91** |
| Total Cost | **$332,204.07** |
| Gross Profit | **$194,439.84** |
| Gross Margin % | **36.92%** |
| Order Count | **80** |
| Avg Order Value | **$6,583.05** |

### Verify — over time and target

| | 2025 | 2026 |
|---|---|---|
| Revenue | $266,271.26 | $260,372.65 |
| Revenue Target | $280,000 | $272,000 |
| Achievement % | 95.1% | 95.7% |

**YoY revenue ≈ −2.2%.** Both years land just shy of target — steady, not dramatic. The interest is in *where* and *what margin*.

---

## Stage 5 — The dashboard (three acts)

Same discipline as the Dash — one page, one message.

**Act 1 — KPI cards with comparison:** Revenue vs target ($526.6k, 95% of a combined $552k), Gross Margin % (36.9%), YoY Growth (−2.2%).

**Act 2 — Trend:** `[Total Revenue]` and `[Revenue Target]` over `Date` — the story is *flat against a rising target*, not collapse.

**Act 3 — Cause + recommendation:**

- **Revenue by region**, sorted: **Europe $167,364 · Middle East $117,005 · Asia Pacific $103,847 · North America $97,456 · Latin America $40,972.** Europe leads; Latin America is a quarter of Europe.
- **Margin by subcategory**, sorted ascending — the leak is at the bottom: **Fragrance 32.4% · Safety 33.2% · Fitness 33.7%** are the thinnest, well below the 36.9% blend.
- A written recommendation box (What / Why / So what / Now what) naming one action — e.g. review pricing on the low-margin subcategories, or investigate why Latin America lags.

**Title states the finding**, two slicers (Year, Region), alt text on every visual, detail on a drill-through page.

---

## Stage 6 — Publish & secure

1. **Publish** to a proper workspace (not *My Workspace*).
2. **Scheduled refresh** on; **failure notifications** on.
3. **Row-level security** from the `security` sheet (UserEmail → Region): create a role that filters `Geography[RegionName]` (or the region field) to `= USERPRINCIPALNAME()`'s mapped region, and **test with View as role**. Verify a North America user sees only North America.
4. Note which sources would need an **on-premises gateway** (anything not cloud-hosted — a local SQL Server or a file share) and why.

---

## Stage 7 — Present (two minutes)

| Time | Say |
|---|---|
| 0:00–0:20 | "Revenue is $527k — 95% of target, and down about 2% on last year." |
| 0:20–0:60 | "It's flat against a target that went up. Europe carries the business; Latin America is a quarter of it." |
| 0:60–1:30 | *One click:* margin by subcategory — "Fragrance, Safety and Fitness are our thinnest lines at ~33%." |
| 1:30–1:50 | "I'd review pricing on those three and dig into why Latin America trails." |
| 1:50–2:00 | "Caveat: three discontinued products carry $688 of revenue we can't attribute — a data-quality flag, not a big number." |

Don't narrate your merges. The page is the proof.

---

## Your model will differ — and that's fine

There are several defensible shapes here (whether to bring in shipments, payments, campaigns; how far to take the geography dim). Compare yours against **[Project Clean Model.pbix](../../../powerbi_files/day3_visualizing_publishing_automating/Project%20Clean%20Model.pbix)** — the point isn't to match it, it's to be able to defend every table you kept and every one you dropped.

---

**Exercise:** [LESSON.md](LESSON.md) · **Dashboard Dash answer key:** [SOLUTION_dash.md](SOLUTION_dash.md) · **Day 3 index:** [README](../README.md) · **Course home:** [README](../../../README.md)


---

*© 2026 Global Academy. Prepared for the ZIMASCO (Kwekwe) workshop. Facilitated by Tapiwa Zireva. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
