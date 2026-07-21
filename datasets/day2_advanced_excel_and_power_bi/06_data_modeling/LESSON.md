---
title: "Day 2 · Module 6 — Data Modelling & Introduction to DAX"
---

# Day 2 · Module 6 — Data Modelling & Introduction to DAX

**Afternoon · ~90 minutes**
*Topics: relationships, star schemas, hierarchies, calculated columns vs measures, DAX aggregations, time intelligence, KPIs*

**Finished example:** `powerbi_files/day2_advanced_excel_and_power_bi/Data Modeling.pbix`
**Data:** the `dim_*` and `fact_*` CSVs in this folder

> **This is the most important module on Day 2.** Visuals are easy. A good model is what makes them correct.

**📥 Data files:** [data_modeling_data.zip](data_modeling_data.zip) — the 14 dim/fact CSVs

---

## Learn it

## Part A — Data modelling

### Why not just one big table?

You could flatten everything into a single wide table. Many people do, and then wonder why their report is slow, their file is huge, and their totals are wrong.

A **model** splits data into two kinds of table:

| Type | Holds | Answers | Examples here |
|---|---|---|---|
| **Fact** | Events, measurements, numbers | "how much / how many" | `fact_sales`, `fact_orders`, `fact_budget` |
| **Dimension** | Descriptive attributes | "by what" | `dim_product`, `dim_customer`, `dim_date`, `dim_store` |

Facts are long and narrow and grow forever. Dimensions are short and wide and change slowly.

**The test:** if you'd put it after "by" in a sentence — sales *by product*, *by month*, *by region* — it's a dimension. The thing being measured is the fact.

### The star schema

Arrange one fact table in the middle with dimensions around it and you get a shape like a star.

```
        dim_date
            |
dim_customer — fact_sales — dim_product
            |
        dim_store
```

This is the standard, and it is standard for good reasons: it is fast, the filters flow predictably, and it is easy to explain. **Aim for a star.**

The alternative, a **snowflake**, is where a dimension links to another dimension (`dim_store` → `dim_region` in this model). It is not wrong — and sometimes it is the honest structure — but flattening region into the store table would be simpler still.

### Relationships

Model view → drag a field from one table onto the matching field in another.

**Cardinality** — the `1` and `*` on the line:

| Type | Meaning |
|---|---|
| **One-to-many (1:\*)** | One product, many sales rows — **the normal case** |
| One-to-one | Rare; usually the two tables should be one |
| Many-to-many | Powerful and easy to get wrong — avoid until you must |

**Filter direction** — the arrow:

- **Single** (the default and the right choice) — filters flow from the "one" side down to the "many" side. Filter to one product and the sales rows filter with it.
- **Both** — filters flow in both directions. Occasionally necessary, frequently the cause of ambiguous, wrong, slow models. Do not set it casually.

**Active vs inactive** — only one relationship between two tables can be active (solid line). Others are dashed and only work when explicitly invoked in DAX. This matters immediately — see role-playing dimensions below.

### The date table

**Every model needs a dedicated date table.** Not the date column in your fact table — a separate table with one row per day, unbroken, covering your whole range.

Why:

- Time intelligence functions in DAX require one
- It gives you Year, Quarter, Month Name, Day Name as ready-made fields
- It lets you compare *different* facts on a common calendar
- Months with no sales still appear, instead of silently vanishing from a chart

Then **Mark as Date Table** (Table tools → Mark as date table). Power BI needs to be told.

`dim_date` here runs from `2025-01-01` to `2027-12-31` — three unbroken years.

### Role-playing dimensions

Look at `fact_orders`. It has **five** date columns: `order_date`, `ship_date`, `delivery_date`, `invoice_date`, `payment_date`.

You cannot have five active relationships to `dim_date`. So:

- Make **one** active — usually `order_date`
- Create the others as **inactive** (dashed)
- Invoke an inactive one when you need it, with `USERELATIONSHIP()`

That is a role-playing dimension: one date table playing several roles. It comes up constantly in real work — sales by order date is a different question from sales by payment date, and finance usually cares about both.

### Hierarchies

Right-click a field → Create hierarchy, then drag related fields in. `dim_product` supports Category → Subcategory → Product; `dim_date` supports Year → Quarter → Month → Day.

Put a hierarchy on a visual and users can **drill down** through the levels with the arrows in the visual's header.

### Hiding fields

