---
title: "Day 2 · Module 1 — Advanced Formulas: Lookups, Logic & Dynamic Arrays"
---

# Day 2 · Module 1 — Advanced Formulas: Lookups, Logic & Dynamic Arrays

**Morning · ~75 minutes**
*Topics: XLOOKUP, INDEX/MATCH, logical and conditional formulas, dynamic arrays, Tables and Named Ranges*

> **Prerequisite:** Day 1, especially [Module 4 — relative vs absolute references](../../day1_excel_foundations/04_formulas_fundamentals/LESSON.md). Every formula here depends on knowing when to lock a reference.

---

## Learn it

### The problem lookups solve

Real data almost never lives in one table. Your order log records *which* customer and *which* product — as IDs. The names, countries, categories and prices live elsewhere.

```
Orders:    O0001 | C012 | NT-018 | 1
Customers: C012  | Amara Otieno | Kenya | Nairobi
Products:  NT-018| iPhone 16 | Smartphone | $1,000
```

A **lookup** function bridges that gap: *"take this ID, go and find its matching row in the other table, bring back one column."*

This is the single most valuable formula skill in Excel — and it is also, not coincidentally, exactly what a **relationship** does in Power BI. Learn it properly here and this afternoon will make immediate sense.

### `XLOOKUP` — the modern answer

```
=XLOOKUP(lookup_value, lookup_array, return_array, [if_not_found])
```

```
=XLOOKUP(C2, Customers!$A$2:$A$13, Customers!$B$2:$B$13, "Not found")
```

Read it as: *take the value in `C2`; look for it in the Customers ID column; return the matching value from the Customers name column; if it isn't there, say "Not found".*

Three things to notice:

1. **The lookup ranges are absolute** (`$A$2:$A$13`). The value being looked up (`C2`) is relative so it moves down the rows — the ranges must not. Day 1 Module 4, doing real work.
2. **The two ranges must be the same height.** Row 5 of the lookup array corresponds to row 5 of the return array.
3. **Always supply the fourth argument.** Without it a missing match returns `#N/A`, which then poisons every total downstream. `"Not found"` or `0` keeps your report readable.

`XLOOKUP` replaced `VLOOKUP`, and it is better in ways that matter:

| | `VLOOKUP` | `XLOOKUP` |
|---|---|---|
| Return column | Counted by number — breaks when a column is inserted | Named directly |
| Look leftwards | Cannot | Yes |
| Default match | **Approximate** — a notorious source of silent wrong answers | Exact |
| Not-found handling | Needs wrapping in `IFERROR` | Built in |

> **If you inherit a workbook full of `VLOOKUP`**, check for a `FALSE` as the last argument. Without it, `VLOOKUP` matches *approximately* and will happily return the wrong row without any error at all. It is one of the most common causes of quietly incorrect spreadsheets in the wild.

### `INDEX` / `MATCH` — the one that works everywhere

`XLOOKUP` needs Microsoft 365 or Excel 2021+. Plenty of organisations are on older versions, and you will meet this pairing constantly in existing files:

```
=INDEX(Products!$C$2:$C$19, MATCH(D2, Products!$B$2:$B$19, 0))
```

Take it in two halves, inside out:

- **`MATCH(D2, Products!$B$2:$B$19, 0)`** → *which row* is this product on? Returns a number, e.g. `7`.
- **`INDEX(Products!$C$2:$C$19, 7)`** → give me the 7th value from the Category column.

**The `0` in `MATCH` means exact match.** Always include it. Left out, `MATCH` assumes your data is sorted and returns approximate results — the same silent-failure trap as `VLOOKUP`.

`INDEX`/`MATCH` looks clumsier but it is genuinely robust: it can look left, and inserting columns doesn't break it. Learn both. Use `XLOOKUP` when you can, read `INDEX`/`MATCH` when you must.

### Logical formulas: `IFS`

