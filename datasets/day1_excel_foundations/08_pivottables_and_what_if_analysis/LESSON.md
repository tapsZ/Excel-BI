---
title: "Module 8 — PivotTables & What-If Analysis"
---

# Module 8 — PivotTables & What-If Analysis

**Day 1 · Afternoon · ~60 minutes**
*Topics: Intro to PivotTables, Doing More with PivotTables, What-If Analysis*

> **This module is the bridge to Day 2.** A PivotTable is Excel's version of the idea that Power BI is built entirely around: take detailed rows, group them, and summarise them — interactively. Understand the PivotTable and Power BI will feel familiar rather than foreign.

---

## Learn it

## Part A — PivotTables

### The problem

You have 20 orders. Even 20 rows is slow to total by hand — category by category, year by year — and real data has thousands. They want to know: *which category earns the most, and is it growing?*

You could answer that with `SUMIF` — one formula per category, rewritten every time someone asks a slightly different question. A PivotTable answers all such questions by dragging fields.

### What a PivotTable does

It takes your detailed rows and **groups them**, then **summarises** each group. That is genuinely all. Everything else is options.

### Creating one

1. Click any cell in your data
2. **Insert** tab → **PivotTable** → New Worksheet → OK
3. You get an empty pivot and a **field list** with four drop zones

### The four zones

| Zone | Holds | Effect |
|---|---|---|
| **Rows** | What to list down the side | One row per unique value |
| **Columns** | What to list across the top | One column per unique value |
| **Values** | What to calculate | The numbers in the middle |
| **Filters** | What to slice by | A dropdown above the pivot |

Drag `Category` → Rows and `Sales` → Values, and you have total sales per category. Two drags.

Add `Year` → Columns and it becomes a **cross-tab**: categories down, years across, sales in the middle.

### Changing what the numbers mean

Right-click any value → **Summarize Values By** → Sum, Count, Average, Max, Min.

- **Sum of Sales** = how much money
- **Count of Sales** = how many orders

Two completely different questions, one right-click apart.

> **If your Values field says "Count" when you expected "Sum",** the column contains text somewhere — often a blank or a stray label. Excel defaults to Count for text. Fix the source data rather than the pivot.

### Show Values As

Right-click a value → **Show Values As** → **% of Grand Total**. Now every figure is a share rather than an amount. Also available: % of Row, % of Column, Running Total, Difference From. This turns a table of amounts into a table of *proportions* without touching a single formula.

### Slicers

**PivotTable Analyze** tab → **Insert Slicer** → tick a field → OK.

A slicer is a box of clickable buttons that filters the pivot. It does the same job as the Filters zone but visibly, so anyone can use it without knowing how a pivot works.

**Remember slicers.** They are exactly what you will use in Power BI on Days 2 and 3 — same concept, same name, same behaviour.

### PivotCharts

**PivotTable Analyze** → **PivotChart**. The chart links to the pivot: filter the pivot and the chart follows; click the chart and the pivot follows. A one-page interactive dashboard, in Excel.

### Refreshing

**A PivotTable does not update by itself.** Change the source data and you must right-click the pivot → **Refresh** (or PivotTable Analyze → Refresh).

This surprises everyone once. If a pivot disagrees with your source, refresh before you start debugging.

> **Building your source properly matters.** A PivotTable needs a tidy, flat block of data: one row per record, one column per field, headers on row 1, no blank rows, no merged cells. This is the same rule from Module 1, and this is where breaking it finally bites.

---

## Part B — What-If Analysis

PivotTables answer *what happened*. What-If answers *what might happen* — and, better, *what would we have to do*.

**Data** tab → **What-If Analysis**.

### Goal Seek — the useful one

Normally Excel works **forwards**: you type the inputs, and a formula gives you the answer.

**Goal Seek works backwards.** You tell Excel the answer you *want*, and it finds the input that gets you there.

**A simple example.** Laptops sell for **$1,000** each. You want **$10,000** in laptop sales. How many must you sell? You could work it out — 10 — but Goal Seek does it for you, even when the maths is too messy to do in your head.

