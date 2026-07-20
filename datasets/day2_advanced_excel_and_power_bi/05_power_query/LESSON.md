---
title: "Day 2 · Module 5 — Data Cleaning & Transformation in Power BI"
---

# Day 2 · Module 5 — Data Cleaning & Transformation in Power BI

**Afternoon · ~50 minutes**
*Topics: the Power Query Editor in Power BI — cleaning, appending, merging, unpivoting*

**Finished example:** `powerbi_files/day2_advanced_excel_and_power_bi/Data Cleaning (Power Query).pbix`
**Data:** `orders_jan.csv`, `orders_feb.csv`, `sales_flattable.csv`, `sales_monthly.csv` (this folder)

---

## Learn it

### The good news

**This is the same Power Query you used this morning.** Same editor, same Applied Steps panel, same ribbon, same M language. Microsoft built it once and put it in both products.

So this module is mostly practice, with three additions worth knowing.

Quick recap of the rules that matter:

1. **Set data types first.** Most Power Query problems are type problems.
2. **Applied Steps is the script.** Every step replays on refresh.
3. **Append** = more rows. **Merge** = more columns.
4. **Unpivot** wide data into tall data.

### What's different in Power BI

**1. Close & Apply, not Close & Load.** Power BI has no worksheet to load into — everything goes into the model. One button, fewer decisions.

**2. Enable load.** Right-click a query → untick **Enable load** for staging queries you only merge from. This is the equivalent of "Only Create Connection" in Excel, and it keeps your model clean.

**3. Column quality tools.** View tab → tick **Column quality**, **Column distribution** and **Column profile**. You get bars above each column showing the percentage Valid / Error / Empty, and the count of distinct and unique values.

> **Turn these on and leave them on.** They tell you instantly which columns have errors, which have blanks, and whether a supposedly-unique key actually is unique. It is the fastest data-quality check available anywhere, and it takes one click.
>
> By default profiling looks at the first 1,000 rows only. The status bar at the bottom lets you switch it to the entire dataset.

### Cleaning techniques, in the order you usually need them

| Problem | Fix |
|---|---|
| Wrong types | Set the type on every column, first |
| Junk rows at the top | Home → Remove Rows → Remove Top Rows |
| Headers in row 1 of the data | Home → **Use First Row as Headers** |
| Blank rows | Home → Remove Rows → Remove Blank Rows |
| Duplicates | Home → Remove Rows → Remove Duplicates |
| Leading/trailing spaces | Transform → Format → **Trim** |
| Non-printing characters | Transform → Format → **Clean** |
| Inconsistent case | Transform → Format → lowercase / Capitalize Each Word |
| Unwanted characters (`#`, `$`) | Right-click → Replace Values |
| Two things in one column | Home → **Split Column** → By Delimiter |
| Columns you don't need | Right-click → Remove |
| Blanks that should be values | Transform → **Fill Down** |

**`Trim` and `Clean` deserve special mention.** Invisible whitespace is the commonest reason a join silently fails to match — the values look identical on screen and are not. Trim your key columns before every merge, as a reflex.

### Splitting columns

`"Laptop Pro | ELEC"` holds two facts. Home → Split Column → By Delimiter → Custom → `|` gives you a product name and a category code in separate columns. Trim afterwards.

**One column, one fact.** That rule from Day 1 Module 1 is what Split Column enforces.

### Adding columns

**Add Column** tab:

- **Custom Column** — write an M expression
- **Conditional Column** — an if/then builder with no code, the equivalent of a nested `IF`
- **Column from Examples** — type a couple of desired results and Power Query infers the transformation. Genuinely useful for fiddly text work
- **Date/Time →** extract Year, Month, Quarter, Day Name from a date

> **Extracting date parts is a habit worth forming**, though Module 6 shows a better way still: a dedicated date table.

### Query dependencies

View → **Query Dependencies** draws a diagram of which queries feed which. On a model with staging queries and merges it quickly becomes the only way to see what is going on.

---

## See it — worked example

Open **`Data Cleaning (Power Query).pbix`** and inspect the queries in the Power Query Editor (Home → Transform Data). Then look at the source files, which are each built to teach one thing.

### `orders_jan.csv` + `orders_feb.csv` — **append**

```
Order_ID,Product,Sales
1,Laptop,1200
2,Mouse,300
3,Keyboard,500
```

Identical columns; different rows. Note that both files contain an `Order_ID` 3 — a nice illustration that appending does **not** deduplicate. Stack them, then decide whether those are genuinely different orders or a data problem. Power Query will not decide for you.

### `sales_flattable.csv` — **the cleaning gauntlet**