Right-click → **Hide in report view** for anything a report author shouldn't touch: key columns like `customer_id`, and raw numeric columns that should only ever be consumed through a measure.

A model where every field in the list is safe to use is a model people will use correctly.

---

## Part B — Introduction to DAX

### What DAX is

**D**ata **A**nalysis **Ex**pressions — the formula language of Power BI, Power Pivot and Analysis Services.

If you wrote the measures in Module 2, you have already written DAX.

**The fundamental difference from Excel:** Excel formulas refer to **cells**. DAX refers to **columns and tables**. There is no `H2` in DAX; there is `fact_sales[line_total]`, meaning that entire column.

### Calculated columns vs measures — get this right

This is the distinction people most often get wrong, and it has real consequences.

| | **Calculated column** | **Measure** |
|---|---|---|
| Computed | Once, per row, at refresh | On the fly, per visual |
| Stored | In the model — **uses memory** | Not stored |
| Row context | Yes — can see other columns on its row | No — sees whatever the visual is filtering |
| Use for | Something you want to **slice by** | Something you want to **calculate** |
| Created by | Table tools → New column | Table tools → New measure |

**The rule of thumb: if you want to put it on an axis, in a slicer, or in a legend, it's a column. If you want it to be a number in the middle of a visual, it's a measure.**

Default to measures. A model with fifty calculated columns is usually a model that should have had five measures.

```
-- calculated column: a label on each row
Price Band = IF(dim_product[price] >= 500, "Premium", "Standard")

-- measure: a number that responds to context
Total Sales = SUM(fact_sales[line_total])
```

### Filter context — the one concept to actually understand

**A measure is calculated against whatever is filtering it at that moment.**

Write `Total Sales = SUM(fact_sales[line_total])` once. Then:

- On a card with no filters → the grand total
- In a table with Category on rows → each row shows *that category's* total
- With a slicer set to East Africa → only East African sales
- In a matrix by Category and Year → each cell computes its own intersection

You wrote one formula. It produces a different answer in every cell, because each cell has a different **filter context**. Nothing is copied down.

> **If you understood absolute vs relative references on Day 1, you are ready for this.** It's the same underlying question — *what is this calculation actually looking at?* — asked at the level of a whole model instead of a grid of cells.

### Core aggregations

```
Total Sales    = SUM(fact_sales[line_total])
Order Lines    = COUNTROWS(fact_sales)
Distinct Orders= DISTINCTCOUNT(fact_sales[order_id])
Avg Line Value = AVERAGE(fact_sales[line_total])
Total Quantity = SUM(fact_sales[quantity])
```

`COUNTROWS` counts rows in a table; `DISTINCTCOUNT` counts unique values. On this data `COUNTROWS(fact_sales)` is **10** (order *lines*) while `DISTINCTCOUNT(fact_sales[order_id])` is **8** — because order 1001 has two lines. **Choosing the wrong one is a classic way to overstate order volume.**

### `CALCULATE` — the most important function in DAX

`CALCULATE` evaluates an expression with *modified* filters.

```
Online Sales = CALCULATE([Total Sales], fact_sales[order_channel] = "Online")

Sales All Products = CALCULATE([Total Sales], ALL(dim_product))
```

`ALL()` removes a filter — which is how you compute a percentage of total:

```
% of Total = DIVIDE([Total Sales], CALCULATE([Total Sales], ALL(dim_product)))
```

The numerator respects the current row's product; the denominator ignores it. That ratio is the whole trick.

**Always use `DIVIDE(a, b)` rather than `a / b`** — it returns blank instead of an error when the denominator is zero.

### Time intelligence

These need a proper, marked date table.

```
Sales YTD =
    TOTALYTD([Total Sales], dim_date[date])

Sales Last Year =
    CALCULATE([Total Sales], SAMEPERIODLASTYEAR(dim_date[date]))

YoY Growth % =
    DIVIDE([Total Sales] - [Sales Last Year], [Sales Last Year])

Sales Previous Month =
    CALCULATE([Total Sales], PREVIOUSMONTH(dim_date[date]))
```

And with an inactive relationship:

```
Sales by Payment Date =
    CALCULATE(
        [Total Sales],
        USERELATIONSHIP(fact_orders[payment_date], dim_date[date])
    )
```

### KPIs — comparing against budget

`fact_budget` holds targets by month, product and scenario (Plan / Stretch):

