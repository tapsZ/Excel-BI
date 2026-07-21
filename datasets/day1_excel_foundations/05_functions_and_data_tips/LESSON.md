---
title: "Module 5 — Functions & Working with Data"
---

# Module 5 — Functions & Working with Data

**Day 1 · Afternoon · ~60 minutes**
*Topics: Functions, Basic Tips for Working with Data*

---

## Learn it

### What a function is

A function is a formula Microsoft already wrote for you. You supply the inputs; it returns the answer.

```
=SUM(H2:H21)
 ↑    ↑
 name argument (what to work on)
```

You could add 30 cells with `=H2+H3+H4+...`. `SUM` does it in one short expression that still works when the data grows.

Every function follows the same shape: `=NAME(arguments)`. Arguments are separated by commas. As you type, Excel shows a tooltip listing the arguments it expects — read it, it is genuinely helpful.

### A range

`H2:H21` means "every cell from H2 down to H31". The colon means "through". You can type it, or just drag over the cells with the mouse after typing `=SUM(`.

### The five you will use constantly

| Function | Answers | Example |
|---|---|---|
| `SUM` | What is the total? | `=SUM(H2:H21)` |
| `AVERAGE` | What is the typical value? | `=AVERAGE(H2:H21)` |
| `MAX` | What is the biggest? | `=MAX(H2:H21)` |
| `MIN` | What is the smallest? | `=MIN(H2:H21)` |
| `COUNT` | How many numbers are there? | `=COUNT(H2:H21)` |

> **`COUNT` vs `COUNTA`** — a classic trap. `COUNT` only counts **numbers**. `COUNTA` counts **anything non-empty**, including text. To count rows in a text column, `COUNT` returns 0 and `COUNTA` gives you the real answer.

**A free preview:** select a range of numbers and look at the bottom-right of the status bar. Excel already shows Count, Sum and Average. For a quick check you do not need a formula at all.

### Conditional functions — the real step up

So far every function has processed the whole range. These process only the rows that *match a condition*.

**`COUNTIF(range, criteria)`** — how many cells meet the test?

```
=COUNTIF(F2:F21,"Laptop")        how many orders were laptops
=COUNTIF(H2:H21,">1000")         how many orders exceeded $1,000
```

**`SUMIF(range, criteria, sum_range)`** — add up only the matching rows.

```
=SUMIF(F2:F21,"Laptop",H2:H21)
```

Read it left to right: *look through F2:F21; wherever you find "Laptop", add the matching value from H2:H21.*

The two ranges must be the same height and line up row for row. `F2:F21` and `H2:H21` both cover rows 2–21, so row 5's category is checked against row 5's sales figure.

**Writing criteria:**

| Criteria | Meaning |
|---|---|
| `"Laptop"` | exactly equal to Laptop |
| `">1000"` | greater than 1000 |
| `">="&B1` | greater than or equal to whatever is in B1 |
| `"<>Zimbabwe"` | **not** equal to Zimbabwe |
| `"Lap*"` | starts with "Lap" (`*` = any characters) |

Text and operators go in quotes. Cell references are joined on with `&`.

> For multiple conditions at once there are `COUNTIFS` and `SUMIFS` (note the S). You will meet `SUMIFS` in Module 8. The idea is identical, just with more pairs of range-and-criteria.

### `IF` — making Excel decide

```
=IF(test, value_if_true, value_if_false)
```

```
=IF(H2>=1000,"Large","Small")
```

*If H2 is 1,000 or more, write "Large"; otherwise write "Small".*

Text results need quotes. Numbers do not.

**Nesting `IF`** — putting another `IF` where the false answer goes, to handle a third case:

```
=IF(H2>=1000,"Large",IF(H2>=300,"Medium","Small"))
```

Excel checks in order and stops at the first match:

- `H2` is 1500 → Large
- `H2` is 500 → not ≥ 1000, so try the next test → ≥ 300 → Medium
- `H2` is 100 → neither → Small

**Order matters when nesting.** Test the most extreme case first. Reverse these two and everything ≥ 300 would be labelled Medium, because the first test would catch it before the second ever ran.

### Handling errors gracefully

```
=IFERROR(A1/B1,0)
```

Returns `A1/B1`, but shows `0` instead of `#DIV/0!` if it fails. Very useful on reports that other people will see.

