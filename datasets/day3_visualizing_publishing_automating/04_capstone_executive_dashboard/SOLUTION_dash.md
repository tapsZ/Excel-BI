---
title: "Day 3 · Module 4 — Answer Key: The 45-Minute Dashboard Dash"
---

# Answer Key — The 45-Minute Dashboard Dash

> **Open this only after you've run the Dash and given your pitch.** A worked solution read too early is just someone else's answer; worked *after* your own attempt, it's the most useful half-hour of the course. Your model will differ in small ways and that's fine — what matters is that every number below reconciles with yours.

**Data:** [novatech_capstone.xlsx](novatech_capstone.xlsx) · **Exercise:** [LESSON.md](LESSON.md)

This is one full, defensible solution — every click, every formula, every figure. Where two routes work, the simpler one is shown.

---

## Round 0 — the traps, and why each one bites

You could see all seven from the Navigator preview without loading a thing.

| # | Trap | Why it hurts if you miss it |
|---|---|---|
| 1 | `order_lines_BACKUP` is a byte-for-byte copy of `order_lines` (both 578 rows) | Load both and every revenue, cost and margin figure **doubles**. Silent, and it looks plausible. |
| 2 | `ORDERS_2025` has 15 columns, `ORDERS_2026` has 13 (`LegacyRef`, `SourceFile` are 2025-only) | Append without reconciling and Power Query pads 2026 with **null** columns you then have to explain. |
| 3 | `PRODUCTS` carries `hash_key` and `source_id` | System noise. Harmless to totals, but clutters the model and confuses the relationship pane. |
| 4 | `PRODUCTS` has junk rows — `DO NOT USE` (Brand `TEST`) plus two rows whose `Category` is `SAP-099001`/`SAP-099002` | The two `SAP-…` rows **repeat existing product names**, so `ProductName` is no longer unique — and your one-to-many relationship silently becomes many-to-many. |
| 5 | `OrderDate` is a date **and time** (`2026-10-18 11:33:50`) | Left as datetime it fragments your date axis and won't join cleanly to a date table. |
| 6 | `staff_contacts` has four rows per branch | Not a dimension — a fan-out that would multiply your branch rows. Leave it out. |
| 7 | `monthly_targets` is monthly; `order_lines` is per line | Different grains. Relate them directly and you get wrong totals; relate through the date table instead. |

**Six sheets belong in the model:** `ORDERS_2025`, `ORDERS_2026`, `order_lines`, `PRODUCTS`, `branches`, `monthly_targets`. The other three (`_INSTRUCTIONS`, `order_lines_BACKUP`, `staff_contacts`) do not.

---

## Round 1 — Clean

### Load

**Home → Get Data → Excel workbook →** `novatech_capstone.xlsx` **→** in the Navigator tick the six sheets above **→ Transform Data** (not Load — you clean first).

### 1 · Reconcile the columns, then append

In `ORDERS_2025`: select `LegacyRef` and `SourceFile` → right-click → **Remove Columns**. Both order queries now have the same 13 columns.

