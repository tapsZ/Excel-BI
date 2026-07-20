---
title: "Module 3 — Multiple Worksheets & Navigation"
---

# Module 3 — Multiple Worksheets & Navigation

**Day 1 · Morning · ~40 minutes**
*Based on GCFGlobal/LearnFree Excel: Working with Multiple Worksheets, Freezing Panes and View Options*

---

## Learn it

### Working with sheets

Sheet tabs live along the bottom of the window.

| Action | How |
|---|---|
| Add a sheet | Click the `+` |
| Rename | Double-click the tab, type, Enter |
| Colour a tab | Right-click → Tab Colour |
| Reorder | Drag the tab left or right |
| Copy a sheet | Right-click → Move or Copy → tick **Create a copy** |
| Delete | Right-click → Delete — **this cannot be undone** |

Keep names short and descriptive: `Laptop`, `Summary`, `Raw Data`. Avoid `Sheet1 (2) final v3`.

> Deleting a sheet is one of the very few actions in Excel that `Ctrl` + `Z` will not bring back. Pause before confirming.

### Referring to another sheet

This is the real skill in this module. To pull a value from another sheet, put the sheet name and an exclamation mark in front of the cell address:

```
=Laptop!F2                 one cell from the Laptop sheet
=SUM(Laptop!F:F)           the whole of column F on the Laptop sheet
='Smart Home'!F2           quotes needed - the sheet name contains a space
```

**The quote rule:** if a sheet name has a space or punctuation in it, wrap it in single quotes. Excel adds them for you automatically if you build the reference by clicking, which is the easy way:

1. Type `=SUM(`
2. **Click the other sheet's tab**
3. Select the cells you want
4. Type `)` and press Enter

Excel writes the reference — and the quotes — correctly. You end up back on your original sheet with a working formula.

### Whole-column references

`=SUM(Laptop!F:F)` adds every number in column F, however many rows there are today or tomorrow. Convenient, and safe as long as column F contains nothing but the numbers you want to add — a stray total further down the column would get counted twice.

### Freezing panes

Scroll down a long list and your headers vanish. Freezing pins them in place.

**View** tab → **Freeze Panes**:

- **Freeze Top Row** — locks row 1. The quick option.
- **Freeze First Column** — locks column A.
- **Freeze Panes** — locks everything *above and to the left of the selected cell*.

That last one is the flexible one, and the rule is worth memorising:

- Select `A2` → freezes row 1 only
- Select `B2` → freezes row 1 **and** column A — so headers stay visible across *and* down

A common mistake is selecting the row you want frozen. Select the cell *below and right of* what you want to keep.

### Other view options

- **Split** (View tab) — two independently scrolling views of one sheet, for comparing distant rows
- **New Window** — two windows onto the same workbook, handy on two monitors
- **Zoom** — bottom-right slider
- **Show/hide gridlines** — View tab, useful when building something that will be printed

---

## See it — worked example

Open `practice_completed.xlsx`.

There are seven sheets. `All Orders` holds the 40 source orders, sorted by category. Then one colour-coded sheet per category — `Laptop`, `Smartphone`, `Tablet`, `Accessory`, `Smart Home` — each holding only its own orders. Finally, `Summary`.

Go to the **Summary** sheet and click cell `B2`. The formula bar shows:

```
=SUM(Laptop!F:F)
```

That cell is on Summary but is reading the Laptop sheet. Now click `B6`:

```
=SUM('Smart Home'!F:F)
```

Same formula, but with quotes — because "Smart Home" contains a space.

`B7` is the grand total, `=SUM(B2:B6)`, adding up the five sheet totals.

Row 9 is the proof. It holds `=SUM('All Orders'!G:G)` — the total straight from the source data. **The two figures match.** That is how you check a summary: compute the same number two different ways and confirm they agree. If they ever diverge, something was missed in the split.

Now try this: go to the `Laptop` sheet and change any Sales figure. Return to Summary — both the category total and the grand total have already updated. Nothing was copied; the formulas are live links.

---

## Do it — practice

Open **`practice_start.xlsx`** and follow the `Instructions` sheet. You will:

1. Freeze the header row on `All Orders`
2. Create and colour five category sheets
3. Sort by category, then move each group onto its own sheet
4. Build a `Summary` sheet using cross-sheet `SUM` formulas
5. Add a grand total, and cross-check it against the source

**Check yourself:** your grand total must equal `=SUM('All Orders'!G:G)`. If it does not, one category is missing rows.

### If you get stuck

| Problem | Why | Fix |
|---|---|---|
| `#REF!` in a formula | The sheet was renamed or deleted | Rewrite the reference; check the tab name spelling |
| Formula shows as text | You forgot the leading `=` | Add it |
| Grand total is too low | Rows were missed when splitting | Compare row counts; check for a category you did not create a sheet for |
| Grand total is too high | Some rows were copied twice | Check for duplicates, and that no total row got included |
| Frozen the wrong rows | Wrong cell selected | View → Unfreeze Panes, select `A2`, freeze again |

---

## Check your understanding

1. Write a formula that sums column D on a sheet called `Q1 Sales`. (Careful — the name has a space.)
2. Which cell should you select before Freeze Panes if you want row 1 *and* column A both locked?
3. Why is it worth calculating a grand total two different ways?
4. Which single sheet action cannot be undone?

---

**Previous:** [Module 2](../02_formatting_cells_and_numbers/LESSON.md) · **Next:** [Module 4 — Formulas Fundamentals](../04_formulas_fundamentals/LESSON.md)


---

*© 2026 Tapiwa Zireva. Prepared for the ZIMASCO (Kwekwe) workshop. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