```
Total Budget    = SUM(fact_budget[budget_sales])
Plan Budget     = CALCULATE([Total Budget], fact_budget[scenario] = "Plan")
Variance        = [Total Sales] - [Plan Budget]
Achievement %   = DIVIDE([Total Sales], [Plan Budget])

Status = IF([Achievement %] >= 1, "On Target",
          IF([Achievement %] >= 0.9, "Close", "Behind"))
```

> **A grain warning.** `fact_sales` is one row per order *line*. `fact_budget` is one row per *month, product and scenario*. Two facts at different grains cannot relate directly — they connect **through shared dimensions** (`dim_date` and `dim_product`). This is exactly why the star schema is worth the effort: shared dimensions let you compare things that don't otherwise line up.

### Writing DAX that people can read

```
Sales YTD =
VAR CurrentSales = [Total Sales]
VAR LastYear = CALCULATE([Total Sales], SAMEPERIODLASTYEAR(dim_date[date]))
RETURN
    DIVIDE(CurrentSales - LastYear, LastYear)
```

`VAR` names an intermediate result. Use it for anything non-trivial: it is easier to read, easier to debug, and often faster, because each `VAR` is evaluated once.

---

## See it — worked example

Open **`Data Modeling.pbix`** and go to **Model view**.

### The tables

| Table | Rows | Notes |
|---|---|---|
| `fact_sales` | 10 | Order **lines** — order 1001 has two |
| `fact_orders` | 8 | Order headers, with five date columns |
| `fact_budget` | 7 | By month × product × scenario |
| `fact_sales_monthly` | 5 | Pre-aggregated monthly totals |
| `dim_date` | 1,095 | 2025-01-01 → 2027-12-31 |
| `dim_product` | 7 | Aevo / Lumio / Corten Pro |
| `dim_customer` | 5 | |
| `dim_store` | 3 | Links on to `dim_region` — the snowflake |
| `dim_region` | 3 | Southern Africa / East Africa / West Africa |
| `dim_saleseprson` | 6 | |
| `dim_manager` | 2 | |
| `dim_customer_segment` | 7 | **Multiple rows per customer** |
| `dim_customer_loyalty` | 5 | One row per customer |
| `security` | 2 | Row-level security mapping |

**The data is small on purpose.** With ten sales rows you can verify every measure by hand — and you should. Getting a total wrong on ten rows is how you learn to spot it wrong on ten million.

### Five things to look at closely

**1. The star.** `fact_sales` sits in the middle; dimensions radiate outwards. Trace the relationship lines and note the `1` and `*` on each.

**2. Role-playing dates.** `fact_orders` has five date columns. See which relationship to `dim_date` is solid (active) and which are dashed (inactive). Only one can be live at a time.

**3. The snowflake.** `dim_store` → `dim_region`. Ask yourself whether flattening `region_name` into `dim_store` would be simpler. (It usually would.)

**4. A genuine many-to-many.** `dim_customer_segment` has **two rows for customer 1** — Online and VIP. A customer can sit in several segments. This is not a mistake in the data; it is a real modelling situation, and it is why you must not blindly assume every `dim_` table has unique keys. Check `dim_customer_loyalty` by contrast: one row per customer, cleanly one-to-many.

**5. Mixed grain.** `fact_sales` (line level) and `fact_budget` (month × product) relate only through shared dimensions.

### Two things in this data worth criticising

Good practice includes noticing when a model isn't clean:

- **`dim_saleseprson.csv` is misspelled** — it should be `dim_salesperson`. It has been left as-is because the `.pbix` file references it by name and renaming would break the connection. That is exactly the real-world dilemma: a naming mistake becomes expensive once things depend on it. **Name things carefully the first time.**
- **The email domains disagree** — salespeople and managers use `@arka.com`, while `security.csv` uses `@retailco.com`. Since row-level security matches on email, these would **not match**, and RLS would silently show nothing. A great illustration of why you validate keys across tables rather than assuming.

Finding both of those is a legitimate part of the exercise.

---

## Do it — practice

### Exercise 1 — Build the model (~30 min)

1. Load all the `dim_*` and `fact_*` CSVs into a new Power BI file
2. Set data types — dates as dates, IDs consistently typed
3. **Model view:** create relationships from `fact_sales` to `dim_product`, `dim_customer`, `dim_store`, `dim_saleseprson`, and `dim_date` (on `order_date`)
4. Relate `dim_store` → `dim_region`
5. Check every cardinality reads **one-to-many** with **single** filter direction
6. Select `dim_date` → **Mark as date table** on the `date` column
7. Hide every ID column from report view

