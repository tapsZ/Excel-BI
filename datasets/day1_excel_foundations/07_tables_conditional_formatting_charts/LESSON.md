---
title: "Module 7 — Tables, Conditional Formatting & Charts"
---

# Module 7 — Tables, Conditional Formatting & Charts

**Day 1 · Afternoon · ~55 minutes**
*Based on GCFGlobal/LearnFree Excel: Tables, Conditional Formatting, Charts*

---

## Learn it

## Part A — Excel Tables

A **Table** is a range you have formally told Excel is a table. It looks like formatting. It is much more than that.

Create one: click inside your data → **`Ctrl` + `T`** → confirm "My table has headers" → OK.

### Why tables are worth the two keystrokes

**1. They grow by themselves.** Type a new row directly beneath a table and it joins the table automatically — inheriting the formatting, the formulas, and crucially any chart or PivotTable built on it. With a plain range you would have to go back and fix every reference.

**2. Headers replace the column letters.** Scroll down a table and the column letters `A B C` turn into `Order ID`, `Customer`, `Sales`. No freezing needed.

**3. Formulas read in English.** Inside a table you can write:

```
=SUM(OrdersTable[Sales])
```

instead of `=SUM(H2:H46)`. This is called a **structured reference**. It says what it means, and it stretches automatically as rows are added.

**4. Built-in totals.** Table Design tab → tick **Total Row**. A total appears at the bottom with a dropdown to switch between Sum, Average, Count and so on. (It uses `SUBTOTAL` behind the scenes — the filter-aware function from Module 6.)

**5. Filters come free**, already switched on.

### Name your table

Table Design tab → the **Table Name** box at the far left. Change `Table1` to something meaningful like `OrdersTable`.

> **Do this every time.** Named tables make formulas readable, and when you connect this workbook to Power BI on Day 2 you will be picking your data source by name. `OrdersTable` is a great deal clearer than `Table1`.

---

## Part B — Conditional Formatting

Conditional formatting makes cells change appearance based on their own value. The formatting is a live rule, not a paint job — change the number and the colour follows.

**Home** tab → **Conditional Formatting**:

| Type | What it does | Good for |
|---|---|---|
| **Highlight Cells Rules** | Colours cells meeting a test (greater than, between, text contains, duplicate values) | Flagging exceptions |
| **Top/Bottom Rules** | Top 10, above average… | Finding the notable few |
| **Data Bars** | A bar inside each cell, length proportional to the value | Comparing magnitudes at a glance |
| **Colour Scales** | A colour gradient across the range | Spotting patterns in a block of numbers |
| **Icon Sets** | Arrows, traffic lights, flags | Status at a glance |

**Data Bars are the quiet star.** They turn a column of numbers into a bar chart that lives inside the cells, taking up no extra space at all.

### Managing rules

Conditional Formatting → **Manage Rules** lets you see every rule on the sheet, edit it, change its range, reorder it, or delete it. Rules apply top-down, so order matters when two rules could hit the same cell.

**Use it sparingly.** Colour draws the eye — that is the entire point. If everything is coloured, nothing stands out and you have simply made the sheet harder to read. Two or three rules per sheet is usually plenty.

---

## Part C — Charts

A chart turns numbers into a picture. Choosing the *right* picture is most of the skill.

**Insert** tab → pick a chart type. But first, know what you are choosing between:

| Chart | Use it to show | Do not use it for |
|---|---|---|
| **Column / Bar** | Comparing categories | Trends over many periods |
| **Line** | Change over time | Unrelated categories |
| **Pie** | Parts of one whole, 2–4 slices | More than ~5 slices, or comparing two pies |
| **Scatter** | Relationship between two numbers | Anything with categories on one axis |

**The rule of thumb: bars compare, lines trend, pie divides a whole.**

> **Why pie charts get a bad reputation:** humans judge lengths far more accurately than angles. With eight similar slices, nobody can tell which is biggest. A bar chart answers the same question instantly. Keep pie for a genuine part-to-whole story with a handful of slices.

### Charts need summarised data