> **Where the cells live.** In the practice workbook this all sits on a sheet called **`What-If`** — a separate sheet from `Orders`, with just three cells:
>
> | Cell | Meaning | Holds |
> |---|---|---|
> | `B4` | Laptops to sell | a plain number (starts at 5) |
> | `B5` | Price each | 1000 |
> | `B7` | **Total sales** | the formula `=B4*B5` |

To find how many laptops reach $10,000, open **Data → What-If Analysis → Goal Seek** and fill in three boxes:

```
Set cell:      B7      (Total sales — the cell with the formula)
To value:      10000   (the answer you want)
By changing:   B4      (Laptops to sell — a plain number)
```

Click OK and Excel puts **10** in `B4` — because 10 × 1,000 = 10,000. That's it: *you knew the result you wanted, and Excel found the input.*

The three boxes, in plain words:

| Box | What it is |
|---|---|
| **Set cell** | the answer you want to control — it must hold a **formula** (here, `B7`) |
| **To value** | the number you want that answer to become (here, `10000`) |
| **By changing** | the input Excel is allowed to adjust — it must be a **plain number**, not a formula (here, `B4`) |

Two rules that follow from this: the **Set cell** must contain a formula, and the **By changing** cell must be a plain number that actually feeds into that formula. Goal Seek changes one input to hit one target — that is its limit, and within it, it is excellent.

### Now on real data — where Goal Seek earns its keep

The laptop example was easy: you could do it in your head. Here's the version you can't.

Lower down on the same `What-If` sheet is a second little model that uses **our actual order data**:

| Cell | Meaning | Holds |
|---|---|---|
| `B12` | Current total sales | `=SUM(Orders!J2:J21)` — this adds the Sales column on the **Orders sheet**, giving our real total of **$17,640** |
| `B13` | Sales growth | a plain number (a %, starts at 0) |
| `B14` | **Projected total** | `=B12*(1+B13)` |

Management wants total sales of **$20,000**. If every sale grew by the same percentage, *what growth rate hits the target?*

You can't do `20,000 ÷ 17,640` in your head — but Goal Seek can:

```
Set cell:      B14      (Projected total — the formula)
To value:      20000    (the target)
By changing:   B13      (Sales growth — the plain-number input)
```

Excel finds **about 13.4%**. That is the point of Goal Seek: on real numbers, with no tidy answer, it works backwards to the exact input in a second.

### Scenario Manager

Save several named sets of input values and switch between them.

Data → What-If Analysis → **Scenario Manager** → Add. Name it ("Modest"), pick the changing cell (`B4`), enter a value (say 8). Repeat for another ("Ambitious", 15). Then **Show** to apply one, or **Summary** to generate a side-by-side comparison of the Total sales each one gives.

This is how you present options to a decision-maker: not one number, but two or three, clearly labelled.

### Data Tables

Data → What-If Analysis → **Data Table** produces a grid showing how a result changes across many input values at once — one variable down the side, optionally a second across the top. Powerful for sensitivity analysis. Worth knowing it exists; Goal Seek and Scenario Manager will cover most of your needs.

---

## See it — worked example

Open `d1_m8_pivottables_answers.xlsx`. Three sheets.

### `Orders`

All 20 orders. Note the extra **Year** and **Month** columns — derived from the order date so they can be used as pivot fields. Pivots group by whatever columns exist, so adding a Year column up front is a small piece of preparation that pays off immediately. (In Power BI a proper date table does this job, as you will see on Day 2.)

### `Pivot Result`

This shows the numbers **your PivotTable should produce**: total sales by category down the side, by year across the top, with row and column grand totals.

It is built with `SUMIFS` rather than an actual PivotTable, deliberately — so you can click a cell and see the logic:

```
=SUMIFS(Orders!$J$2:$J$21, Orders!$H$2:$H$21, $A5, Orders!$C$2:$C$21, 2024)
```

Read it as: *add up column J (Sales), where column H (Category) matches the category on this row, and column C (Year) is 2024.*

`SUMIFS` is `SUMIF` with more conditions — the sum range comes **first**, then each range-and-criteria pair.

**This is exactly what a PivotTable does internally.** Every cell in a pivot is a conditional sum, one per row/column intersection. The pivot writes those for you when you drag. Once you have seen that, a PivotTable stops being magic.

Note the absolute references (`$J$2:$J$21`) so the formula can be copied across the grid, and the relative `$A5` so each row picks up its own category — relative and absolute from Module 4, doing exactly the job they were introduced for.

