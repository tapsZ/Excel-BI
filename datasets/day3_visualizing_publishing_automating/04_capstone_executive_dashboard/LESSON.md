---
title: "Day 3 · Module 4 — Capstone: Build and Present an Executive Dashboard"
---

# Day 3 · Module 4 — Capstone: Build and Present an Executive Dashboard

**Afternoon · ~2 hours build + 5 minutes presenting**

**Data:** `raw_tables.xlsx` (this folder) — 23 sheets of genuinely messy source data
**Reference build:** `powerbi_files/day3_visualizing_publishing_automating/Project Clean Model.pbix`

**📥 Data file:** [raw_tables.xlsx](raw_tables.xlsx) — download this before you start the capstone

---

## The brief

You are the analyst for a B2B distributor. The executive team meets on Monday. They have asked for **one dashboard** that tells them how the business is performing and what needs attention.

Nobody has cleaned the data. You have `raw_tables.xlsx` — 23 sheets, exported from several systems over several years, with all the problems that implies.

**Deliverable:** a one-page executive dashboard, plus a two-minute verbal walkthrough ending in a recommendation.

This uses everything from all three days.

---

## What's in the workbook

Open `raw_tables.xlsx` and look before you build. Roughly, the sheets fall into groups:

**Transactions (facts)**

| Sheet | Rows | Notes |
|---|---|---|
| `ORDERS_2025` | 40 | Order headers |
| `ORDERS_2026` | 40 | Same idea, **column layout differs slightly** |
| `order_line_items` | 200 | Line detail — quantity, unit price, unit cost, discount |
| `INVOICES` / `invoice_lines` | 68 / 164 | Billing |
| `payments` | 65 | Cash received |
| `shipments` | 85 | Despatch and delivery |
| `Sheet1` | 85 | **Look closely at this one** |

**Reference (dimensions)**

| Sheet | Rows | Notes |
|---|---|---|
| `CUST_MASTER` | 61 | Customers, with `hash_key` / `source_id` clutter |
| `products` | 63 | Catalogue |
| `Address` / `cities` / `regions` | 70 / 20 / 5 | Geography, split across three sheets |
| `subcategories` | 18 | **Category and subcategory are jammed into one column** |
| `customer_contacts` | 153 | **Several contacts per customer** |
| `user_details` | 60 | Credit limits |

**Targets, campaigns, other**

| Sheet | Rows | Notes |
|---|---|---|
| `sales_targets` | 20 | Revenue targets by period — your KPI comparison |
| `CAMPAIGN_LOG` | 140 | Marketing spend and clicks |
| `campaign_skus` | 6 | **A list of SKUs crammed into one cell per row** |
| `inventory` | 24 | **Wide format — a column per month** |
| `exchange_rates` | 12 | Currency conversion |
| `security` | 5 | Row-level security mapping |

### The traps, stated plainly

You'll find these. They're deliberate, and each maps to something you learned:

1. **`Sheet1` is a byte-for-byte duplicate of `shipments`** — same 85 rows, same columns. A leftover staging copy. Load one, and if you load both you'll double every shipment metric. *(Day 2 M5)*
2. **`ORDERS_2025` has 15 columns; `ORDERS_2026` has 13.** 2025 carries `LegacyRef` and `SourceFile`; 2026 has neither. Reconcile the columns before appending, or Power Query will fill the gaps with nulls. *(Day 2 M5)*
3. **`subcategories` packs both levels into one column**, pipe-delimited: `electronics|phones`. Split by `|`. *(Day 2 M5)*
4. **`inventory` is wide** — one column per month (`2025-01`, `2025-02`, …). Unpivot it. *(Day 2 M5)*
5. **`campaign_skus` holds a comma-separated SKU list in one cell** (`HOM-044, BEA-012, SPO-011, …`). Split to rows, then trim, if you use it. *(Day 2 M5)*
6. **`customer_contacts` has up to 4 rows per customer** across 60 customers. It is *not* a clean dimension — filter to `IsPrimary`, or leave it out. *(Day 2 M6)*
7. **Geography is spread over three sheets** — Address → cities → regions. Merge them into one clean dimension rather than snowflaking three levels. *(Day 2 M6)*
8. **`hash_key` and `source_id` columns are system noise.** Remove them. *(Day 2 M5)*
9. **Several sheets join on `CustomerName`, not an ID.** Text keys are fragile — trim and standardise case, and check for near-duplicates. *(Day 2 M5)*
10. **`order_line_items` is line-level; `sales_targets` is by period.** Different grains — relate through shared dimensions only. *(Day 2 M6)*