You cannot usefully chart 45 individual order rows. First summarise — with `SUMIF` (Module 5), subtotals (Module 6), or a PivotTable (Module 8) — then chart the summary. Chart the five category totals, not the forty-five orders.

### Making a chart readable

- **Give it a real title** that states the finding: "Total Sales by Category", not "Chart 1"
- **Label the axes**, including units — "Sales (USD)"
- **Drop the legend** when there is only one series; it just repeats the title
- **Delete the clutter** — heavy gridlines, 3-D effects, drop shadows. They add ink, not meaning
- **Start value axes at zero** for bar charts. Truncating the axis exaggerates small differences and misleads the reader

That last point matters more than it sounds. It is the most common way a well-meaning chart tells a lie. We return to all of this properly on Day 3.

---

## See it — worked example

Open `practice_completed.xlsx`.

### Sheet `Orders`

The data is a real Excel Table called **OrdersTable** — click inside it and the **Table Design** tab appears with the name in the top-left box. Scroll down and watch the column letters become field names.

Three conditional formatting rules are live:

- **Data Bars** on Sales — instant visual comparison of order sizes
- **A green-yellow-red colour scale** on Quantity
- **A red highlight** on any Sales value over $1,500

Home → Conditional Formatting → **Manage Rules** shows all three with their ranges.

Try this: change a Sales figure to `5000`. The cell turns red and its data bar stretches to full width immediately. The rules are live.

### Sheet `Summary`

Five categories with `=SUMIF(Orders!F:F,A2,Orders!H:H)` pulling each total from the Orders sheet — a conditional sum from Module 5, doing real work.

Two charts sit beside it, deliberately showing the *same* data:

- A **column chart**, "Total Sales by Category"
- A **pie chart**, "Share of Sales by Category"

Look at both and ask which one lets you rank the categories faster. For most people it is the columns — you compare bar heights directly. The pie is better only at one thing: showing that the slices make up a whole.

That comparison is the lesson. The chart type is a decision, not a decoration.

---

## Do it — practice

Open **`practice_start.xlsx`** and follow the `Instructions` sheet:

**Part A — Table**
1. `Ctrl` + `T` to create the table
2. Rename it `OrdersTable`
3. Add a Total Row, set to Sum on Sales
4. Add a row at the bottom and watch the table absorb it

**Part B — Conditional formatting**
5. Data Bars on Sales
6. Colour Scale on Quantity
7. Highlight Cells Rule: Sales greater than 1500
8. Open Manage Rules and inspect all three

**Part C — Charts**
9. Build a `Summary` sheet with `=SUMIF(Orders!F:F,A2,Orders!H:H)`
10. Insert a column chart from it
11. Give it a proper title
12. Add a pie chart of the same data and compare the two

**Check yourself:** compare against `practice_completed.xlsx`.

### If you get stuck

| Problem | Why | Fix |
|---|---|---|
| `Ctrl` + `T` grabbed too much | A blank row or column broke the block | Correct the range in the dialog |
| New row doesn't join the table | You left a blank row between | Type directly under the last row |
| Conditional formatting looks wrong | Rule applied to the wrong range | Manage Rules → fix "Applies to" |
| Every cell is coloured | Threshold too low, or overlapping rules | Manage Rules → adjust or delete |
| Chart is empty | Headers were not included in the selection | Reselect including row 1 |
| Chart shows 45 bars | You charted raw rows, not a summary | Summarise first, then chart |

---

## Check your understanding

1. Name two things a Table does that a plain range does not.
2. What is a structured reference, and why is it easier to read than `H2:H46`?
3. Why should you name your table before moving on to Power BI?
4. When is a pie chart a reasonable choice — and when is it not?
5. Why is starting a bar chart's value axis above zero misleading?

---

**Previous:** [Module 6](../06_sorting_filtering_organizing/LESSON.md) · **Next:** [Module 8 — PivotTables & What-If Analysis](../08_pivottables_and_what_if_analysis/LESSON.md)


---

*© 2026 Tapiwa Zireva. Prepared for the ZIMASCO (Kwekwe) workshop. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