### `What-If`

A tiny, self-contained planner — everything is on this one sheet:

- **Yellow cells** — the inputs: `B4` laptops to sell, `B5` price each (1000)
- **Green cell** — the result: `B7` total sales, holding `=B4*B5`

Change `B4` (laptops) and watch `B7` (total sales) update — that's the *forward* direction. Then use Goal Seek to run it *backwards*: set `B7` to `10000` and let Excel find the laptops (`B4` = 10). One multiplication, so you can check it in your head: 10 × 1,000 = 10,000.

Lower down, a second block uses the **real order total**: `B12` = `=SUM(Orders!J2:J21)` = $17,640, `B13` a growth %, `B14` the projected total. Goal Seek there sets `B14` to `20000` by changing `B13` and lands on **~13.4%** — the answer you couldn't have guessed.

---

## Do it — practice

Open **`d1_m8_pivottables_start.xlsx`** and follow the `Instructions` sheet.

**Part A — your first PivotTable**
1. Insert → PivotTable → New Worksheet
2. `Category` → Rows, `Sales` → Values
3. Format the values as Currency

**Part B — go deeper**
4. `Year` → Columns (now a cross-tab)
5. `Country` → Filters
6. Switch the value to Count, then back to Sum
7. Show Values As → % of Grand Total
8. Insert a Slicer on Category and click the buttons
9. Insert a PivotChart

**Part C — What-If (all on the `What-If` sheet)**
10. Change `B4` (laptops to sell) and watch `B7` (total sales) update — the forward direction
11. **Goal Seek (simple):** set `B7` to `10000` by changing `B4` — the answer is `10`
12. **Goal Seek (real data):** lower down, set `B14` to `20000` by changing `B13` — the answer is about **13.4%**
13. **Scenario Manager:** add "Modest" (`B4` = 8) and "Ambitious" (`B4` = 15), then produce a Summary

**Check yourself:** your pivot's category and year totals should match the `Pivot Result` sheet in `d1_m8_pivottables_answers.xlsx`.

### If you get stuck

| Problem | Why | Fix |
|---|---|---|
| Values show Count, not Sum | Text or blanks in the Sales column | Clean the source; right-click → Summarize Values By → Sum |
| Pivot doesn't reflect new data | Pivots don't auto-update | Right-click → Refresh |
| "Field name not valid" | A header cell is blank | Fill in every header |
| Goal Seek: "cell must contain a value" | You pointed "By changing" at a formula | It must be a plain number |
| Goal Seek finds no solution | The target is unreachable | Try a nearer target |
| Slicer greys out buttons | Another filter already excludes them | Clear the other filters |

---

## Check your understanding

1. What do the Rows, Columns, Values and Filters zones each control?
2. Your Values field defaults to Count instead of Sum. What does that tell you about the source data?
3. Why does a PivotTable need refreshing?
4. Explain what `=SUMIFS(J:J, H:H, "Laptop", C:C, 2024)` returns.
5. Goal Seek requires the "Set cell" to hold a formula and the "By changing" cell to hold a plain number. Why?
6. Which feature would you use to compare a cautious, expected and aggressive forecast side by side?

---

## You have finished Day 1

Look at what you can now do. You started by typing into a cell. You can now clean and format a dataset, write formulas that reference each other, use conditional functions, sort and filter and subtotal, apply conditional formatting and build charts, summarise every order with a PivotTable, and work backwards from a target with Goal Seek.

**Now do the [Day 1 homework](../homework/HOMEWORK.md)** — a fresh dataset and about 30 minutes to prove all of it sticks.

Then, on Day 2, we finish Excel properly — `XLOOKUP`, `INDEX`/`MATCH`, dynamic arrays, Power Query and Power Pivot — before making the move to Power BI. Several ideas from today reappear almost unchanged there: slicers, relationships between tables, and summarising detail into insight. You have already met them.

---

**Previous:** [Module 7](../07_tables_conditional_formatting_charts/LESSON.md) · **Next:** [Day 1 Homework](../homework/HOMEWORK.md)


---

*© 2026 Global Academy. Prepared for the ZIMASCO (Kwekwe) workshop. Facilitated by Tapiwa Zireva. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
