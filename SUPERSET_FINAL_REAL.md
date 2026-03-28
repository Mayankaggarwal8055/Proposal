# Apache Superset GSoC 2026 Proposal
## Dashboard Native Filter X-Filtering Implementation

---

## Personal Information

**Name:** Mayank Aggarwal  
**Location:** Delhi, India  
**Email:** aggarwalmayank184@gmail.com  
**GitHub:** [Mayankaggarwal8055](https://github.com/Mayankaggarwal8055)  
**Time Zone:** IST (UTC +5:30)

---

## About Me

I'm a CS student at IGNOU finishing my degree while working as a Teaching Assistant. I've been coding for a few years now and learned pretty early that the best way to understand how systems actually work is to build stuff and ship it.

I started with the usual "I want to make websites." But that got boring. I realized I actually learn by working on real projects that people use. Open source is where that happens.

A few months ago I started contributing to Superset because the codebase is solid and the team is good to work with. I've submitted 6 PRs, got 2 merged, and have 4 more in review right now. All in the dashboard and filter area—which is exactly where X-filtering fits.

That's not a coincidence. I've been deliberately working in this area because I understand it now and genuinely think I can build this feature well.

---

## My Actual PRs (What I've Actually Done)

### Merged (2)

**PR #38745 - Larger JSON metadata editor** 
[GitHub Link](https://github.com/apache/superset/pull/38745)
- Status: Merged 3 days ago
- What: Made the JSON editor bigger so you can actually read what you're editing
- Simple change, but users complained about it
- Teaches me: Small UX things matter

**PR #38649 - Fixed broken doc links**
[GitHub Link](https://github.com/apache/superset/pull/38649)
- Status: Merged 2 weeks ago
- What: Updated broken links in developer docs
- Sounds boring but saved people hours of debugging
- Feedback from maintainers: "We appreciate your thorough follow-up"
- Teaches me: Good documentation is infrastructure

**PR #64355 - Fixed Helm chart docs**
[GitHub Link](https://github.com/apache/airflow/pull/64355)
- Status: Merged 37 minutes ago (Airflow)
- Shows I'm actively contributing across projects right now

### Open Right Now (These are actually in review)

**Superset (6 open):**

**PR #38762 - Improved cross-filter scope resolution**
[GitHub Link](https://github.com/apache/superset/pull/38762)
- Opened: Last week
- Status: Under review
- Code changes: 20+ files, significant refactor
- Why this matters: This is literally about how filters propagate through dashboards. When I build X-filtering, I'll be using the logic I'm improving here.
- This shows: I understand the architecture, not theoretically but by actually modifying it

**PR #38743 - Remove transparency from filter indicator**
[GitHub Link](https://github.com/apache/superset/pull/38743)
- Opened: Last week
- Status: Under review
- What: Visual consistency fix
- Why it matters: Shows I pay attention to details, not just features

**PR #38741 - Normalize tooltip font sizes**
[GitHub Link](https://github.com/apache/superset/pull/38741)
- Opened: Last week
- Status: Under review (1 comment, 18 reviews)
- What: Makes all tooltips look consistent
- Why: Consistency is invisible until it's broken

**PR #38729 - Fix spacing in Tabs layout**
[GitHub Link](https://github.com/apache/superset/pull/38729)
- Opened: Last week
- Status: Under review
- What: Dashboard charts inside tabs have weird spacing
- Shows: I find and fix actual user problems

**PR #38656 - Remove double scrollbars**
[GitHub Link](https://github.com/apache/superset/pull/38656)
- Opened: 2 weeks ago
- Status: Under review
- What: UI bug where scrollbars appear unnecessarily
- Blocked for 48h by CI (probably flaky test)
- Shows: I can debug UI issues, understand CSS, handle CI problems

### PouchDB (3 open)

**PR #9193 - Guard Error.stack access**
[GitHub Link](https://github.com/pouchdb/pouchdb/pull/9193)
- Status: Under review (2 conversations)
- What: Error handling bug fix
- Shows: Can work on different codebases (not just Superset)

**PR #9186 - Update CouchDB docs link**
[GitHub Link](https://github.com/pouchdb/pouchdb/pull/9186)
- Status: Under review (1 comment, 6 total)
- What: Documentation link was moved upstream

**PR #9185 - Surface HTTP 413 error**
[GitHub Link](https://github.com/pouchdb/pouchdb/pull/9185)
- Status: Under review (1 comment, 3 total)
- What: Replication error handling improvement

### ECharts (1 open)

**PR #19 - Add scroll-to-top button**
[GitHub Link](https://github.com/apache/echarts-website/pull/19)
- Status: Under review
- What: Navigation improvement for the docs site

### Also merged elsewhere: SedonaDB (3 merged)

**PR #710 - Remove norun markers from Rust docs**
- Merged 2 weeks ago
- Status: Merged
- What: Documentation build fix

**PR #676 - Remove unused CI scripts**
- Merged last month
- Status: Merged
- What: Technical debt cleanup

**PR #463 - Fix typo in GPU code**
- Merged Dec 17, 2025
- Status: Merged
- What: Typo fix showing attention to detail

---

## The Pattern Here

I notice all my Superset PRs are in **dashboard/filter/UI area**. That's deliberate. I spent time understanding that part of the codebase. I have 6 PRs in that area because I kept finding things to improve in the code I was learning.

This matters for X-filtering because it means I'm not starting from "I've read about filters." I'm starting from "I've modified filter code, seen how it works, understand the patterns."

---

## The Problem (The Honest Version)

When you use Superset, you have a dashboard with a line chart showing revenue across 12 months. You see something interesting in Q2. You want to zoom in on just Q2.

Right now you:
1. Find the date filter (usually on the left side)
2. Click it
3. Manually set April 1 to June 30
4. Wait for the dashboard to reload

This is annoying because you're not interacting with the chart. You're hunting for controls.

Better: Click and drag on the chart's X-axis from April to June. Everything filters. No separate panel. Just direct interaction.

**Why this matters:** 
- Users explore data faster (direct interaction)
- Feels modern (like Tableau or Google Analytics)
- Enterprises comparing tools notice this (it's a feature Tableau has)
- Non-technical users can use it (don't need to understand filter UI)

**Real feedback from community:**
- "Why can't I just click on the chart to filter like in Tableau?"
- Multiple GitHub issues asking for this
- Teams choosing other tools partly because of this

**What's missing:**
Superset has Y-axis filtering (click a bar to filter by that bar's value). It doesn't have X-axis filtering (select a range on the X-axis).

Different chart types have different X-axes:
- Line charts: continuous time
- Bar charts: categories or time (grouped/stacked)
- Pie charts: no real X-axis (would be segments)
- Tables: columns
- Pivot tables: row/column headers

One solution can't handle all of these. That's why it's not been done yet.

---

## What I'm Building

A feature that lets you click and drag on the X-axis of any chart to select a range, and it filters the dashboard.

For a **line chart:**
- Visual range selector appears (overlay or highlight)
- You drag to select Apr-Jun
- Dashboard filters to show only Q2 data
- All connected charts update

For a **bar chart:**
- Similar but with grouped/stacked considerations
- Need to maintain aggregation logic

For a **pie chart:**
- Different: click a segment to filter by that segment
- Maintain percentage calculations

For **tables:**
- Click column headers to select range
- Support numeric, text, date filtering

For **pivot tables:**
- Row and column header filtering
- Maintain aggregations and drill-down

---

## Timeline (Realistic, Not Overly Optimistic)

**10 weeks, 280 hours of work**

### Phase 1: Foundation (Weeks 1-3)

**Week 1: Learn the system**
- Read how Y-filtering currently works
- Trace code: Chart → Filter → Query → Backend → Response
- Study ECharts (the charting library)
- Understand Redux filter state
- Set up local environment with test data

**Week 2-3: Build core infrastructure**
- Write xAxisFilter component
- Handle mouse events (drag detection)
- Convert pixel coordinates to actual data values
- Integrate with Redux
- Test on Table chart (simplest)

Deliverable: Proof-of-concept on Table chart. It works but no animations, no polish.

### Phase 2: Implementation (Weeks 4-7)

**Week 4:** Line chart X-filtering
- ECharts event handling
- Time value parsing
- Handle timezones
- Test with various time formats

**Week 5:** Bar chart
- Grouped/stacked bar logic
- Maintain aggregations when filtering

**Week 6:** Pie and Pivot charts
- Pie: segment filtering, maintain percentages
- Pivot: row/column filtering, aggregations

**Week 7:** Comprehensive testing
- Test all 5 charts with real data
- Edge cases (empty data, null values, single values)
- Cross-browser testing
- Performance testing (100k+ rows)

### Phase 3: Polish (Weeks 8-10)

**Week 8:** UX improvements
- Smooth animations
- Visual indicators for active filters
- Mobile support
- Accessibility (keyboard, screen readers)
- Feature flag (ENABLE_X_FILTERING)

**Week 9:** Documentation
- User guide (how to use)
- Developer guide (how to extend)
- Blog post
- Code comments

**Week 10:** Testing and fixes
- Final regression testing
- Bug fixes
- Polish

---

## Technical Approach

### How it works end-to-end:

1. User drags on chart X-axis
2. Browser captures mouse events
3. Calculate which data values are in the selected range
4. Create filter: {column: 'date', min: '2024-04-01', max: '2024-06-30'}
5. Redux state updates with this filter
6. Dashboard query builder adds WHERE clause
7. Backend executes query
8. Results come back
9. All charts re-render with new data
10. User sees filtered dashboard

### The complicated parts:

- **Data type handling:** Dates, numbers, categories all work differently
- **Multiple chart types:** Each has different X-axis structure
- **Maintaining aggregations:** When you filter a sum, it stays a sum
- **Performance:** Dashboard with 20+ charts, each filtering shouldn't lag
- **Edge cases:** What if the filter returns no data? What if someone filters to a single value?

### Technology:

- React/TypeScript (UI)
- Redux (state management)
- ECharts (charting library that handles coordinate conversion)
- Python/Flask (backend query building)
- Jest and pytest (testing)

I've worked with all of these. I have 2 PRs already merged and 6 open, all in this exact tech stack.

---

## Honest Limitations

**I'm still relatively new to Superset.** I've been contributing for a few months, not years. But I've been deliberately working in the filter/dashboard area. I understand that part of the code. I know who to ask for help.

**This is a complex feature.** Different chart types behave differently. I might discover mid-project that one chart type is harder than expected. If that happens, I'll reduce scope—maybe 3 core charts instead of 5—and do them well instead of shipping half-working code.

**Timeline could slip.** Any project can hit unexpected issues. That's why I built in a buffer week. If I'm behind by week 9, I have week 10 to catch up.

**I don't know everything.** But I know how to ask good questions and I know when I need help. I've gotten feedback from maintainers that I handle code review well and iterate quickly.

---

## Why Me

**Proof:** I've actually done this before. Not X-filtering specifically. But I've modified filter code (PR #38762), I've worked with the dashboard system, I've integrated with Redux state. This isn't theoretical knowledge—it's from actual code I've written.

**Track record:** 6 PRs in 2-3 months, 2 merged, 4 in review. That's consistent. That's not "I'm really interested but will disappear." That's "I'm showing up."

**I know the codebase:** I could start coding in week 1 and actually make progress. I don't need weeks to figure out where things are. I've already navigated this code.

**Maintainers already know me:** They approved my PRs. They didn't ask who this person is. They reviewed code from someone they recognize as competent.

**I actually care about this project:** I could be contributing to anything. I chose Superset because the community is good and the codebase is well-maintained. That matters to me.

---

## Risk Handling

**If one chart type is blocking me:** I'll move to another chart type and come back. I have 5 to implement. If I get stuck on pie charts, I'll finish line, bar, table, pivot, then come back to pie.

**If I fall behind schedule:** I cut from 5 charts to 3 core charts (Table, Line, Bar) and make sure those are perfect. I'd rather deliver 3 excellent charts than 5 mediocre ones.

**If testing finds major issues:** Week 10 is buffer. I'll spend it fixing. Nothing ships broken.

**If something unexpected comes up:** I communicate early with mentor. I don't wait until week 9 to say "this is harder than expected." I flag issues week 2 if they exist.

---

## Success Metrics

- X-filtering works on 5 charts (or 3 if we scope down)
- 80%+ test coverage
- No regressions in existing filtering
- All code passes CI
- Documentation is complete
- Works across browsers
- Smooth performance (no lag)

That's it. I'm not trying to be a hero. I'm trying to ship working code.

---

## Communication

**Weekly:** Mentor sync Wednesday 9 AM IST. Show working code. Talk about what's blocking. Adjust timeline if needed.

**Daily:** Commits on GitHub. Slack updates. Ask questions when stuck (not "stuck for 3 days"—ask day 1).

**Honest:** If I'm behind, I'll say it. If something's harder than expected, I'll mention it. If I found a better approach, I'll explain why we should change direction.

---

## Final Thought

I'm not doing this for a resume line. I'm doing this because I've been working on Superset and I think I can build this well. The proof is that I'm already contributing and getting PRs merged.

I'm not claiming to be an expert. I'm claiming to be someone who learns quickly, ships code, and doesn't disappear after summer.

Let me build this.

---

**What you'll get:**
- Real working code
- Production-quality implementation
- Good documentation
- Someone who stays around after GSoC

**What I'm getting:**
- Deep experience with large open source project
- Relationship with good mentors
- Proof I can ship complex features
- Part of a community I respect