Day 1 used nested `IF`. For more than two or three conditions it becomes hard to read:

```
=IF(J2>=2000,"Platinum",IF(J2>=1000,"Gold",IF(J2>=300,"Silver","Bronze")))
```

`IFS` says the same thing flatly:

```
=IFS(J2>=2000,"Platinum", J2>=1000,"Gold", J2>=300,"Silver", TRUE,"Bronze")
```

Test, result, test, result — checked top to bottom, stopping at the first `TRUE`.

**The final `TRUE` is the catch-all.** `TRUE` is always true, so it fires when nothing above matched. Leave it out and unmatched rows return `#N/A`.

**Order still matters.** Put the highest threshold first, exactly as with nested `IF`.

Also worth knowing: `AND(a,b)` (all must be true), `OR(a,b)` (any), `NOT(a)`, and `IFERROR(formula, fallback)` for tidying a finished report.

### Named Ranges

A **named range** gives a fixed range a human name. Formulas tab → **Name Manager** → New, or select the range and type a name into the Name Box.

Compare:

```
=XLOOKUP(K2, Tiers!$A$4:$A$7, Tiers!$C$4:$C$7, 0)
=XLOOKUP(K2, TierName, TierDiscount, 0)
```

The second explains itself. Named ranges are already absolute, so there are no dollar signs to forget, and if the range needs to move you update it in one place instead of in fifty formulas.

Use them for anything referenced repeatedly: rate tables, lookup tables, key inputs.

### Tables (a reminder, and why it matters more now)

Day 1 Module 7 introduced `Ctrl` + `T`. On Day 2 it stops being cosmetic:

- Structured references — `=SUM(Orders[Sales])` instead of `=SUM(H2:H101)`
- **Tables grow automatically**, so lookups against a table keep working as rows arrive
- **Power Query and Power BI both prefer named tables as sources**

Name every table you create. `Orders` and `Products` beat `Table1` and `Table3` — especially this afternoon, when you will be selecting them by name in Power BI.

### Dynamic arrays

*(Microsoft 365 / Excel 2021+. Skip if `#NAME?` appears — the older techniques above still cover you.)*

Traditionally one formula filled one cell. Dynamic arrays let one formula fill *many* cells — it **spills** into the space it needs.

| Function | Does |
|---|---|
| `UNIQUE(range)` | The distinct values, duplicates removed |
| `SORT(range, col, order)` | Sorted; `-1` for descending |
| `FILTER(range, condition, if_empty)` | Only the matching rows |
| `SEQUENCE(n)` | A list of numbers |

```
=UNIQUE(Products!C2:C19)                                    every distinct category
=SORT(Products!B2:D19, 3, -1)                               most expensive first
=FILTER(Products!B2:D19, Products!C2:C19="Laptop", "none")  laptops only
```

They **nest**, which is where they become powerful:

```
=SORT(FILTER(Products!B2:D19, Products!D2:D19>500, "none"), 3, -1)
```

*Filter to products over $500, then sort by price descending* — a live, self-updating report from one formula.

Things to know about spilling:

- Only the **top-left cell** holds the formula; the rest is spill output and cannot be edited
- **`#SPILL!`** means something is blocking the output area — clear the cells below/right
- The `#` suffix refers to a whole spill range: `=SUM(F2#)`

---

## See it — worked example

Open `d2_m1_advanced_formulas_answers.xlsx`. Five sheets: `Orders`, `Products`, `Customers`, `Tiers`, `Dynamic Arrays`.

The `Orders` sheet starts with only IDs — `Customer ID` and `Product`. Columns F to M are all built by formula. Click across row 2 and read the formula bar:

| Cell | Formula | Technique |
|---|---|---|
| `F2` | `=XLOOKUP(C2,Customers!$A$2:$A$13,Customers!$B$2:$B$13,"Not found")` | XLOOKUP |
| `G2` | `=XLOOKUP(C2,Customers!$A$2:$A$13,Customers!$C$2:$C$13,"Not found")` | same lookup, different return column |
| `H2` | `=INDEX(Products!$C$2:$C$19,MATCH(D2,Products!$B$2:$B$19,0))` | INDEX/MATCH |
| `I2` | `=XLOOKUP(D2,Products!$B$2:$B$19,Products!$D$2:$D$19,0)` | XLOOKUP on product |
| `J2` | `=I2*E2` | plain arithmetic |
| `K2` | `=IFS(J2>=2000,"Platinum",…,TRUE,"Bronze")` | IFS |
| `L2` | `=XLOOKUP(K2,TierName,TierDiscount,0)` | **named ranges** |
| `M2` | `=J2*(1-L2)` | applies the discount |

Notice how `L2` reads almost as English. That is the named-range payoff.

Look at what has been built: three separate tables joined into one enriched view, with a rule-driven tier and discount applied — and not a single value typed twice.

**Now prove it is a model.** Go to the `Tiers` sheet and change the Gold discount from 10% to 20%. Every Gold order on the `Orders` sheet updates. The tier rules live in one small table, not scattered through formulas.

On the `Dynamic Arrays` sheet, click the top-left cell of each result block. One formula; many rows of output. Click a cell *below* it — empty formula bar, because that cell is spill output.

> **The idea to carry into this afternoon:** what you just did with `XLOOKUP` — joining Orders to Products and Customers — is precisely what Power BI does with a **relationship**. There, you draw a line between two tables once, and every visual honours it. No formula copied down 25 rows. Same concept, less work.

---

## Do it — practice

Open **`d2_m1_advanced_formulas_start.xlsx`** and follow the `Instructions` sheet. Columns F–M are empty.

1. `F2` — `XLOOKUP` for Customer Name; copy down
2. `G2` — `XLOOKUP` for Country
3. `H2` — `INDEX`/`MATCH` for Category
4. `I2` — `XLOOKUP` for List Price
5. `J2` — Revenue = price × quantity
6. `K2` — `IFS` for the tier
7. Inspect the named ranges via Formulas → Name Manager
8. `L2` — `XLOOKUP` using `TierName` and `TierDiscount`
9. `M2` — Net Revenue
10. Change the Gold discount on `Tiers` and confirm everything moves
11. On `Dynamic Arrays`, type each formula from column F

### If you get stuck

| Problem | Why | Fix |
|---|---|---|
| `#N/A` | No match found | Check for trailing spaces or case differences; add the not-found argument |
| Results shift wrongly as you copy down | Missing `$` on lookup ranges | Lock them: `$A$2:$A$13` |
| `#NAME?` on `XLOOKUP`/`IFS`/`FILTER` | Excel version too old | Use `INDEX`/`MATCH` and nested `IF` |
| `#SPILL!` | Something blocks the spill area | Clear the cells below and to the right |
| `#REF!` inside `INDEX` | `MATCH` returned a row outside the range | Check both ranges cover the same rows |
| Everything says "Bronze" | `IFS` conditions in the wrong order | Highest threshold first |

---

## Check your understanding

1. Why is `XLOOKUP`'s fourth argument worth supplying every time?
2. Explain what `MATCH` returns, and what `INDEX` then does with it.
3. Why is a `VLOOKUP` without `FALSE` dangerous?
4. What does the final `TRUE` do in an `IFS`?
5. Give two reasons to use a named range instead of `Tiers!$C$4:$C$7`.
6. What does `#SPILL!` mean?
7. **Looking ahead:** which Power BI feature does an `XLOOKUP` between two tables correspond to?

---

**Next:** [Module 2 — Power Query & Power Pivot in Excel](../02_excel_power_query_and_power_pivot/LESSON.md) · **Day 2 index:** [README](../README.md)


---

*© 2026 Global Academy. Prepared for the ZIMASCO (Kwekwe) workshop. Facilitated by Tapiwa Zireva. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
