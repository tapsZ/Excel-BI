---
title: "Module 6 — Sorting, Filtering & Organizing Data"
---

# Module 6 — Sorting, Filtering & Organizing Data

**Day 1 · Afternoon · ~45 minutes**
*Topics: Sorting Data, Filtering Data, Groups and Subtotals*

---

## Learn it

### Sorting

Sorting rearranges rows into order.

**One column:** click any cell in the data, then **Data** tab → **Sort A to Z** (or Z to A). For numbers this reads as smallest-to-largest or largest-to-smallest.

> **The classic disaster:** selecting *only* one column before sorting. Excel sorts that column and leaves every other column where it was — silently scrambling which customer bought what. If Excel warns you about "Sort Warning", always choose **Expand the selection**.
>
> Safest habit: click **one single cell** inside the data and let Excel work out the range itself.

**Multiple columns** — Data tab → **Sort** (the button with the dialog):

```
Sort by      Category    A to Z
Then by      Sales       Largest to Smallest
```

Rows group by category, and inside each category the biggest order comes first. Levels apply in order, top to bottom.

**Custom order** — in the Sort dialog, Order → Custom List. Useful for things with a natural sequence that is not alphabetical: `Low, Medium, High` rather than `High, Low, Medium`.

### Filtering

Filtering **hides** rows that do not interest you. Nothing is deleted — hidden rows come straight back.

Turn it on: click inside the data → **Data** tab → **Filter**. Dropdown arrows appear on every header.

Click an arrow and you can:

- Untick **Select All**, then tick only the values you want
- Use the search box for long lists
- **Number Filters** → Greater Than, Between, Top 10…
- **Text Filters** → Contains, Begins With…
- **Date Filters** → This Month, Last Year, Between…

**Filters stack.** Filter Category to Laptop *and* Country to Zimbabwe and you see only rows meeting both.

Two things to watch:

- A funnel icon on a header means that column is filtering. If your numbers look wrong, check for stray funnels.
- The status bar reports "*x* of *y* records found" — a quick count for free.

Clear one column from its own dropdown → Clear Filter; clear everything with Data → **Clear**.

### Subtotals

Subtotals insert automatic total rows into a sorted list.

> **You must sort first.** Subtotal breaks the data wherever the chosen column *changes value*. On unsorted data you get a mess of tiny meaningless groups. Sort by the grouping column, then subtotal.

Steps: sort by Category → **Data** tab → **Subtotal** →

```
At each change in:  Category
Use function:       Sum
Add subtotal to:    Sales
```

Excel inserts a total row after each category and a grand total at the bottom.

### The outline buttons

Subtotals add small `1 2 3` buttons at the top-left.

| Button | Shows |
|---|---|
| `1` | Grand total only |
| `2` | Category totals only — **an instant summary report** |
| `3` | Everything |

Level `2` is the useful one: it collapses hundreds of rows into a handful of totals in one click.

To remove subtotals: Data → Subtotal → **Remove All**.

### `SUBTOTAL` — the function underneath

Look at a total row Excel created and you will see something unexpected:

```
=SUBTOTAL(9,H2:H12)
```

Not `SUM`. The `9` means "sum" (there are other codes: `1` average, `2` count, `4` max, `5` min).

`SUBTOTAL` behaves differently from `SUM` in two ways that matter:

1. **It ignores rows hidden by a filter.** Filter to one country and the total updates to reflect only what you can see. `SUM` would stubbornly keep totalling everything.
2. **It ignores other `SUBTOTAL` cells inside its range.** That is how the grand total at the bottom avoids double-counting the category totals above it.

That second behaviour is genuinely clever, and it is why Excel uses `SUBTOTAL` here rather than `SUM`.

---

## See it — worked example

Open `d1_m6_sorting_answers.xlsx`, sheet `Orders`.

The 20 orders are sorted by **Category** (A→Z), and within each category by **Sales** (largest first). Under each category sits a shaded total row, and a gold grand total at the very bottom.

Click the **Laptop Total** in column H (row 6). The formula bar shows:

```
=SUBTOTAL(9,H2:H5)
```

Now click the grand total (row 27):

```
=SUBTOTAL(9,H2:H26)
```

Look carefully at that range — it spans the *whole* table, all five category subtotal rows included. Yet the grand total is correct, not double. That is `SUBTOTAL` ignoring its own kind.

Filters are already switched on. Try this:

1. Filter **Category** to Laptop only
2. Watch the grand total drop to the laptop figure alone
3. Clear the filter and it returns

That is a live, filter-aware report — and you built it with two menu commands.

A note at the bottom of the sheet restates the `SUBTOTAL` rule for reference.

---

## Do it — practice

Open **`d1_m6_sorting_start.xlsx`** and follow the `Instructions` sheet:

1. Sort by Sales, largest first — who placed the biggest order?
2. Sort by Category, then Sales descending (two levels)
3. Turn on filters
4. Answer with filters: how many laptop orders? how many US orders? how many over $1,000?
5. Add subtotals by Category, summing Sales
6. Use the `1 2 3` outline buttons — click `2` for an instant summary

**Check yourself:** compare your category totals against `d1_m6_sorting_answers.xlsx`.

### If you get stuck

| Problem | Why | Fix |
|---|---|---|
| Data scrambled after sorting | Only one column was selected | `Ctrl` + `Z` immediately; reselect a single cell and re-sort |
| Subtotals in odd places | Data wasn't sorted first | Remove All, sort, subtotal again |
| Rows missing | A filter is still active | Data → Clear; look for funnel icons |
| Grand total looks doubled | You used `SUM` over a range containing subtotals | Use `SUBTOTAL(9,…)` |
| Total ignores hidden rows unexpectedly | That is `SUBTOTAL` working as designed | Use `SUM` if you want all rows regardless |

---

## Check your understanding

1. What goes wrong if you select one column before sorting, and how does Excel try to warn you?
2. Why must data be sorted before applying Subtotal?
3. Name the two ways `SUBTOTAL(9,…)` differs from `SUM`.
4. Which outline button gives you a one-line-per-category summary?
5. Does filtering delete rows?

---

**Previous:** [Module 5](../05_functions_and_data_tips/LESSON.md) · **Next:** [Module 7 — Conditional Formatting & Charts](../07_tables_conditional_formatting_charts/LESSON.md)


---

*© 2026 Global Academy. Prepared for the ZIMASCO (Kwekwe) workshop. Facilitated by Tapiwa Zireva. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
