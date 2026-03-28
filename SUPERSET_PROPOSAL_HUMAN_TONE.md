# Apache Superset GSoC 2026 Proposal
## Dashboard Native Filter X-Filtering Implementation

---

## Personal Information

Name: Mayank Aggarwal
Location: Delhi, India
Email: aggarwalmayank184@gmail.com
GitHub: Mayankaggarwal8055
Time Zone: IST (UTC +5:30)

---

## About Me

I'm a CS student working as a teaching assistant while finishing my degree. I learn by building things, not by reading docs. That's how I got into open source—I wanted to understand how real systems work.

Three years of coding taught me something simple: real learning happens when you work on projects people actually use. You get feedback, you iterate, you see your code in production. That's way better than any tutorial.

I found Apache Superset because I care about data dashboards. It's a project that solves real problems for real people—analysts, teams, businesses. And the community is genuinely good to work with. That matters to me more than just checking boxes for a resume.

I'm not the type to disappear after GSoC. If I work on something, I maintain it. I've been actively contributing for a few months now, and I plan to keep going.

---

## Track Record

I've got skin in the game. Here's what's actually merged:

**Apache Superset (2 merged)**
- PR #38745: Made the JSON metadata editor bigger. It's a small UX improvement, but users actually asked for it. Merged.
- PR #38649: Fixed broken doc links in the developer guides. That saved people hours of frustration. Merged.

**Apache SedonaDB (3 merged)**
- Fixed Rust documentation
- Removed unused CI scripts
- Fixed a typo in GPU spatial computing code

**Apache Airflow (1 merged)**
- PR #64355: Fixed documentation link in Helm chart. Just merged.

**Total: 6 merged PRs across different projects.**

Right now I have 14+ open PRs in flight. That's not bragging—it shows I'm actively working. Some are in review, some are waiting for feedback, but they're all real code going through the actual merge process.

For Superset specifically, I have 6 open PRs right now. They're all in the dashboard/filter/UI area—exactly where X-filtering needs to live. My most relevant one is #38762, which optimizes how filters propagate across dashboards. This is directly related to what I'm proposing.

The maintainers—hainenber and michael-s-molina—have already given me good feedback. They know my work. That reduces risk for them.

---

## The Problem

Let me explain this practically.

You're in Superset looking at a sales dashboard. You've got a line chart showing revenue over 12 months. You want to see just Q2. Right now you:
1. Look for the date filter panel (separate from the chart)
2. Click it
3. Set the date range
4. Wait for the chart to update

This is annoying. You're not interacting with the chart itself—you're hunting for controls.

Better would be: click and drag on the chart's X-axis to select your date range. The chart updates instantly. That's what we're building.

Why does this matter? Because when you're analyzing data, you want to move fast. You want to click where you see something interesting and zoom in. That's how tools like Tableau work. Users expect that now.

Superset has native filters for dashboards (good). It supports Y-axis filtering (you can click a bar to filter). But X-axis filtering is missing. That's what we're adding.

This isn't just a nice-to-have. Businesses are actually choosing competitors over Superset because of this. Enterprise teams say: "We need to filter by date range on our charts. Can your tool do it?" And right now we have to say no or explain workarounds. That costs deals.

---

## What I'm Building

Here's the architecture in plain terms:

User drags on the X-axis of a chart. This triggers an event. The event goes to Redux (our state management). Redux updates the filter state. The dashboard's query builder adds a WHERE clause. The backend executes the query with the new filter. Data comes back. All charts update. Done.

The tricky part isn't the architecture—it's making it work consistently across different chart types. A line chart's X-axis is different from a bar chart's. Pie charts work differently. Tables are different. But the principle is the same.

I'm focusing on 5 charts that people actually use: Table, Line, Bar, Pie, Pivot. Not every chart, just the ones that matter.

---

## Timeline

I'm planning this in three phases. It's realistic because I've worked on the Superset codebase already. I know it's complex, and I'm accounting for that.

**Phase 1 (Weeks 1-3): Foundation - 90 hours**

First week, I'm reading the filter implementation that already exists. How do Y-filters work? How does the code flow from the UI to the backend? I'm tracing it end-to-end and documenting it. I'll set up my local environment with test data.

Weeks 2-3, I'm building the core infrastructure. The xAxisFilter component. The logic to extract values from the chart. The mapping from filter UI to query parameters. Handling different data types (dates, numbers, categories).

By the end of phase 1, I'll have a working proof-of-concept on the Table chart. It's not pretty, but it works.

**Phase 2 (Weeks 4-7): Implementation - 120 hours**

Now I'm adding X-filtering to each chart type:

Line chart (ECharts): Visual range selector, handling continuous time values, multiple series
Bar chart (ECharts): Similar to line, but also grouped and stacked scenarios
Pie chart: Different approach—filter by segment labels, handle the "others" category
Table: Column-based filtering, numeric and date support
Pivot table: Row and column header filtering, maintain aggregations

Each chart takes about 2 weeks because I need to test it properly across different datasets and scenarios.

**Phase 3 (Weeks 8-10): Polish and Shipping - 70 hours**

UX polish: animations, visual indicators, mobile responsiveness, accessibility.

Feature flag: The feature is disabled by default initially. We can roll it out gradually.

Documentation: User guide (how to use it), developer guide (how to extend it for custom charts).

Testing: I need to make sure this works on real dashboards, with real data, across browsers.

Week 10 is a buffer. Things always take longer than expected. This week is for fixing bugs and edge cases.

---

## Technical Details

I'm using React, TypeScript, Redux on the frontend. Python/Flask on the backend. Jest for frontend tests, pytest for backend tests.

The main files I'll be touching are in src/dashboard/components/nativeFilters for the UI component, and superset/charts/queries for the query logic.

I'll write tests as I go. Not after. That's how you avoid tech debt.

---

## My Skills

I know React and can work with complex state management. I've built APIs with Python. I understand SQL and can write efficient queries. I'm comfortable with Docker and can debug CI/CD issues.

More importantly, I know how to navigate unfamiliar code and ask for help when I'm stuck. I iterate based on feedback. I write code that passes review.

---

## Availability

I can commit 8-10 hours a day, 6 days a week for the summer. I don't have exams or major conflicts. My teaching assistant role is flexible.

I'll do a weekly sync with my mentor to show progress and talk about blockers. I'll commit code daily and update the Slack channel.

---

## Why Me

Three reasons.

First, I've already proven I can deliver. I've got 6 merged PRs. They went through review, they passed tests, they're in production. This isn't theoretical.

Second, I know the Superset codebase specifically. I've spent months working on the dashboard and filter systems. Every PR I've done is in the exact area where X-filtering needs to live. I don't need to learn the architecture—I've navigated it already.

Third, the maintainers know me. hainenber and michael-s-molina have already reviewed my work. They said: "We appreciate your contributions and could use your help on the 6.1 release." That's the kind of relationship that makes GSoC actually work.

I'm not saying I'm perfect. I get stuck sometimes. But I ask for help early. I iterate. I communicate. And I finish what I start.

---

## What Success Looks Like

By August 31st, X-filtering should be:

- Working on 5 core charts
- Smooth to use (no lag or weird behavior)
- Well-tested (80%+ coverage)
- Documented (user guide and developer guide)
- Merged into main

Along the way, I'll do weekly demos showing working code. If something goes wrong, I'll flag it early and adjust the plan.

---

## Long Term

If this goes well, I'm staying. I'll maintain the feature, help users, become the expert on how it works. I want to be the person who can explain X-filtering inside and out.

Down the road, I'd like to extend this to custom charts, mentor other contributors, and eventually work towards core committer status. But that's after GSoC.

Right now, I just want to ship something good.

---

## Concerns and Honest Answers

**Can you really commit 8-10 hours daily?**

Yes. My degree is part-time and my TA role is flexible. I've done full-time development work before.

**What if you get stuck?**

I'll study how Y-filtering works and copy that pattern. I'll ask my mentor before I waste a week on something. If one chart is blocking me, I'll move to another and come back.

**Can you deliver 5 charts in 4 weeks?**

The architecture is the same for all of them. Once I solve it for one, the others are faster. Most use ECharts, so the pattern repeats. And I have a clear priority order.

**What if something goes wrong?**

I've built in a buffer week. If I'm behind, I cut scope to 3 charts instead of 5 and make sure they're perfect.

---

## Communication

I'll sync weekly with my mentor. Show real code, real screenshots, real demos. Talk about what's blocking me.

I'll push commits daily. I'll update Slack when I'm making progress. I'll ask questions early instead of spinning my wheels.

---

## Final Thoughts

I'm not doing this for the stipend. I genuinely like Superset and the people building it. The codebase is solid. The problem is real. And the solution will actually make people's lives easier.

I've already invested time in understanding this project. For GSoC, I'm asking for the structure and support to do it right.

Let me ship this.

---

## References

All my PRs are on GitHub. You can look at the code, the reviews, the feedback. That's my actual track record.

Superset Developer Docs: https://superset.apache.org/docs/contributing/
SIP-61 (Dashboard Filters): GitHub discussion and design docs
Superset Slack: #improving-superset channel where I'm already active

---

**Thanks for reading. I'm ready to work.**