### Exercise 2 — Role-playing dates (~10 min)

8. Relate `fact_orders[order_date]` to `dim_date[date]` — active
9. Add a second relationship on `payment_date` — it will be created **inactive**
10. Note the solid vs dashed lines

### Exercise 3 — Hierarchies (~5 min)

11. In `dim_product`, build Category → Subcategory → Product
12. Put it on a bar chart and drill down

### Exercise 4 — Write measures (~30 min)

Create these, formatting each sensibly:

```
Total Sales       = SUM(fact_sales[line_total])
Order Lines       = COUNTROWS(fact_sales)
Distinct Orders   = DISTINCTCOUNT(fact_sales[order_id])
Avg Line Value    = DIVIDE([Total Sales], [Order Lines])
Total Budget      = SUM(fact_budget[budget_sales])
Plan Budget       = CALCULATE([Total Budget], fact_budget[scenario] = "Plan")
Variance          = [Total Sales] - [Plan Budget]
Achievement %     = DIVIDE([Total Sales], [Plan Budget])
Sales YTD         = TOTALYTD([Total Sales], dim_date[date])
% of All Products = DIVIDE([Total Sales], CALCULATE([Total Sales], ALL(dim_product)))
Sales by Payment Date =
    CALCULATE([Total Sales], USERELATIONSHIP(fact_orders[payment_date], dim_date[date]))
```

13. **Verify by hand.** With only ten rows you can check every figure. Your measures should give:

    | Measure | Expected |
    |---|---|
    | `Total Sales` | **5,546** |
    | `Order Lines` | **10** |
    | `Distinct Orders` | **8** |
    | `Total Budget` (all scenarios) | **15,083** |
    | `Plan Budget` | **11,087** |

    If `Total Sales` doesn't come to 5,546, something is wrong with a relationship — fix it now, before building anything on top.

14. Compare `Order Lines` (10) with `Distinct Orders` (8) and make sure you can explain the difference.
15. Put `Total Sales` on a table with Category on rows, then add a slicer. Watch one measure produce many different answers — that is filter context.

### Exercise 5 — Investigate (~10 min)

16. Check whether `dim_customer_segment` has unique customer IDs. What does that mean for relating it?
17. Compare the email domains in `dim_saleseprson` and `security.csv`. Would row-level security work? What would you fix?

### If you get stuck

| Problem | Why | Fix |
|---|---|---|
| Relationship won't create | Type mismatch, or neither side unique | Match types; the "one" side needs unique values |
| Totals far too large | Many-to-many, or bidirectional filtering | Check cardinality; set direction to Single |
| Time intelligence returns blank | No marked date table, or gaps in it | Mark as date table; ensure unbroken dates |
| Measure ignores a slicer | No relationship path to that table | Check Model view for a missing link |
| `#ERROR` in a measure | Syntax, or a bad divide | Use `DIVIDE()`; check bracket types |
| Wrong column vs measure | Column can't respond to filters | Recreate it as a measure |
| Blank rows in a visual | Fact keys with no dimension match | Left Anti Join in Power Query to find them |

---

## Check your understanding

1. What distinguishes a fact table from a dimension table? Give the "by" test.
2. Why is a star schema preferred over one big flat table?
3. When should something be a calculated column rather than a measure?
4. Explain filter context in one sentence.
5. Why is `DISTINCTCOUNT(order_id)` different from `COUNTROWS(fact_sales)` here?
6. Why does every model need a separate date table?
7. What is a role-playing dimension, and which function activates an inactive relationship?
8. `fact_sales` and `fact_budget` are at different grains. How can they be compared at all?
9. Why is bidirectional filtering something to avoid by default?
10. Why would the row-level security in this model currently fail?

---

## Where you are now

You have connected data, cleaned it, modelled it, and calculated on it. That is the whole back end of business intelligence, and it is the part most people skip and later regret.

**Now do the [Day 2 homework](../homework/HOMEWORK.md)** — about 45 minutes, building a small model end to end.

**Day 3** is the front end: making it look good, telling a story with it, publishing it, and automating the refresh. The hard thinking is behind you.

---

**Previous:** [Module 5](../05_power_query/LESSON.md) · **Next:** [Day 2 Homework](../homework/HOMEWORK.md) · **Day 2 index:** [README](../README.md)


---

*© 2026 Global Academy. Prepared for the ZIMASCO (Kwekwe) workshop. Facilitated by Tapiwa Zireva. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
