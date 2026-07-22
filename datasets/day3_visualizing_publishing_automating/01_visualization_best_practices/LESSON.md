---
title: "Day 3 · Module 1 — Power BI Visualization Best Practices"
---

# Day 3 · Module 1 — Power BI Visualization Best Practices

**Morning · ~75 minutes**
*Topics: choosing the right chart, custom visuals, conditional formatting, interactive dashboards with slicers, filters and bookmarks*

> **Work in the model you built on Day 2** ([Data Modeling.pbix](../../../powerbi_files/day2_advanced_excel_and_power_bi/Data%20Modeling.pbix) or your homework file). This module is about presentation, so it needs something real to present.

---

## Learn it

### The one question to ask first

**What is the reader supposed to do with this?**

A dashboard is not a place to display data. It is an instrument for making a decision. If a visual doesn't help someone decide something, it is decoration — and decoration costs attention that the important numbers need.

Before adding any visual, finish this sentence: *"I'm adding this so the reader can ______."*

### Choosing the right chart

The choice is determined by the *question*, not by taste.

| Question | Chart |
|---|---|
| How do these categories compare? | **Bar / Column** |
| How has this changed over time? | **Line** |
| What is the single headline number? | **Card** |
| How does it compare to target? | **KPI**, or **Gauge** |
| What are the details? | **Table / Matrix** |
| Where is this happening? | **Map** |
| How do two measures relate? | **Scatter** |
| What makes up this whole? | **Donut / Pie** — sparingly |
| What's the biggest contributor to a change? | **Waterfall** |
| Where do people drop out of a process? | **Funnel** |

**Bar charts are the default for a reason.** People compare lengths from a common baseline more accurately than any other visual encoding. When in doubt, use bars.

**Horizontal bars beat vertical columns when category names are long** — the labels stay readable instead of turning sideways.

### The chart rules worth being strict about

**1. Start the value axis at zero on bar charts.** Truncating the axis exaggerates differences and misleads people. It is the most common way an honest analyst produces a dishonest chart. (Line charts showing change over a narrow range are the accepted exception.)

**2. Sort deliberately.** Sort bars by value, not alphabetically, unless the category has a natural order (months, quarters, sizes). A sorted bar chart answers "who's biggest?" instantly.

**3. Avoid pie charts with more than about four slices.** Humans judge angles poorly. If you catch yourself adding data labels so the pie can be read, you needed a bar chart.

**4. Never use 3-D.** It adds no information and distorts the very comparison the chart exists to make.

**5. Limit colours.** Colour should *mean* something. A chart where every bar is a different colour has used its most powerful tool to say nothing. Use one colour, and a second only to highlight the point you're making.

**6. Label directly where you can.** A line labelled at its end beats a legend the reader has to look back and forth to decode.

### Formatting that earns its place

The **Format pane** (the paint-roller icon) with a visual selected:

| Setting | Why |
|---|---|
| **Title** | Make it state the finding — "Laptops drive 47% of revenue" beats "Sales by Category" |
| **Data labels** | On for a handful of bars; off when they'd crowd |
| **Y-axis** | Often removable once you have data labels — less ink |
| **Gridlines** | Lighten or remove |
| **Data colours** | One colour, plus a highlight |

### Conditional formatting in visuals

Format pane → **Data colours** → the *fx* button. Colour by rules, by a colour scale, or by a field value.

Genuinely useful applications:

- Colour a bar **red when below target**, green when above
- A **matrix** with background colour scales — a heatmap of performance
- **Data bars inside a table** — the same idea as Day 1 Module 7

Use rules tied to a measure rather than fixed thresholds so the formatting stays honest as the data changes.

### Custom visuals

The three-dots on the Visualizations pane → **Get more visuals** opens Microsoft's AppSource marketplace.

Worth knowing about:

| Visual | Use |
|---|---|
| **Smart Narrative** | Auto-generated text summary that updates with the data — built in |
| **Decomposition Tree** | Interactive drill into what's driving a number — built in, and excellent |
| **Key Influencers** | Finds which factors drive an outcome — built in |
| Bullet chart | Actual vs target, compactly |
| Word cloud | Rarely genuinely informative |

> **Two cautions on custom visuals.** They may not render in all export formats, and an organisation's admin settings may block uncertified ones. Before you build a report around one, check it works where the report will actually be read. The three built-in AI visuals above are safe and are the ones most worth learning.

### Interactivity

**Slicers** — on-canvas filter buttons. Same as Excel's.
Format them as a list, dropdown (saves space), or slider for dates. **Sync slicers** across pages via View → Sync slicers.

**The Filters pane** — three levels of scope:

