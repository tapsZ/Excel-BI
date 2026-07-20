---
title: "Day 3 · Module 2 — Storytelling with Data: Turning Dashboards into Decisions"
---

# Day 3 · Module 2 — Storytelling with Data: Turning Dashboards into Decisions

**Morning · ~45 minutes**
*Topics: structuring an analytical narrative, writing findings rather than labels, presenting to an executive audience*

> This is the shortest module of the course and, in career terms, possibly the most valuable. Plenty of people can build a dashboard. Far fewer can make one change a decision.

---

## Learn it

### The gap this module closes

You have spent two days learning to produce correct numbers. Correct numbers do not, by themselves, change anything.

A dashboard that reports "Revenue: $154,387" is *accurate* and *useless*. It leaves every remaining question — is that good? compared to what? what should we do? — for the reader to work out. Most won't. They'll glance, feel vaguely informed, and move on.

**Your job is not to present data. It is to answer a question that someone was going to have to make a decision about anyway.**

### Start with the decision, not the data

Before opening Power BI:

1. **Who reads this?** A CFO and a warehouse supervisor need different things from the same data.
2. **What decision are they making?** Where to spend? What to stop? Who to call?
3. **What would change their mind?** That is your headline number.
4. **What action follows?** If none, ask whether the report should exist.

A dashboard built from those four answers is nearly always simpler than one built from "what data do we have?" — and much more used.

### The three-act structure

The oldest structure there is, and it works because it matches how people take in an argument:

| Act | Content | On a dashboard |
|---|---|---|
| **1. Context** | Where we stand | KPI cards along the top — the headline numbers, against target |
| **2. Conflict** | What's changed, or gone wrong | The trend, the variance, the outlier — the middle of the page |
| **3. Resolution** | What to do about it | The breakdown that points at the cause, plus a written recommendation |

**Most dashboards are all Act 1.** Rows and rows of numbers with no argument. Adding the second and third acts is what turns a report into something people act on.

### Titles that say something

The single highest-return change you can make to any dashboard.

| Instead of | Write |
|---|---|
| "Sales by Category" | "Laptops drive 47% of revenue" |
| "Monthly Trend" | "Revenue has fallen three months running" |
| "Sales by Region" | "East Africa is 12% behind target; every other region is ahead" |

A label makes the reader do the analysis. A finding hands it to them. You are the one who has looked at the data — say what you found.

**Watch the honesty line.** If a title states a finding, the finding must survive filtering. A title reading "East Africa is behind target" becomes false the moment someone slices to a different year. Either write titles that hold generally, or use a **dynamic title** driven by a DAX measure so the text updates with the data:

```
Headline Title =
VAR Top = TOPN(1, VALUES(dim_product[category]), [Total Sales])
RETURN "Top category: " & CONCATENATEX(Top, dim_product[category])
```

Format pane → Title → *fx* → Field value → your measure.

### Give numbers a comparison

A number alone means nothing. `$154,387` is good, bad or catastrophic depending entirely on context you haven't supplied.

Always pair a headline number with at least one of:

- **vs target** — the most useful of all
- **vs last period** — direction of travel
- **vs the same period last year** — strips out seasonality
- **vs peer** — other regions, stores, products
- **as a share of total** — is this big or small in context?

Power BI's **KPI visual** and card **callout value + reference label** are built for this. Use them rather than a bare card.

### Reduce, then reduce again

The instinct is to include everything, because leaving things out feels like hiding them. But attention is finite: every extra visual reduces the odds that the important one is read.

- **One page, one message.** More messages, more pages.
- Cut every visual that doesn't serve the decision.
- Detail belongs behind **drill-through**, not on the front page.
- If a visual needs a paragraph of explanation, it's the wrong visual.

### Colour as an argument

Grey everything, then colour the one thing you want looked at. It feels almost too simple and it works better than any other single technique.

Keep colour meaning **consistent across the whole report**: if red means "behind target" on page 1, it cannot mean "East Africa" on page 2. Inconsistent colour quietly destroys trust in a report.

### Writing the recommendation

Put actual words on the dashboard — a text box is fine. Structure:

> **What:** East Africa finished Q2 12% behind target.
> **Why:** Laptop volume fell 22% after the March price change; other categories held.
> **So what:** At the current run-rate, the annual target is missed by roughly $180k.
> **Now what:** Review the laptop pricing decision before Q3 planning closes on the 15th.

Four sentences. That is what turns a dashboard into a decision — and almost nobody writes them.

### Presenting it

You will present your capstone this afternoon. What works:

- **Lead with the finding**, not the method. "East Africa is behind target, and here's why" — not "First I cleaned the data…"
- **Say the number, then what it means.** Never just the number.
- **Don't narrate the build.** Nobody needs the story of your merges. Your effort shows in the result.
- **Demonstrate one interaction**, not seven. Click one slicer, show the report respond, move on.
- **Know your weak point.** If the data has a caveat — a missing quarter, an assumption about nulls — say it yourself, early and briefly. Being asked about a flaw you concealed is much worse than mentioning it.
- **End on the recommendation.** That is the whole reason you're standing there.

### The tests

Two quick checks before you show anyone anything:

**The five-second test.** Show it for five seconds, take it away, ask what they remember. Whatever they say is what your dashboard is *actually* communicating. If that isn't your main message, fix the layout — don't add more.

**The "so what?" test.** For every visual, ask "so what?" If you can't answer in one sentence tied to a decision, cut it.

---

## See it — worked example

Take a dashboard you built in Module 1 — or the Day 2 model — and rework it against this checklist. Comparing your before and after is the exercise.

**Before:** cards showing Total Sales, Order Count, Average Order Value. A bar chart titled "Sales by Category". A line titled "Monthly Trend". A matrix of everything.

**After:**

- Cards now show **actual vs target**, with variance and an up/down indicator
- Bar chart is sorted, one colour, leader highlighted, titled with the finding
- Line chart title names the trend and its direction; the notable month is annotated
- Matrix moved to a **drill-through page** — off the front
- A text box carries the four-sentence What / Why / So what / Now what
- Everything not serving the message: gone

Same data. Same model. A completely different level of usefulness.

---

## Do it — practice

Rework one dashboard, then rehearse it.

1. Write down, in one sentence each: **who** reads this, **what decision** they make, **what action** follows
2. Identify your **single most important message**
3. Restructure into the three acts — context up top, conflict in the middle, resolution at the bottom
4. Rewrite **every title as a finding**; make at least one dynamic with a DAX measure
5. Give every headline number a **comparison**
6. Grey everything, then colour only what matters
7. **Cut two visuals.** There are always two.
8. Write the four-sentence recommendation into a text box
9. Run the **five-second test** on a colleague
10. **Rehearse the two-minute version out loud.** Time it.

That last step is the one people skip, and it's the one that shows.

---

## Check your understanding

1. What four questions should you answer before opening Power BI?
2. What are the three acts, and which one do most dashboards stop at?
3. Rewrite "Sales by Region" as a finding.
4. When does a finding-style title become dishonest, and what's the fix?
5. Why is a headline number without a comparison nearly meaningless?
6. What is the five-second test measuring?
7. Why mention a weakness in your data before anyone asks?

---

**Previous:** [Module 1](../01_visualization_best_practices/LESSON.md) · **Next:** [Module 3 — Publishing, Sharing & Automating](../03_publishing_and_sharing/LESSON.md) · **Day 3 index:** [README](../README.md)


---

*© 2026 Tapiwa Zireva. Prepared for the ZIMASCO (Kwekwe) workshop. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