> **Do not try to load all 23 sheets.** A good model here uses maybe eight to ten tables. Deciding what to leave out is part of the exercise — and is the difference between a model and a mess.

---

## Suggested approach

### Stage 1 — Plan before you click (15 min)

Genuinely do this on paper first.

1. What are the two or three questions the executives actually need answered?
2. Which sheets do you need? Which are noise?
3. What is your **fact** table, and at what **grain**?
4. Which **dimensions** surround it?
5. Sketch the star.

Skipping this stage is the most reliable way to spend two hours and produce something confusing.

### Stage 2 — Extract and clean (40 min)

Load `raw_tables.xlsx`, selecting only the sheets you need.

- Set data types **first**
- Remove `hash_key`, `source_id` and other system columns
- Trim and standardise every text key
- Append the two order years
- Split `subcategories`; unpivot `inventory` if you're using it
- Merge Address → cities → regions into one geography dimension
- Build the date dimension
- **Left Anti Join** each fact against each dimension to find orphan keys — do this *before* you trust any total

### Stage 3 — Model (25 min)

- Create relationships; every one **one-to-many, single direction**
- Mark the date table
- Hide keys and raw numeric columns
- Build a Category → Subcategory → Product hierarchy
- Verify: no ambiguous paths, no unexpected many-to-many

### Stage 4 — Measures (25 min)

At minimum:

```
Total Revenue     = SUM(order_line_items[LineTotal])
Total Cost        = SUMX(order_line_items, order_line_items[Quantity] * order_line_items[UnitCost])
Gross Profit      = [Total Revenue] - [Total Cost]
Gross Margin %    = DIVIDE([Gross Profit], [Total Revenue])
Order Count       = DISTINCTCOUNT(order_line_items[OrderID])
Avg Order Value   = DIVIDE([Total Revenue], [Order Count])
Revenue Target    = SUM(sales_targets[TargetRevenue])
Variance          = [Total Revenue] - [Revenue Target]
Achievement %     = DIVIDE([Total Revenue], [Revenue Target])
Revenue YTD       = TOTALYTD([Total Revenue], 'Date'[Date])
Revenue LY        = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR('Date'[Date]))
YoY Growth %      = DIVIDE([Total Revenue] - [Revenue LY], [Revenue LY])
```

Consider also: days-to-deliver from `shipments`, days-to-pay from `payments` vs `INVOICES`, and campaign ROI from `CAMPAIGN_LOG`.

**`SUMX` is new here.** It iterates row by row, multiplying quantity by cost *per row* before summing. `SUM(Quantity) * SUM(UnitCost)` would be badly wrong — a good illustration of why row-level maths needs an iterator.

**Sanity-check every measure by hand** against a filtered subset before you build visuals on it.

### Stage 5 — Build the dashboard (30 min)

Apply Modules 1 and 2:

- KPI cards **with comparisons** — revenue vs target, margin, YoY
- One sorted bar chart, one colour, leader highlighted
- One trend line over the date dimension
- One matrix with conditional formatting
- Two slicers, restrained
- Detail on a **drill-through** page, not the front
- Every title a **finding**
- A text box with What / Why / So what / Now what

### Stage 6 — Publish (10 min, if you have an account)