| Level | Affects |
|---|---|
| Visual | That one visual |
| Page | Every visual on the page |
| All pages | The whole report |

**Edit interactions** — select a visual, then Format → **Edit interactions**, and set what each *other* visual does when this one is clicked: filter, highlight, or nothing. Use it to stop a slicer or a detail table from filtering something it shouldn't.

**Drill-through** — right-click a data point → Drill through → a detail page built for exactly that item. Set it up by putting the relevant field into the target page's Drill-through well.

**Bookmarks** — View → Bookmarks → Add captures the current state: filters, selections, which visuals are visible.

Two things they're brilliant for:

1. **A guided story** — a sequence of bookmarks stepped through with the Selection pane, so a report walks the reader through an argument
2. **Toggling views** — a button that swaps a chart for a table in the same space, letting one page serve two audiences

Combine a bookmark with a **Button** (Insert → Buttons → set Action to Bookmark) for a properly app-like report.

### Layout

- **Put the most important thing top-left.** Readers start there.
- **A row of KPI cards across the top**, detail beneath, is a reliable and readable pattern.
- **Align and size consistently.** View → Snap to grid. Ragged visuals read as careless work.
- **Leave white space.** A cramped dashboard is a hard dashboard.
- **Aim for five to seven visuals per page.** More than that and nobody knows where to look — make a second page.

### Accessibility

Not optional, and quick:

- Don't rely on colour alone — around 8% of men have some colour vision deficiency. Add labels, icons or position.
- Check contrast between text and background.
- Set **Alt text** on each visual (Format → General → Alt text) for screen readers.
- Set a sensible **tab order** (Selection pane).

---

## See it — worked example

Open your Day 2 model and try these comparisons directly. Seeing the difference is worth more than reading about it.

**Experiment 1 — sorting.** Build a bar chart of `Total Sales` by product. Sort alphabetically, then by value. Ask how long it takes to find the top product each way.

**Experiment 2 — the axis.** Make a bar chart, then Format → Y axis → set Start to something just below your lowest value. The differences look dramatic. Now set it back to zero. Same data, a completely different impression. **This is why the rule exists.**

**Experiment 3 — colour.** Give every bar its own colour. Then set all bars to one grey and colour only the top one. The second chart makes an argument; the first just displays data.

**Experiment 4 — pie vs bar.** Put the same six-category breakdown into a pie and a bar chart. Rank the categories from each.

**Experiment 5 — the Decomposition Tree.** Add one, put `Total Sales` in Analyze and Category, Country and Product in Explain by. Click through the levels. It answers "why is this number what it is?" better than any static chart.

---

## Do it — practice

Build a **one-page dashboard** on your Day 2 model. Requirements:

1. A row of **three or four KPI cards** across the top
2. A **sorted bar chart** — one colour, with the leader highlighted
3. A **line chart** over time, using your date table
4. A **matrix** with conditional formatting (a colour scale or data bars)
5. **Two slicers**, formatted so they don't dominate the page
6. Every visual titled with a **statement**, not a label
7. A **bookmark** capturing a filtered "focus" state, plus a button to reach it
8. **Edit interactions** so the matrix doesn't filter the cards
9. **Alt text** on every visual
10. Everything aligned to the grid

**Then apply the test:** show it to someone for ten seconds, take it away, and ask what they remember. If it isn't the thing you most wanted them to know, the layout needs work — not more visuals.

### If you get stuck

| Problem | Why | Fix |
|---|---|---|
| Chart shows only one bar | Field is a measure, not a category | Put a category field on the axis |
| Slicer filters something it shouldn't | Default interaction | Format → Edit interactions |
| Colours look arbitrary | Per-category default palette | Set one Data colour; use *fx* rules for the highlight |
| Bookmark restores the wrong thing | It captured filters you didn't intend | Edit the bookmark's captured properties |
| Custom visual won't load | Blocked by tenant settings | Use a built-in equivalent |
| Map won't plot | Field's Data Category unset | Set it to City / Country |
| Page feels crowded | Too many visuals | Split across pages; use drill-through for detail |

---

## Check your understanding

1. What question should you answer before adding any visual?
2. Why are bar charts the safe default?
3. Why must bar chart axes start at zero, and what's the accepted exception?
4. When is a pie chart defensible?
5. What are the three scopes in the Filters pane?
6. Give two distinct uses for bookmarks.
7. What does "Edit interactions" control?
8. Name two accessibility steps that take under a minute each.

---

**Next:** [Module 2 — Storytelling with Data](../02_storytelling_with_data/LESSON.md) · **Day 3 index:** [README](../README.md)


---

*© 2026 Global Academy. Prepared for the ZIMASCO (Kwekwe) workshop. Facilitated by Tapiwa Zireva. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