```
OrderID,Product_Info,First_Name,Last_Name,Email,Customer_ID,Amount,Price,Order_Date,Technical_Log_ID
1001,"Laptop Pro | ELEC",#ANna,ScHmidt,ANNA@email.com,C-1001-DE,120.567,1200.499,D2024-01-05,SYS-88721-XZ
1003,"Mechanical Keyboard | ACC",   Luca,Rossi,LUCA@email.com,C-1003-IT," ",150.899,D2024-01-07,SYS-88723-XZ
```

Count the problems:

| Issue | Example | Fix |
|---|---|---|
| Two facts in one column | `Laptop Pro \| ELEC` | Split by `\|`, then Trim |
| Stray `#` prefix | `#ANna` | Replace Values |
| Chaotic casing | `ANna`, `ScHmidt` | Format → Capitalize Each Word |
| Leading spaces | `   Luca` | Trim |
| Whitespace posing as a value | `" "` in Amount | Replace with null, then set type |
| Dates with a `D` prefix | `D2024-01-05` | Replace `D` with nothing, set type Date |
| **Invalid dates** | `D2024-99-13` | There is no month 99 — decide: remove the row, or null it |
| Blank rows | | Remove Blank Rows |
| A column nobody needs | `Technical_Log_ID` | Remove |

**Turn on Column quality before you start** and watch the red error bars shrink as you work. It is the most satisfying demonstration of the feature.

The invalid date is the interesting one. There is no correct automatic answer — you must make a judgement and *record* it as a step. That is data cleaning: not making errors disappear, but handling them deliberately and repeatably.

### `sales_monthly.csv` — **unpivot**

```
Customer,Jan Sales,Jan Cost,Feb Sales,Feb Cost,Mar Sales,Mar Cost
Ali,100,60,120,70,140,90
Sara,150,90,180,100,200,120
```

Perfectly readable to a human, and useless to a chart. You cannot plot sales over time, because "time" is spread across column *headings* rather than sitting in a column.

Worse, each month has **two** measures, so a single unpivot gives you an ugly `Jan Sales` / `Jan Cost` attribute column. The full fix:

1. Select the six month columns → Transform → **Unpivot Columns**
2. Split the new `Attribute` column by the space → `Month` and `Measure`
3. **Pivot** the `Measure` column (values = Value) to get clean `Sales` and `Cost` columns

Result: `Customer | Month | Sales | Cost` — four columns, tall, and ready for anything.

That unpivot-split-pivot sequence is a genuinely common real-world pattern. It is worth doing once slowly.

---

## Do it — practice

**Exercise 1 — Append (~10 min)**
Load `orders_jan.csv` and `orders_feb.csv`. Home → Append Queries. Investigate the duplicate `Order_ID` 3 and decide what to do about it.

**Exercise 2 — The cleaning gauntlet (~25 min)**
Load `sales_flattable.csv` with **Transform Data**. Turn on Column quality/distribution/profile first. Then work through the table above until every column is clean and correctly typed. Aim for: no errors, no stray characters, consistent casing, proper dates, and `Technical_Log_ID` gone.

Then look at your Applied Steps list. That is a reusable cleaning script.

**Exercise 3 — Unpivot (~15 min)**
Load `sales_monthly.csv` and perform the full unpivot → split → pivot sequence to reach `Customer | Month | Sales | Cost`. Add a custom column for Profit (`Sales - Cost`).

**Stretch:** merge your cleaned `sales_flattable` output onto something using `Customer_ID`, and try a **Left Anti Join** to check for keys with no match.

### If you get stuck

| Problem | Why | Fix |
|---|---|---|
| `Error` cells after typing | Values that can't convert | Click one to read the message; fix or remove first |
| Dates won't convert | Prefix or invalid values | Strip the prefix; handle impossible dates explicitly |
| Split produced too many columns | Delimiter appears more than expected | Split by leftmost delimiter only |
| Trim didn't fix a match | Non-printing characters | Apply **Clean** as well as Trim |
| Unpivot grabbed the wrong columns | Wrong selection | Select the keeper columns → **Unpivot Other Columns** |
| Refresh is slow | Steps in an inefficient order | Remove unneeded columns/rows **early** |

---

## Check your understanding

1. What is different about Power Query in Power BI compared with Excel?
2. What do the Column quality and Column distribution panels tell you, and why turn them on first?
3. Why does `Trim` matter so much before a merge?
4. `sales_monthly.csv` can't be charted over time. Why not, and what fixes it?
5. You find `D2024-99-13`. What are your options, and why is there no automatic right answer?
6. Appending two files produced a duplicate ID. Did Power Query do something wrong?
7. When would you untick "Enable load"?

---

**Previous:** [Module 4](../04_connection_types/LESSON.md) · **Next:** [Module 6 — Data Modelling & Introduction to DAX](../06_data_modeling/LESSON.md) · **Day 2 index:** [README](../README.md)


---

*© 2026 Global Academy. Prepared for the ZIMASCO (Kwekwe) workshop. Facilitated by Tapiwa Zireva. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