**Home → Append Queries → Append Queries as New →** `ORDERS_2025` + `ORDERS_2026` → name the result **`Orders`**. Right-click the two original order queries → **uncheck Enable load** (keep them as staging, don't load them twice).

✅ `Orders` = **270 rows × 13 columns**. If you see 15 columns, you appended before removing — the extra two are null for every 2026 row.

### 2 · Fix the date

`Orders` → click the `OrderDate` column type icon → **Date** (not Date/Time). The time component is noise here.

### 3 · Clean PRODUCTS

- Select `hash_key` and `source_id` → **Remove Columns**.
- Filter `Brand` → untick `TEST` (drops the `DO NOT USE` row).
- Filter `Category` → untick `SAP-099001` and `SAP-099002` (drops the two duplicate-name rows).

✅ `PRODUCTS` = **61 rows**, 61 distinct `ProductName`.

### 4 · Trim the product names in the fact

`order_lines` → select `ProductName` → **Transform → Format → Trim**. Two rows carry a leading/trailing space.

### 5 · Set types deliberately

`order_lines`: `Quantity` whole number; `UnitPrice`, `UnitCost`, `LineTotal` decimal; `DiscountPct` decimal; all IDs text. Do the same discipline on every query — never trust the auto-detected types.

### 6 · Find the orphans *before* trusting any total

**Home → Merge Queries →** `order_lines` as the top table, `PRODUCTS` below, join on `ProductName` → **Join Kind: Left Anti (rows only in first)** → OK. The preview now shows only fact rows with **no** matching product.

✅ **Before** trimming (step 4): **5 orphan rows.** **After** trimming: **3 rows** — `Aevo One 12`, `Lumio Pulse 6`, `Kestrel Router X1`, carrying **$2,166.10** of revenue.

Those three are genuinely discontinued — they no longer exist in the catalogue, and no amount of cleaning conjures them back. The right move is to flag them to whoever owns the product master, and note in your pitch that ~$2.2k of revenue can't be attributed to a live product. The other two orphans were only a stray space, and Trim already fixed them.

> Delete the anti-join query once you've read it — it was a diagnostic, not a model table. **Close & Apply.**

---

## Round 2 — Model

### The five relationships

Drag to create each; all **one-to-many, single cross-filter direction**, arrow pointing from the "one" side down to the fact.

| From (one) | To (many) | On |
|---|---|---|
| `PRODUCTS[ProductName]` | `order_lines[ProductName]` | product |
| `Orders[OrderID]` | `order_lines[OrderID]` | order |
| `branches[BranchID]` | `Orders[BranchID]` | branch |
| `Date[Date]` | `Orders[OrderDate]` | date |
| `Date[Date]` | `monthly_targets[TargetDate]` | month (see below) |

### The date table

**Modeling → New table:**

```
Date =
ADDCOLUMNS(
    CALENDAR(DATE(2025,1,1), DATE(2026,12,31)),
    "Year",      YEAR([Date]),
    "Month",     MONTH([Date]),
    "MonthName", FORMAT([Date], "MMM"),
    "YearMonth", FORMAT([Date], "YYYY-MM")
)
```

Then **Table tools → Mark as Date Table → `Date`**.

### Relating the targets across a grain gap

`monthly_targets[Period]` is text like `2026-03`, so it joins to nothing. Back in Power Query (or add it before Close & Apply), add a custom column on `monthly_targets`:

```
TargetDate = Date.FromText([Period] & "-01")
```

Now relate `Date[Date]` **1 → ✱** `monthly_targets[TargetDate]`. Each month's target lands on the 1st of that month, so any year/month filter picks it up — and the two tables never have to share a grain.

### Tidy

Hide every key column (`OrderID`, `BranchID`, `ProductName` on the fact, `TargetDate`) from report view. Open **Model view** and confirm there are no dashed (inactive) or bidirectional lines you didn't intend.

✅ **Five tables, five relationships.** Filters flow `Date → Orders → order_lines` and `branches → Orders → order_lines`.

> **If Power BI refused one-to-many on `PRODUCTS[ProductName]` and offered many-to-many instead, Round 1 step 3 was skipped.** `AEV-901` and `LUM-902` repeated `Aevo One 15` and `Lumio Boom Speaker`, so the name wasn't unique. Delete those two rows and the clean one-to-many appears. A relationship Power BI won't build is a dimension you haven't finished cleaning.

---

## Round 3 — Measure

Create these as measures (not calculated columns). Set the format on each in **Measure tools** as noted.

```
Total Revenue    = SUM(order_lines[LineTotal])                                    -- $ #,0.00
Total Cost       = SUMX(order_lines, order_lines[Quantity] * order_lines[UnitCost]) -- $ #,0.00
Gross Profit     = [Total Revenue] - [Total Cost]                                 -- $ #,0.00
Gross Margin %   = DIVIDE([Gross Profit], [Total Revenue])                        -- 0.0 %
Order Count      = DISTINCTCOUNT(order_lines[OrderID])                            -- #,0
Avg Order Value  = DIVIDE([Total Revenue], [Order Count])                         -- $ #,0.00
Revenue Target   = SUM(monthly_targets[TargetRevenue])                           -- $ #,0
Revenue LY       = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR('Date'[Date]))  -- $ #,0.00
Achievement %    = DIVIDE([Total Revenue], [Revenue Target])                     -- 0.0 %
```

> **Why `SUMX`, never `SUM(Quantity) * SUM(UnitCost)`.** Cost is a per-row product of quantity and unit cost; it must be summed row-by-row. Write it the wrong way and `Total Cost` returns **$312,383,281.20** — a factor of ~800 out, and it takes gross margin down with it. Whenever a measure looks absurd, check for a missing iterator first.

### Verify — the whole model

| Measure | Value |
|---|---|
| Total Revenue | **$487,289.50** |
| Total Cost | **$382,930.14** |
| Gross Profit | **$104,359.36** |
| Gross Margin % | **21.42%** |
| Order Count | **270** |
| Avg Order Value | **$1,804.78** |
| Revenue Target | **$519,200** |
| Achievement % | **93.9%** |

### Verify — by year (this is the whole exercise)

Drop `Date[Year]` on rows, the measures across, into a matrix:

| | 2025 | 2026 | Change |
|---|---|---|---|
| Revenue | $240,483.45 | $246,806.05 | **+2.6%** |
| Gross Profit | $55,707.52 | $48,651.84 | **−12.7%** |
| Gross Margin % | 23.16% | 19.71% | −3.45 pts |
| Achievement % | 97.0% | 91.0% | −6 pts |

**Revenue rose; gross profit fell by an eighth.** That contradiction is the finding. Round 4 is about naming its cause.

---

## Round 4 — Build the three-act page

Target layout is the wireframe in the lesson (`images/three-act-dashboard.svg`). Build these six things, nothing more.

**Act 1 — Context (KPI row, top).** Four **Card** (or KPI) visuals across the top. Each must carry a comparison, not a bare number:

| Card | Primary | Comparison |
|---|---|---|
| Revenue | `[Total Revenue]` → $246.8k (2026) | ▲ 2.6% vs 2025 (`[Total Revenue] - [Revenue LY]`) |
| Gross Profit | `[Gross Profit]` → $48.7k | ▼ 12.7% vs 2025 |
| Margin % | `[Gross Margin %]` → 19.7% | ▼ 3.5 pts vs 2025 |
| Achievement | `[Achievement %]` → 91% | ▼ $24.4k short of target |

Use the card's callout value + reference label, or a KPI visual with target. Colour the good comparison green, the bad ones red.

**Act 2 — Conflict (trend, middle).** A **Line chart**: axis `Date[YearMonth]` (or the date hierarchy), values `[Total Revenue]` and `[Revenue Target]`. It shows revenue holding the line all year while margin quietly erodes underneath.

**Act 3 — Resolution (cause + recommendation, bottom).**

- A **sorted bar chart**: axis `PRODUCTS[Category]`, value `[Gross Margin %]`, sorted ascending. **Phones sits at the bottom at 10.6%** — every other category is 19–41%. One colour for all bars, highlight the phones bar in a warning colour. That single visual is the cause.
- A **text box** — the written recommendation, in words, on the page:
  > **What:** Revenue grew 2.6%; gross profit fell 12.7%.
  > **Why:** Average phone discount rose from 2% to 12.8%.
  > **So what:** Phones are ~40% of sales, so it swamps every other gain.
  > **Now what:** Cap phone discount at 5% without sign-off; review the Mutare branch.

**The title states the finding.** Not "Sales Overview." Something like **"Phone discounting cost us $9.7k of gross profit in 2026."** For a title that survives slicing, drive it with a measure via Format → Title → *fx*.

**Slicers & accessibility.** Two slicers only — `Date[Year]` and `branches[BranchName]`. Set **alt text** on every visual (Format → General → Alt text). Nothing overlapping; align to the grid.

> If you're tempted by a fifth visual, you've stopped answering the MD's question. Detail (branch-by-branch, product-level) goes on a drill-through page, not the front.

---

## Round 5 — The 60-second pitch

Filled in with the real numbers, to the lesson's timing skeleton:

> **(0–10 · headline)** "We grew revenue 2.6% last year, to $247k — but gross profit fell 12.7%. The growth was bought, not earned.
> **(10–25 · conflict)** Margin dropped from 23% to 20%. That's roughly $7k of profit gone while sales went *up*.
> **(25–40 · cause)** It's phones, and only phones. *[one click: the margin-by-category bar]* Average phone discount went from 2% to nearly 13% — and phones are 40% of what we sell, so it drags everything.
> **(40–52 · recommendation)** I'd cap phone discounts at 5% without manager sign-off, and separately look at Mutare, which is down a third.
> **(52–60 · caveat)** One honest gap: April and September are missing entirely from the 2026 export, so the against-target number is a little unfair to last year. I'd confirm those before we present externally."

Do **not** narrate your Power Query steps. The finished page is the evidence.

### Bonus +10 — publish it

1. **Publish** to a real workspace (not *My Workspace*).
2. In the Service: dataset **Settings → Scheduled refresh** on, pick a daily time, and **turn on refresh-failure notifications**.
3. Open the report **as a Viewer** to check it reads for someone who wasn't in your head.

No account? Write the four-line deployment plan instead: workspace, audience + licences, refresh cadence, who gets the failure alert.

---

## The full picture — what the data was telling you

**Headline.** Revenue grew 2.6% to $246,806.05 in 2026; gross profit fell 12.7% to $48,651.84.

**Cause — phones.** Every category except phones held its margin almost exactly:

| Category | GM% 2025 | GM% 2026 | Avg discount 2025 | Avg discount 2026 |
|---|---|---|---|---|
| **Phones** | **20.4%** | **10.6%** | **2.0%** | **12.8%** |
| Computers | 18.7% | 19.3% | 1.6% | 0.9% |
| Tablets | 23.1% | 23.6% | 1.2% | 0.5% |
| Smart Home | 33.9% | 34.0% | 1.7% | 1.5% |
| Audio | 37.3% | 37.5% | 1.1% | 0.8% |
| Accessories | 41.4% | 41.0% | 1.1% | 1.7% |

Phone gross profit fell from **$19,702.14 to $9,982.54** — a **$9,719.60** loss. The rest of the business gained ~$2,700. Net: the whole company's $7,055.68 profit fall *is* phones.

**Second story (a drill page, not the front).** Harare Central **+25.5%**, Gweru **+15.9%**, but Mutare Eastgate **−33.8%** and Kwekwe **−14.7%**. Growth is concentrated in one branch while two shrink.

**The caveat.** **April and September 2026 have no orders at all** in the export, though both carry a target — so 91% achievement is unfair to 2026 and any monthly trend has two holes. Volunteering this yourself is the difference between an analyst people trust and one they double-check.

---

**Exercise:** [LESSON.md](LESSON.md) · **Boss Level answer key:** [SOLUTION_boss.md](SOLUTION_boss.md) · **Day 3 index:** [README](../README.md) · **Course home:** [README](../../../README.md)


---

*© 2026 Global Academy. Prepared for the ZIMASCO (Kwekwe) workshop. Facilitated by Tapiwa Zireva. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