Use it to tidy a finished report — not while you are still building it, where a visible error is a useful clue.

### Practical habits

- **`Ctrl` + `Z`** undoes almost anything
- **Select an entire column** by clicking its letter
- Watch the **status bar** for instant Sum/Count/Average
- Keep to a **flat table**: one row per record, one column per field, no blank rows in the middle
- Blank rows break `Ctrl` + arrow navigation, sorting, filtering and PivotTables — leave them out

---

## See it — worked example

Open `d1_m5_functions_answers.xlsx`, sheet `Orders`. Twenty orders, with a Summary Statistics panel in columns K and L.

Click each cell in column L and read the formula bar:

| Cell | Formula | Idea |
|---|---|---|
| `L2` | `=SUM(H2:H21)` | total sales |
| `L3` | `=AVERAGE(H2:H21)` | typical order |
| `L4` | `=MAX(H2:H21)` | biggest order |
| `L5` | `=MIN(H2:H21)` | smallest order |
| `L6` | `=COUNT(H2:H21)` | how many orders |
| `L7` | `=SUM(G2:G21)` | total units — a different column |
| `L8` | `=COUNTIF(F2:F21,"Laptop")` | **counting with a condition** |
| `L9` | `=SUMIF(F2:F21,"Laptop",H2:H21)` | **adding with a condition** |
| `L10` | `=COUNTIF(I2:I21,"Large")` | counting the results of your own `IF` |
| `L11` | `=SUMIF(D2:D21,"Zimbabwe",H2:H21)` | conditional on a *different* column |

Then look at column I, `Order Size`. Every cell holds:

```
=IF(H2>=1000,"Large",IF(H2>=300,"Medium","Small"))
```

Compare a few rows against their Sales figures and satisfy yourself the labels are right.

Notice what `L10` does: it counts the output of a formula in column I. **Formulas can read other formulas.** That is how a small model gets built — each layer sits on the one below.

One more thing worth seeing: `L2` (total of everything) is larger than `L9` (laptops only), and `L6` (all orders) is larger than `L8` (laptop orders). Sanity checks like these catch mistakes early. A conditional total should never exceed its unconditional one.

---

## Do it — practice

Open **`d1_m5_functions_start.xlsx`**. Column L and column I are empty. Follow the `Instructions` sheet:

1. `L2:L6` — `SUM`, `AVERAGE`, `MAX`, `MIN`, `COUNT` on `H2:H21`
2. `L7` — total units from column G
3. `L8` — `COUNTIF` for laptops; `L9` — `SUMIF` for laptop sales
4. Column I — the nested `IF` that labels each order Large / Medium / Small, copied down
5. `L10` — count your "Large" labels
6. `L11` — `SUMIF` on the Country column

**Check yourself:** every figure in column L should match `d1_m5_functions_answers.xlsx`. If one is off, click that cell and read the formula bar — the mistake is visible there.

### If you get stuck

| Problem | Why | Fix |
|---|---|---|
| `#NAME?` | Function name misspelled | Check spelling; Excel autocompletes if you let it |
| `COUNT` returns 0 | The column holds text | Use `COUNTA` |
| `SUMIF` returns 0 | Criteria doesn't match exactly — a trailing space, or wrong spelling | Check the actual cell value |
| `SUMIF` gives a strange number | The two ranges are different sizes | Make both cover the same rows |
| Nested `IF` labels everything Medium | Tests are in the wrong order | Test the largest threshold first |
| `#VALUE!` | Text where a number is expected | Find the offending cell |

---

## Check your understanding

1. When does `COUNT` give the wrong answer, and what should you use instead?
2. In `=SUMIF(F2:F21,"Laptop",H2:H21)`, what is each of the three arguments for?
3. Why must `=IF(H2>=1000,"Large",IF(H2>=300,"Medium","Small"))` test 1000 before 300?
4. Write a formula counting orders **over** $500 in `H2:H21`.
5. Your `SUMIF` for "Laptop" returns a bigger number than your `SUM` of all sales. What must be wrong?

---

**Previous:** [Module 4](../04_formulas_fundamentals/LESSON.md) · **Next:** [Module 6 — Sorting, Filtering & Organizing Data](../06_sorting_filtering_organizing/LESSON.md)


---

*© 2026 Global Academy. Prepared for the ZIMASCO (Kwekwe) workshop. Facilitated by Tapiwa Zireva. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
