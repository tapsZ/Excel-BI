---
title: "Module 7 — Conditional Formatting & Charts"
---

# Module 7 — Conditional Formatting & Charts

**Day 1 · Afternoon · ~40 minutes**
*Topics: Conditional Formatting, Charts*

**📥 Practice files:** [Start workbook](d1_m7_tables_charts_start.xlsx) · [Answer key](d1_m7_tables_charts_answers.xlsx)

---

## Learn it

Two features that turn a plain block of numbers into something a reader understands at a glance: colour that reacts to the values, and charts.

## Part A — Conditional Formatting

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

## Part B — Charts

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

You cannot usefully chart 20 individual order rows. First summarise — with `SUMIF` (Module 5), subtotals (Module 6), or a PivotTable (Module 8) — then chart the summary. Chart the five category totals, not the twenty orders.

### Making a chart readable

- **Give it a real title** that states the finding: "Total Sales by Category", not "Chart 1"
- **Label the axes**, including units — "Sales (USD)"
- **Drop the legend** when there is only one series; it just repeats the title
- **Delete the clutter** — heavy gridlines, 3-D effects, drop shadows. They add ink, not meaning
- **Start value axes at zero** for bar charts. Truncating the axis exaggerates small differences and misleads the reader

That last point matters more than it sounds. It is the most common way a well-meaning chart tells a lie. We return to all of this properly on Day 3.

---

## See it — worked example

Open `d1_m7_tables_charts_answers.xlsx`.

### Sheet `Orders`

Plain order data — 20 rows — with three conditional formatting rules live on it:

- **Data Bars** on Sales — instant visual comparison of order sizes
- **A green-yellow-red colour scale** on Quantity
- **A red highlight** on any Sales value over $1,500

Home → Conditional Formatting → **Manage Rules** shows all three with their ranges.

Try this: change a Sales figure to `5000`. The cell turns red and its data bar stretches to full width immediately. The rules are live — they react to whatever the number becomes.

### Sheet `Summary`

Five categories with `=SUMIF(Orders!F:F,A2,Orders!H:H)` pulling each total from the Orders sheet — a conditional sum from Module 5, doing real work. Because the prices are round whole numbers, you can check a total in your head: the Laptop figure should be the laptop orders added up exactly.

Two charts sit beside it, deliberately showing the *same* data:

- A **column chart**, "Total Sales by Category"
- A **pie chart**, "Share of Sales by Category"

Look at both and ask which one lets you rank the categories faster. For most people it is the columns — you compare bar heights directly. The pie is better only at one thing: showing that the slices make up a whole.

That comparison is the lesson. The chart type is a decision, not a decoration.

---

## Do it — practice

Open **`d1_m7_tables_charts_start.xlsx`** and follow the `Instructions` sheet:

**Part A — Conditional formatting**
1. Data Bars on Sales
2. Colour Scale on Quantity
3. Highlight Cells Rule: Sales greater than 1500
4. Open Manage Rules and inspect all three

**Part B — Charts**
5. Build a `Summary` sheet with `=SUMIF(Orders!F:F,A2,Orders!H:H)`, copied down the five categories
6. Insert a column chart from it
7. Give it a proper title
8. Add a pie chart of the same data and compare the two

**Check yourself:** compare against `d1_m7_tables_charts_answers.xlsx`.

### If you get stuck

| Problem | Why | Fix |
|---|---|---|
| Conditional formatting looks wrong | Rule applied to the wrong range | Manage Rules → fix "Applies to" |
| Every cell is coloured | Threshold too low, or overlapping rules | Manage Rules → adjust or delete |
| Colour didn't change when I edited a number | The cell is outside the rule's range | Manage Rules → widen "Applies to" |
| Chart is empty | Headers were not included in the selection | Reselect including row 1 |
| Chart shows 20 bars | You charted raw rows, not a summary | Summarise first, then chart |
| Pie is unreadable | Too many slices | Use a bar chart, or group small categories |

---

## Check your understanding

1. What does "the rule is live" mean for conditional formatting?
2. Why should you use only two or three conditional-formatting rules on a sheet?
3. Why can't you chart 20 individual order rows directly — what do you do first?
4. When is a pie chart a reasonable choice — and when is it not?
5. Why is starting a bar chart's value axis above zero misleading?

---

**Previous:** [Module 6](../06_sorting_filtering_organizing/LESSON.md) · **Next:** [Module 8 — PivotTables & What-If Analysis](../08_pivottables_and_what_if_analysis/LESSON.md)


---

*© 2026 Global Academy. Prepared for the ZIMASCO (Kwekwe) workshop. Facilitated by Tapiwa Zireva. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