Publish to a workspace, configure refresh, add RLS from the `security` sheet, and check it as a Viewer.

### Stage 7 — Present (5 min)

Two minutes, out loud, once through beforehand.

---

## The reference build

`Project Clean Model.pbix` is one worked solution. **Try Stages 1–3 yourself before opening it** — comparing your model against a reference teaches far more than copying one.

Your model will differ, and that's fine. There are several defensible answers here. What matters is that yours is *deliberate*: you can say why each table is in it and why each is out.

---

## Marking guide

Assess your own work honestly against this.

| Area | What good looks like | Weight |
|---|---|---|
| **Data cleaning** | Duplicates gone, types correct, keys trimmed, system columns removed, orphans checked | 20% |
| **Model** | Clean star; correct cardinality and direction; marked date table; keys hidden; no ambiguity | 25% |
| **Measures** | Correct and verified; sensible use of `CALCULATE`, `DIVIDE`, `SUMX`, time intelligence | 20% |
| **Visual design** | Right charts, sorted, restrained colour, aligned, accessible | 15% |
| **Storytelling** | Finding-style titles, comparisons on every KPI, written recommendation, one clear message | 15% |
| **Publishing** | Workspace, refresh, RLS, appropriate sharing | 5% |

**The heaviest weights are on the model and the data.** That is deliberate, and it reflects reality: a beautiful dashboard on a broken model is worse than no dashboard, because people believe it.

---

## Common capstone failures

| Failure | Consequence |
|---|---|
| Loading all 23 sheets | Ambiguous relationships; nothing totals correctly |
| Skipping the planning stage | Two hours of rework |
| Not checking for orphan keys | Silently missing revenue |
| `SUM(Qty) * SUM(Cost)` instead of `SUMX` | Cost, and therefore margin, badly wrong |
| Relating two facts directly | Wrong totals; relate through dimensions |
| Building visuals before verifying measures | Confident, wrong dashboard |
| Fifteen visuals on one page | No message |
| Titles as labels | Reader does the analysis, and mostly doesn't |
| No recommendation | A report, not a decision |

---

## Presenting: the two-minute structure

| Time | Say |
|---|---|
| 0:00–0:20 | The headline. "Revenue is X, which is Y% against target." |
| 0:20–0:60 | The conflict. What changed, where, and by how much. |
| 0:60–1:30 | The cause. One drill-down or one slicer click — **one**. |
| 1:30–1:50 | The recommendation. What should happen, by when. |
| 1:50–2:00 | The caveat. One honest limitation of the data. |

Do not narrate your Power Query steps. Nobody in that room cares how many merges it took, and the finished thing shows the work.

---

## Check your understanding

1. Why load eight tables instead of all 23?
2. `Sheet1` and `shipments` are identical. What would loading both do?
3. Why is `customer_contacts` unsuitable as a dimension as it stands?
4. Explain why `SUMX` is needed for Total Cost.
5. `order_line_items` and `sales_targets` are at different grains. How do you compare them?
6. What's the fastest way to find fact rows with no matching dimension?
7. Why does the marking guide weight the model above visual design?

---

## You've finished the course

Three days ago the starting point may have been "what is a cell". You have now taken 23 sheets of unstructured export data and turned it into a governed, refreshable, published model with an executive dashboard on top — and a recommendation attached.

The tools will keep changing. The three things that won't:

1. **Clean data first.** Everything downstream inherits its problems.
2. **Model it properly.** A star schema of facts and dimensions, every time.
3. **Say what it means.** A number without a comparison and a recommendation changes nothing.

**Final homework:** [Extend and publish](../homework/HOMEWORK.md).

---

**Previous:** [Module 3](../03_publishing_and_sharing/LESSON.md) · **Day 3 index:** [README](../README.md) · **Course home:** [README](../../../README.md)


---

*© 2026 Global Academy. Prepared for the ZIMASCO (Kwekwe) workshop. Facilitated by Tapiwa Zireva. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
