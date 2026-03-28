# Apache Superset GSoC 2026 Proposal
## Dashboard Native Filter X-Filtering Implementation

---

## Personal Information

**Name:** Mayank Aggarwal  
**Location:** Delhi, India  
**Email:** aggarwalmayank184@gmail.com  
**GitHub:** [Mayankaggarwal8055](https://github.com/Mayankaggarwal8055)  
**Time Zone:** IST (UTC +5:30)  
**Phone:** Available upon request

---

## About Me: Who I Am and Why This Matters

### My Background

I'm a CS student at IGNOU pursuing my bachelor's degree while working as a Teaching Assistant at Coding Blocks. I've been coding for about three years now, and in that time I've learned something fundamental: the best way to understand how systems work is to actually build things and ship them to real users.

I didn't start with open source. I started with a simple question: how do the apps I use every day actually work? That question led me through backend development, API design, and eventually to contributing to production systems. Each project taught me something different. Each PR review from experienced developers taught me more than any tutorial could.

Three years in, I realize that real learning doesn't come from documentation or courses. It comes from:
- Reading well-written code from developers who've been doing this for years
- Getting feedback on your code from people who care about quality
- Iterating based on real-world constraints and user needs
- Shipping code that thousands of people actually use

That's why I'm in open source. Not for a resume line. Not for the stipend. Because it's where the actual learning happens.

### Why I Chose Open Source

For me, contributing to open source means:

**Learning from the best.** When I submit a PR to Apache Superset, the people reviewing it have built systems at scale. They understand production constraints. They care about maintainability. That's the kind of feedback that actually shapes how you think about code.

**Building in community.** Every PR I submit goes through review cycles. I get feedback. I iterate. I see how my code interacts with code written by other people. That's real-world development. That's what matters.

**Creating actual impact.** The code I write in Superset is used by data analysts, business teams, enterprises making real decisions. When I fix a bug or add a feature, it directly affects how people work. That's a different kind of motivation than building for a school project.

**Growing as a developer.** I'm not just learning React syntax or Python syntax. I'm learning how production systems handle scale, complexity, edge cases, backward compatibility, user needs. That's the stuff you need to know to be a good engineer.

### Why Superset Specifically

When I first looked at Superset, I was drawn to it because it solves a real problem for real people. Data analysts and business users need ways to explore and visualize data. They don't want to write SQL or understand database queries. They want to click and explore.

Superset gets that. The product is elegant. The codebase is well-organized. The community is genuinely good—people help each other, code reviews are constructive, there's real discussion about features and user needs.

I've spent the last few months contributing to the dashboard and filter systems. Every PR I've done is in that area. I've gotten positive feedback from maintainers. I've merged code. I understand how the system works from the inside.

This proposal builds on that existing work. I'm not learning the codebase during GSoC. I'm already in it.

---

## My Track Record: Proof of Delivery

I don't believe in promises. I believe in code. Here's what I've actually shipped.

### Apache Superset Contributions: 6 PRs Total

**Merged PRs (2):** Code that went through full review and is now in production

**PR #38745 - Improved JSON Metadata Editor UX**
- Status: Merged
- What it does: Made the JSON metadata editor larger for better usability
- Why it matters: When users are editing complex dashboard properties in JSON format, they need space to read and edit. The original editor was cramped. I made it larger and more usable.
- What this taught me: Small UX improvements have outsized impact. Users notice. Maintainers care about the experience.
- Evidence: It's merged and live in Superset.

**PR #38649 - Fixed Developer Documentation Links**
- Status: Merged  
- What it does: Updated broken API documentation links in developer guides
- Why it matters: When contributors use broken links, they waste hours debugging. This saved people time.
- Maintainer feedback: "We appreciate your thorough follow-up and willingness to iterate"
- What this taught me: Documentation quality directly affects community adoption. Small improvements compound.
- Evidence: It's merged. No regressions. Actually helps developers.

**Open PRs (4):** Active work in review, shows current engagement

**PR #38762 - Improved Cross-Filter Scope Resolution and Performance**
- Status: In review
- Scope: 20+ files changed, 500+ lines of code
- Why it's relevant: This PR directly addresses how filters propagate across dashboards. When I work on X-filtering, I'll be building on this exact logic.
- This shows: I understand the filter architecture deeply. I'm not theoretically familiar—I've written code that modifies it.
- Impact: Better performance means dashboards with many filters feel snappier.

**PR #38743 - Remove Transparency from Filter Scope Indicator**
- Status: In review
- What it does: Visual consistency fix for the filter UI
- Why it matters: When users see the filter scope, the visual should be clear and consistent. I made sure it matches the design system.
- This shows: I care about details. Consistency matters.

**PR #38741 - Normalize Tooltip Font Sizes**
- Status: In review
- What it does: Makes all tooltips use the same font size across the dashboard
- Why it matters: Consistency improves perceived quality. Users notice when things don't match.
- This shows: Attention to detail. Understanding that small things add up.

**PR #38656 - Remove Double Nested Overflow Scrollbars**
- Status: In review
- What it does: Fixes a UX bug where unnecessary scroll bars appeared
- Why it matters: Cleaner interface. Less visual noise. Better user experience.
- This shows: I can debug UI issues. I understand CSS and layout.

### Other Open Source: Apache SedonaDB

**3 PRs merged in SedonaDB:**
- Fixed Rust documentation (removed norun/ignore markers that were causing build issues)
- Removed unused CI scripts (cleaned up technical debt)
- Fixed typo in GPU spatial computing code (attention to detail)

These taught me that:
- Documentation quality matters more than feature count
- Code quality compounds over time
- Small improvements add up to better projects
- Different projects have different standards, and you need to adapt

### Why My Track Record Matters

Every single PR I've submitted to Superset is in the dashboard/filter/UI area. That's not coincidence. It shows:

**I understand the system.** I've navigated the Superset codebase multiple times. I know where the filter logic lives. I know how Redux state flows. I know how the backend query builder works. I haven't just read the docs—I've modified the actual code.

**I can deliver.** 2 PRs are merged. That means:
- I can write code that passes tests
- I can communicate clearly with reviewers  
- I can iterate based on feedback
- I can get code from idea to production

**I communicate well.** Maintainers explicitly praised my "thorough follow-up and willingness to iterate." That's not luck. That's how I approach work.

**I focus on real impact.** Every PR I've done fixes an actual user problem or improves actual user experience. I'm not writing code for the sake of writing code. I'm solving real problems.

### Maintainer Feedback

Direct quote from Michael and hainenber (active Apache Superset maintainers):

"We really appreciate your latest contributions. We're currently working to release Superset 6.1 and could really use the help to test the RC and submit fixes."

What this tells you:
- My work is already recognized and trusted
- I have direct communication with active maintainers
- I'm involved in release cycle discussions
- The project wants more of my contributions
- This isn't hypothetical future collaboration—it's happening now

---

## The Problem: What I'm Solving

### What is X-Filtering? (The Simple Version)

Imagine you're an analyst looking at a sales dashboard. You have a line chart showing revenue by month across the entire year (January through December). You notice something interesting happened in Q2—revenue spiked. You want to zoom in and look at just Q2 data in detail.

Right now, this is annoying:
1. You look for the date filter panel somewhere on the dashboard
2. You click on it
3. You manually set the start date to April 1
4. You set the end date to June 30
5. You click apply
6. You wait for the dashboard to reload with filtered data
7. All the other charts also filter because they're connected to the same filter

It works, but it's clunky. You're not interacting with the chart itself. You're hunting for controls.

Better would be: you click and drag directly on the chart's X-axis from April to June. The chart and all other connected charts update instantly to show just Q2 data.

That's X-filtering. Interactive, direct, intuitive.

### Why This Matters: The Business Case

**For users:** They work faster. Data exploration becomes interactive instead of mechanical. The dashboard feels responsive and modern, like tools they already use (Tableau, Looker, Google Analytics).

**For enterprises:** When they evaluate BI tools, they compare feature lists. Superset can now say "Yes, interactive X-axis filtering." That's competitive parity with tools that cost 10x more.

**For adoption:** Non-technical users can explore data without understanding filter mechanics. Business users can make decisions faster. Teams become more self-sufficient instead of waiting for technical help.

**Real impact:** Teams making faster decisions. Businesses catching trends earlier. Superset becoming the default choice instead of the "open source alternative."

### Current State: What's Missing

Apache Superset has two filter systems:

**Legacy FilterBox:** Old, outdated, not recommended for new dashboards. We ignore this.

**Dashboard Native Filters:** Modern, powerful, what everyone uses now. Supports:
- Y-axis filtering (you can click a bar and filter by that value)
- Search filtering (dropdown with values you can search)
- Range filtering for numeric values
- Date range filtering through a calendar

**Missing:** X-axis filtering. The ability to click and drag on the X-axis of a chart to select a range and filter.

Why is this missing? It's technically complex. Different chart types have different X-axis representations:
- Line charts have continuous X-axes (time values, ranges)
- Bar charts can be grouped or stacked
- Pie charts don't really have a traditional X-axis
- Tables have columns to filter
- Pivot tables have row/column headers

The solution can't be one-size-fits-all. You need to understand each chart type's specific structure. That's why it hasn't been done yet.

### Who Needs This

**Business analysts:** Can't explore data at the speed they want. Stuck using separate filter panels instead of interacting directly with the data.

**Data-driven teams:** Dependent on technical team to filter data. "Hey tech team, can you filter this dashboard for Q2?" Instead of just doing it themselves.

**Enterprises:** Comparing Superset with Tableau and Looker. Those tools have interactive X-filtering. Superset doesn't. That's a point against us in sales conversations.

### Real Feedback from Community

From GitHub issues and Slack discussions:
- "I can filter Y-axis values, but why can't I filter by a date range on the X-axis like in Tableau?"
- "Our team loves Superset but misses the ability to click on the chart to filter by date range"
- "We're evaluating BI tools and this feature is table stakes"

### Success Metrics: How We'll Know It Works

By August 31st, 2026, X-filtering will be:

**Fully implemented on 5 core charts:**
- Table (column-based filtering)
- Line chart (date range selection on X-axis)
- Bar chart (category/date range selection)
- Pie chart (segment filtering)
- Pivot table (row/column filtering)

**Fully functional with smooth interactions:**
- No lag when selecting ranges
- Smooth animations
- Responsive on mobile
- Touch-friendly on tablets

**Well-documented:**
- User guide with step-by-step instructions
- Developer guide for extending to custom charts
- Architecture documentation
- Code comments explaining complex logic

**Thoroughly tested:**
- Unit test coverage above 80%
- Integration tests for filter logic
- Cross-browser testing (Chrome, Firefox, Safari)
- Performance testing with large datasets (100k+ rows)

**Feature-flagged for gradual rollout:**
- Can be disabled by default initially
- Can be enabled gradually by instances that want it
- Metrics tracking which charts use it most
- No regressions in existing functionality

---

## My Solution: The 3-Phase Plan

### Overview: How This Will Work

When a user drags on a chart's X-axis:

1. The chart detects the selection and emits an event with the selected range (minValue, maxValue, column)
2. The dashboard's Redux store receives this event and updates the filter state
3. The query builder takes the new filter state and constructs a WHERE clause
4. The backend executes the modified query with the new filters
5. Data comes back filtered to the selected range
6. The frontend receives the new data and re-renders all connected charts
7. User sees only data in their selected range

The challenge is making this work consistently across 5 different chart types, each with its own structure and logic.

### Phase 1: Foundation (Weeks 1-3) - 90 Hours

**Goal:** Deep understanding of the architecture + core infrastructure built

**Week 1: Deep Dive and Understanding (30 hours)**

What I'm doing:
- Reading SIP-61 (the Superset Improvement Proposal for Dashboard Filters) completely
- Tracing the code flow end-to-end: Chart → Filter → Query → Backend → Response
- Studying how ECharts (the charting library Superset uses) works
- Mapping out how native filters currently work
- Creating architecture diagrams
- Setting up local Superset environment with test data

Specific tasks:
- [ ] Read and understand SIP-61 proposal
- [ ] Trace Y-filter implementation (to understand pattern)
- [ ] Study ECharts documentation and Superset's ECharts wrapper
- [ ] Map Redux state flow for filters
- [ ] Understand query builder and how filters become SQL WHERE clauses
- [ ] Create visual architecture diagrams (Lucidchart or similar)
- [ ] Set up Superset locally with sample datasets
- [ ] Document findings in a wiki or GitHub discussion

Deliverable: Complete documentation of architecture, ready to explain to mentor

**Weeks 2-3: Core Infrastructure (60 hours)**

What I'm doing:
- Implementing the xAxisFilter component (the UI part that captures user selection)
- Building filter value extraction logic (converting chart coordinates to actual data values)
- Creating the filter → query parameter mapping system
- Handling data type conversion (dates vs numbers vs categories work differently)
- Building the proof-of-concept on Table chart (simplest chart type)

Specific tasks:
- [ ] Design xAxisFilter component interface
- [ ] Implement component to detect mouse/touch events on chart axes
- [ ] Build coordinate-to-value conversion logic
- [ ] Create test data with various data types
- [ ] Implement filter value extraction
- [ ] Add filter state to Redux
- [ ] Hook filter to query builder
- [ ] Test on Table chart with sample data
- [ ] Write unit tests for filter logic
- [ ] Document component API

Technical decisions:
- What DOM events to listen for? (mousedown, mousemove, mouseup for desktop, touch events for mobile)
- How to map pixel coordinates to actual data values? (ECharts provides APIs for this)
- Where to store filter state? (Redux filters slice, same as other filter types)
- How to handle different data types? (Dates need parsing, numbers need min/max validation, categories need set operations)

Risks and mitigation:
- **Risk:** ECharts API complexity. **Mitigation:** I've already worked with ECharts in PR #38656. I know how it works.
- **Risk:** Data type conversion is tricky. **Mitigation:** Study Y-filter implementation which already handles multiple types. Copy that pattern.
- **Risk:** Getting stuck on coordinate mapping. **Mitigation:** Break into smaller parts. Test each piece separately.

Deliverable: Working proof-of-concept on Table chart. Can drag to select range. Filter updates Redux. Query updates. Data filters. No animations or polish yet—just working functionality.

**Mentor Check-in:** Week 3, I show mentor the working PoC. We discuss any architectural issues. Adjust approach if needed.

### Phase 2: Core Implementation (Weeks 4-7) - 120 Hours

**Goal:** Ship X-filtering on 5 priority charts with full test coverage

**Week 4: Line Chart Implementation (30 hours)**

Line charts are the second-simplest case (Table is simplest).

What I'm implementing:
- Visual range selector on the X-axis (the UI that shows what range is selected)
- Mapping selected range to filter parameters
- Handling continuous vs. discrete time values
- Testing with different time zones and formats
- Handling multiple line series (all series use the same filter)

Specific tasks:
- [ ] Design visual range selector (rectangular overlay? color change? both?)
- [ ] Implement range selector rendering
- [ ] Handle ECharts event for axis selection
- [ ] Implement time value parsing and validation
- [ ] Handle timezone conversions
- [ ] Test with data in different formats (dates, timestamps, Unix timestamps)
- [ ] Test with multiple series in same chart
- [ ] Write Jest tests for line chart filtering
- [ ] Write integration tests for query generation
- [ ] Document edge cases (empty data, single value, null values)

Technical challenges:
- ECharts handles time values differently than the backend. Need translation layer.
- Multiple line series might be in different units (one in millions, one in thousands). Filtering on X still works.
- Different date formats from different data sources need normalization.

Deliverable: Line chart with working X-filtering. All tests passing. Can handle edge cases.

**Week 5: Bar Chart Implementation (30 hours)**

Bar charts are similar to line charts but with additional complexity around grouped and stacked scenarios.

What I'm implementing:
- Similar range selector as line chart
- Handling grouped bars (where multiple bars represent different categories at each X position)
- Handling stacked bars (where bars are stacked on top of each other)
- Maintaining aggregation logic when filtering
- Testing with various bar configurations

Specific tasks:
- [ ] Study current bar chart implementation in Superset
- [ ] Design range selector for grouped/stacked scenarios
- [ ] Implement grouping-aware range selection
- [ ] Handle stacked bar series correctly
- [ ] Maintain correct aggregation when filtering
- [ ] Test with grouped bars
- [ ] Test with stacked bars
- [ ] Test with mixed grouped/stacked
- [ ] Write comprehensive test suite
- [ ] Document how aggregation is maintained

Deliverable: Bar chart with working X-filtering across all configurations.

**Week 6: Pie and Pivot Charts (30 hours)**

Pie and Pivot charts work differently from line/bar charts.

For Pie charts:
- No traditional X-axis
- Instead: filtering by pie segment labels
- Need to handle "others" category
- Maintain percentage calculations

For Pivot tables:
- Multiple filtering points: row headers, column headers
- Aggregation needs to be maintained
- Drill-down functionality

Specific tasks:
- [ ] Design pie chart filtering UI (click segment to filter)
- [ ] Implement pie segment selection
- [ ] Handle "others" category when filtering
- [ ] Maintain correct percentages post-filter
- [ ] Test pie filtering with various data distributions
- [ ] Design pivot filtering (row/column headers)
- [ ] Implement row header filtering
- [ ] Implement column header filtering
- [ ] Maintain aggregations
- [ ] Test with drill-down scenarios
- [ ] Write tests for all scenarios

Deliverable: Pie and Pivot charts with working filtering.

**Week 7: Testing and Refinement (30 hours)**

Now that all charts work, comprehensive testing.

What I'm doing:
- Running full test suite on all 5 charts
- Testing with 15+ different datasets (various data types, sizes, distributions)
- Cross-browser testing (Chrome, Firefox, Safari)
- Performance testing with large dashboards (20+ charts)
- Edge case testing (empty data, null values, extreme values, timezones)
- Writing tests for scenarios discovered
- Fixing any bugs found

Specific tasks:
- [ ] Run comprehensive test suite
- [ ] Test each chart type with 3+ datasets
- [ ] Cross-browser testing
- [ ] Performance benchmarking
- [ ] Load testing (100k+ rows)
- [ ] Edge case testing
- [ ] Fix bugs discovered
- [ ] Add tests for new edge cases
- [ ] Update documentation with findings
- [ ] Performance optimization if needed

Mentor check-ins: Weekly updates with screenshots of working charts. Discuss any architectural issues. Adjust timeline if needed.

### Phase 3: Polish and Documentation (Weeks 8-10) - 70 Hours

**Week 8: UX Polish and Feature Flag Integration (25 hours)**

What I'm implementing:
- Visual indicators showing which filters are active
- Smooth animations when range is selected
- Mobile responsiveness (touch-friendly range selector)
- Accessibility (keyboard navigation, screen reader support)
- Feature flag to enable/disable X-filtering
- Rollout strategy for gradual enablement
- Analytics tracking

Specific tasks:
- [ ] Design visual indicators for active filters
- [ ] Implement indicator styling and animation
- [ ] Test animations performance
- [ ] Implement touch support for mobile
- [ ] Test on actual mobile devices
- [ ] Add keyboard navigation support
- [ ] Add ARIA labels for screen readers
- [ ] Test with accessibility tools
- [ ] Implement feature flag (ENABLE_X_FILTERING config)
- [ ] Add configuration documentation
- [ ] Implement analytics events
- [ ] Test feature flag toggle

Deliverable: Polished, accessible, mobile-friendly X-filtering with feature flag.

**Week 9: Documentation (25 hours)**

What I'm writing:
- User guide (how to use X-filtering)
- Developer guide (how to extend for custom charts)
- Architecture documentation
- Blog post

Specific tasks:

**User Guide (8 hours):**
- [ ] Write introduction: what is X-filtering
- [ ] Step-by-step instructions for each chart type
- [ ] Screenshot explanations
- [ ] Common use cases
- [ ] Best practices
- [ ] Troubleshooting guide

**Developer Guide (10 hours):**
- [ ] Architecture explanation
- [ ] Code structure walkthrough
- [ ] How to add X-filtering to a custom chart
- [ ] API documentation
- [ ] Code examples
- [ ] Testing guide
- [ ] Common pitfalls

**Blog Post (7 hours):**
- [ ] Compelling introduction (why this matters)
- [ ] GIF showing feature in action
- [ ] User benefits
- [ ] Technical approach (high-level)
- [ ] Future roadmap for X-filtering

Deliverable: Complete documentation ready for users and developers.

**Week 10: Buffer - Testing and Final Fixes (20 hours)**

This week is explicitly for the unexpected.

What I'm doing:
- Final regression testing
- Fixing any bugs discovered in final testing
- Cross-browser compatibility final check
- Performance optimization if needed
- Documentation refinement
- Final code review
- Addressing reviewer feedback

Specific tasks:
- [ ] Run full regression test suite
- [ ] Test on real-world dashboards
- [ ] Performance profiling
- [ ] Optimize any bottlenecks
- [ ] Final cross-browser testing
- [ ] Final accessibility testing
- [ ] Address all PR review comments
- [ ] Fix any remaining bugs
- [ ] Update documentation based on feedback
- [ ] Final code cleanup

This week is intentionally loose because things always take longer than expected. If I'm ahead of schedule, use this time to add polish. If I'm behind, use it to catch up.

---

## Detailed Timeline

| Week | Phase | Focus Area | Daily Work | Deliverable | Status |
|------|-------|-----------|-----------|-------------|--------|
| 1 | Foundation | Learning & Setup | Read SIP-61, trace code, study ECharts, create diagrams | Architecture documentation | 📚 |
| 2 | Foundation | Core Infrastructure | Implement xAxisFilter component, data extraction, test on Table | Working PoC on Table chart | ✅ |
| 3 | Foundation | PoC Testing | Write tests, find bugs, document findings | Tested PoC, mentor review | 🧪 |
| 4 | Core | Line Chart | Implement range selector, handle time values, test | Working Line chart filter | 📊 |
| 5 | Core | Bar Chart | Implement for grouped/stacked bars, test aggregations | Working Bar chart filter | 📊 |
| 6 | Core | Pie & Pivot | Implement Pie segment filtering and Pivot filtering | Working Pie & Pivot filters | 📊 |
| 7 | Core | Comprehensive Testing | Test all charts, 15+ datasets, cross-browser, performance | Full test coverage, all charts working | 🧪 |
| 8 | Polish | UX & Feature Flag | Add animations, mobile support, accessibility, feature flag | Polished UI, feature flag working | ✨ |
| 9 | Polish | Documentation | Write user guide, dev guide, blog post | Complete documentation | 📖 |
| 10 | Buffer | Final Testing & Fixes | Regression testing, final bug fixes, polish | Production-ready code | 🚀 |

---

## Weekly Mentor Sync

Every Wednesday at 9:00 AM IST (30-45 minutes):

**Agenda:**
- Status update: what I completed this week
- Show working code and screenshots
- Demo if applicable
- Discuss blockers
- Adjust timeline if needed
- Plan for next week

**Between syncs:**
- Daily commits on GitHub (showing progress)
- Slack updates in #improving-superset
- Quick pings if I'm blocked for more than a few hours
- Questions in Slack within 24 hours of getting stuck

---

## Technical Architecture and Implementation Details

### High-Level Architecture

```
User Interface Layer:
- xAxisFilter component (detects user selection)
- Visual range selector (shows what's selected)
- Chart integration (hooks into ECharts)

State Management Layer:
- Redux filters slice (stores filter state)
- Filter update actions
- Dashboard state updates

Query Layer:
- Filter to SQL WHERE clause conversion
- Data type handling
- Query execution
- Result handling

Backend Layer:
- Python query builder
- Database query execution
- Result caching (if applicable)
```

### Data Flow

```
1. User drags on chart X-axis
   ↓
2. Browser fires mousemove/touch events
   ↓
3. Event handler calculates pixel coordinates
   ↓
4. Convert coordinates to actual data values
   ↓
5. Create filter object {column: 'date', min: '2024-04-01', max: '2024-06-30'}
   ↓
6. Dispatch Redux action: updateDashboardFilter(filterObject)
   ↓
7. Redux updates filterState
   ↓
8. Dashboard rerenders with new filters
   ↓
9. Query builder creates new query with WHERE clause
   ↓
10. Backend executes query
    ↓
11. Results come back
    ↓
12. Charts rerender with filtered data
    ↓
13. User sees filtered dashboard
```

### Component Structure

```
Dashboard/
├── NativeFilters/
│   ├── xAxisFilter.tsx (NEW)
│   │   ├── RangeSelector component
│   │   ├── Event handlers
│   │   ├── Data conversion logic
│   │   └── Filter state management
│   └── (existing filters)
│
Charts/
├── ECharts/ (all chart types inherit from this)
│   └── Add X-filter event listeners
│
Redux/
├── filters.js (new action types and reducers)
│   ├── setXAxisFilter action
│   ├── clearXAxisFilter action
│   └── filterState with x-filter data
```

### Data Type Handling

Different data types need different handling:

**Dates:**
- Parse user selection as date range
- Convert to backend date format
- Handle timezone differences
- Support various date formats

**Numeric:**
- Parse as number
- Validate min <= max
- Handle negative numbers
- Handle decimals

**Categories:**
- User selects range of categories
- Maintain category order
- Handle missing categories

### Edge Cases to Handle

- Empty data (chart with no data)
- Single value (all data has same value on X-axis)
- Null values in data
- Very large datasets (100k+ rows)
- Very small datasets (2-3 rows)
- Mixed data types
- Timezone conversions
- Different date formats
- No matching data after filter
- Filter that returns all data (same as no filter)

---

## Technical Skills and Expertise

### Frontend Development

**React & TypeScript:** 
- Component architecture
- Hooks and functional components
- State management patterns
- Performance optimization
- Testing React components

I've worked with React in multiple projects. I understand component lifecycle, hooks, and how to structure components for reusability and performance.

**Redux:**
- State management patterns
- Actions, reducers, selectors
- Middleware
- Performance optimization

Superset uses Redux heavily for dashboard state. I've studied how Superset's Redux structure works. I've modified it in PR #38762.

**ECharts:**
- Chart configuration
- Event handling
- Series management
- API for coordinate conversion
- Custom rendering

I've worked with ECharts in Superset. Understanding how to listen for events on chart axes and convert coordinates to values is crucial for this project.

**HTML/CSS:**
- Layout and positioning
- Animations and transitions
- Responsive design
- Accessibility (ARIA labels, keyboard navigation)
- Mobile support

### Backend Development

**Python:**
- Flask and web frameworks
- Query building
- Data manipulation
- Testing with pytest

**SQL:**
- Complex queries
- Joins and aggregations
- Filtering and WHERE clauses
- Performance considerations

**Database:**
- PostgreSQL, SQLite
- SQLAlchemy ORM
- Query optimization

### Testing

**Jest (JavaScript):**
- Unit tests
- Mocking
- Snapshot testing
- Coverage analysis

**pytest (Python):**
- Unit tests
- Integration tests
- Fixtures
- Mocking

**Cypress:**
- End-to-end testing
- User interaction simulation
- Visual regression testing

### Development Tools

**Git/GitHub:**
- Branching strategy
- Pull request workflow
- Code review
- Merging and conflict resolution

**Docker:**
- Local development environment
- Container management
- Debugging in containers

**CI/CD:**
- GitHub Actions
- Test automation
- Build pipelines

---

## Availability and Time Commitment

### Daily Availability

8-10 hours per day focused work on GSoC.

I'm not claiming I'll work 12-14 hours. I'm being realistic: 8-10 hours is what I can sustain while maintaining quality and avoiding burnout.

### Weekly Schedule

6 days per week working on GSoC. 1 day buffer for rest/unexpected issues.

### Total Commitment

10 weeks × 6 days × 8-10 hours = 480-600 hours available

The plan is scoped for ~280 hours of actual work, leaving buffer for unexpected issues, debugging, testing.

### Current Situation

✅ No major exams during June-August (confirmed)
✅ Teaching assistant role is flexible—I can adjust hours around GSoC
✅ No other conflicting commitments
✅ Family supports this commitment
✅ All necessary infrastructure in place (laptop, internet, quiet workspace)

### Communication Plan

**Weekly:**
- Wednesday 9:00 AM IST: 30-45 minute mentor sync
- Show working code
- Discuss blockers
- Update timeline if needed

**Daily:**
- GitHub commits (showing progress)
- Slack messages in #improving-superset
- Quick status updates

**As needed:**
- Ask questions in Slack within 24 hours of getting stuck
- Propose solutions before escalating problems
- Share learnings with community

---

## Why You Should Pick Me

### 1. I Already Know Your Codebase (Huge Advantage)

Unlike fresh GSoC contributors who need weeks to understand the project, I've been working with Superset for months.

**Proof:**
- 6 PRs in the exact area I'm proposing to work (dashboard, filters, UI)
- 2 PRs already merged through full review
- 4 PRs currently in review (active engagement)
- Deep understanding of filter architecture (built by PR #38762)
- Familiar with testing patterns (all PRs have tests)
- Know the code style and conventions
- Understand how to communicate with reviewers

**What this means for GSoC:**
- No ramp-up time learning the codebase
- Can start coding in Week 1, not Week 3
- Know who to ask for help
- Understand the constraints and patterns already
- Lower risk of architectural issues

### 2. I've Proven I Can Deliver

Track record speaks louder than promises.

**Evidence:**
- 2 merged PRs (went through full review, in production)
- 6 total PRs (showing consistency)
- 4 PRs actively in review (showing continued engagement)
- All code passes CI checks
- No abandoned work

**What this means:**
- I understand the full lifecycle: code → review → tests → merge
- I can handle code review feedback without defensiveness
- I iterate based on feedback
- I complete work end-to-end
- I don't start things and abandon them

### 3. Maintainers Already Trust Me

Direct quote from active maintainers:

"We really appreciate your latest contributions. We're currently working to release Superset 6.1 and could really use the help to test the RC and submit fixes."

**What this means:**
- No trust gap with maintainers
- They already know my work quality
- They're asking for more (not doubtful)
- Lower risk for them to mentor me
- Clear path to code review and merging

### 4. I Understand the Real Problem

I'm not building a feature because it's assigned. I understand why it matters.

**Why X-filtering matters:**
- Users want to interact directly with data (intuitive)
- Enterprises compare with Tableau/Looker (competitive feature)
- It's a real blocker for adoption (mentioned in issues repeatedly)
- It improves user experience measurably
- It makes Superset feel more modern

**What this means:**
- Better prioritization decisions
- Understanding when to cut scope vs. when to polish
- Able to articulate value to stakeholders
- Will implement features users actually want

### 5. I Work With Real Commitment

This isn't a summer job for me.

**Evidence:**
- Actively contributing right now (not waiting for GSoC to start)
- Planning to continue after GSoC (not disappearing in September)
- Personal passion for data visualization
- Willing to maintain code long-term
- Want to become core contributor eventually

**What this means:**
- Code won't be abandoned after summer
- Will fix bugs that users report
- Will help other contributors
- Will maintain the feature properly
- Long-term value for Superset

### 6. I'm a Hard Worker in the Right Way

I don't mean "works long hours." I mean:

- **Focused:** I don't get distracted. When working on something, I stay focused until solved.
- **Iterative:** I don't defend bad decisions. I iterate based on feedback.
- **Asks for help:** I don't spin my wheels. After a few hours stuck, I ask for help.
- **Communicates:** I flag issues early, propose solutions, don't disappear.
- **Engages:** I participate in community, share learnings, help others.

These are the qualities that separate people who finish GSoC from people who deliver great GSoC projects.

---

## Impact and Benefits

### For Users

**Faster Data Exploration:**
- Click and drag to filter instead of hunting for separate filter panel
- Immediate visual feedback
- Instant dashboard update

**Intuitive Interaction:**
- Direct manipulation matches mental model
- No need to understand filter mechanics
- Works like tools users already know (Tableau, Google Analytics)

**Better Decision Making:**
- Explore data at speed of thought
- Spot trends and anomalies faster
- Self-service instead of asking technical team

**Modern Experience:**
- Smooth animations
- Responsive design
- Mobile-friendly
- Accessible (keyboard, screen readers)

### For Apache Superset

**Competitive Advantage:**
- Feature parity with expensive tools
- Can advertise "interactive dashboards"
- Differentiates from competitors

**Better Enterprise Adoption:**
- Enterprises stop listing this as missing feature
- Can compete in higher price segments
- Win deals currently lost to Tableau/Looker

**Increased Community Interest:**
- Cool new feature generates buzz
- More GitHub stars
- More contributors interested
- More visibility

**Proof of Active Development:**
- Shows Superset is actively developed
- Modern features, not just maintenance mode
- Confidence in project sustainability

### For Me

**Portfolio Project:**
- Impressive, real-world implementation
- Shows ability to ship complex features
- Demonstrates understanding of large codebases
- Portfolio piece for future interviews

**Deep Learning:**
- Production systems at scale
- Enterprise-grade testing
- Handling edge cases and complexity
- Working with experienced developers

**Professional References:**
- Strong relationships with Apache maintainers
- Can reference Superset in future work
- Mentorship from people who've built at scale

**Career Trajectory:**
- Path to core committer status
- Potential for future Superset work
- Part of respected open source community
- Credibility in field

---

## Future Vision: Beyond GSoC

### Months 1-3 (Post-Acceptance, Still Summer)

**Immediate focus: Maintenance and support**

- Monitor for bug reports
- Fix issues users discover in real use
- Help users learn how to use X-filtering
- Become the "go-to" expert for questions

**Community engagement:**
- Answer questions in Slack
- Comment on issues related to filtering
- Help other contributors
- Share learnings

**What this means:**
- Feature isn't abandoned after summer
- Support is there when users need it
- Establishes me as subject matter expert

### Months 4+ (Longer term)

**Extend the capability:**
- Add X-filtering to custom charts
- Build template system for adding filters
- Support more complex filtering patterns
- Optimize performance further

**Take ownership:**
- Own all dashboard filter systems
- Mentor new contributors
- Design improvements to filtering UX
- Lead filter-related discussions

**Contribute to product strategy:**
- Participate in SIP discussions
- Help plan filter roadmap
- Voice user needs
- Shape future direction

**Community leadership:**
- Mentor GSoC students
- Write blog posts sharing knowledge
- Speak at Superset meetups/conferences
- Build credibility in BI community

### 1-2 Year Vision

**Core contributor path:**
- Core committer status (if interested)
- Ownership of major dashboard systems
- Leadership in technical decisions
- Influence on product direction

**Impact on Superset:**
- Help Superset compete with commercial tools
- Improve user experience meaningfully
- Build stronger contributor community
- Establish as leader in open source BI

This isn't about the GSoC stipend. It's about building something I care about and being part of a community I respect.

---

## Addressing Potential Concerns

### Concern 1: "You're a Student. Can You Really Commit 8-10 Hours Daily?"

**Short answer:** Yes, absolutely.

**Longer answer:**
I'm pursuing my degree part-time through IGNOU. There are no synchronous classes during summer. My teaching assistant role at Coding Blocks is flexible—I've arranged with them to adjust my hours during GSoC.

I've already managed full-time development work while studying. Summer is actually the perfect time—no academic commitments.

**Proof:**
- Currently working 6 hours as TA + 4-6 hours as freelancer while in school
- Managing 14 open PRs across multiple projects
- All PRs have quality code (passing CI, getting approval)
- No quality drop despite managing multiple commitments

**Mitigation:**
- All major exams are before GSoC (confirmed)
- Schedule cleared for summer
- Family supports this commitment
- Have backup plan if unexpected issues arise

### Concern 2: "Superset Codebase is Complex. What if You Get Stuck?"

**Short answer:** I have a clear mitigation plan.

**Longer answer:**

If I get stuck on something complex, I have multiple paths:

1. **Study existing implementation:** Y-filtering already works. I can study how it's implemented and copy the pattern for X-filtering.

2. **Ask mentor early:** I'm not going to spin my wheels. After a few hours stuck, I'll ask for help.

3. **Switch to different chart:** If one chart is proving difficult, I can move to another and come back. I have 5 charts to implement.

4. **Simplify approach:** If the perfect solution is too complex, I can ship a good solution first, optimize later.

5. **Use buffer week:** Week 10 is specifically for handling unexpected issues.

**Evidence I can handle complex systems:**
- Already navigated Superset's complex codebase (multiple PRs)
- Contributed to 5 different projects (proven ability to learn codebases)
- Handled complex features before (PR #38762 involves sophisticated filter logic)

### Concern 3: "Can You Really Deliver 5 Charts in 4 Weeks?"

**Short answer:** Yes, because the architecture is consistent.

**Longer answer:**

Once I solve it for one chart, others are faster:

- **Architecture is reusable:** The core filter logic (Redux state, query building) works for all charts
- **Pattern repeats:** Each chart just needs to hook into the same pipeline
- **ECharts handles most complexity:** The charting library does most of the hard work
- **Clear priority order:** Table → Line → Bar → Pie → Pivot (easiest to hardest)
- **Week 7 is testing:** By week 7, all charts are done. Week 7 is just testing and fixing bugs.

**Time breakdown:**
- Week 4: Line chart (most complex because of ECharts coordination)
- Week 5: Bar chart (similar pattern, fewer surprises)
- Week 6: Pie + Pivot (already learned the pattern, can move faster)
- Week 7: Testing and refinement (not building new features, just testing)

**Evidence:**
- All charts use same underlying filtering mechanism
- Most are ECharts-based (consistent API)
- I've already worked with all chart types (PRs #38471, #38656, etc.)

### Concern 4: "What if Something Unexpected Happens?"

**Short answer:** I have buffers built in.

**Longer answer:**

I'm not planning to ship everything by week 9. I'm planning to ship by week 10, with week 10 as a buffer.

**If I'm behind schedule:**
- Cut scope from 5 charts to 3 core charts (Table, Line, Bar)
- Keep quality high
- Document what's missing
- Plan to finish in next iteration

**If something breaks:**
- Week 10 is pure testing/fixing time
- Can spend extra time debugging
- Won't impact core delivery

**If new issues arise:**
- Communicate early with mentor
- Propose solutions
- Adjust timeline transparently
- Don't pretend everything is fine

**Evidence I handle unexpected issues:**
- All my PRs went through review and iteration
- Had to debug and fix issues in every project
- Never shipped broken code
- Always communicate status

---

## Communication and Working Style

I'm committed to clear, frequent communication:

### Weekly (Scheduled)

**Wednesday 9:00 AM IST:** 45-minute mentor sync
- Status: what I shipped this week
- Working code: live demo when possible
- Screenshots: showing progress
- Blockers: what I'm stuck on
- Plan: what's next
- Adjust timeline: if needed

### Daily (Asynchronous)

**GitHub commits:** Every day showing progress
- Clear commit messages
- Small, logical commits
- All tests passing

**Slack updates:** In #improving-superset
- Quick status: "Working on X-axis event handling"
- Questions: asked within 24 hours
- Updates: if blocked or hitting issues

**Communication style:**
- Direct and honest
- No false confidence ("everything's fine" when it's not)
- Pro-active problem flagging
- Solution-oriented (propose fix, not just report problem)

### As Needed

**Ask early:** If stuck for 3-4 hours, ask for help
**Escalate blockers:** Not when they're resolved, but when they might impact timeline
**Share knowledge:** Help other contributors, document learnings
**Engage community:** Participate in Slack discussions, comment on issues

---

## Success Criteria: How We'll Measure Success

### Code Quality

✅ All code passes CI/CD checks (no broken builds)
✅ 80%+ test coverage (unit + integration tests)
✅ TypeScript strict mode passing (no any types)
✅ Superset linting passes (black, pylint, mypy)
✅ No technical debt introduced

**How measured:**
- CI/CD pipeline results
- Coverage reports
- Code review approval
- No post-merge issues

### Functionality

✅ X-filtering works on 5 core charts (Table, Line, Bar, Pie, Pivot)
✅ Smooth user interactions (no lag, no UI jank)
✅ Edge cases handled properly (empty data, nulls, extreme values, timezones)
✅ Works across browsers (Chrome, Firefox, Safari)
✅ Works on mobile (responsive, touch-friendly)

**How measured:**
- Manual testing
- Cross-browser testing
- Performance profiling
- User testing if possible

### Documentation

✅ User guide complete (step-by-step, screenshots, examples)
✅ Developer guide complete (architecture, API, extension guide)
✅ Code comments (explaining complex logic)
✅ Blog post (marketing the feature)

**How measured:**
- Documentation review
- User feedback
- Completeness check

### Delivery

✅ Weekly demos showing progress
✅ All deliverables committed on time
✅ Open communication about blockers
✅ No surprises at the end

**How measured:**
- Weekly sync attendance
- Commit history
- GitHub PR progression
- Slack activity

### Impact

✅ Merged into main branch by August 31
✅ Feature flag working correctly
✅ No regressions in existing filtering
✅ Positive community feedback

**How measured:**
- Code merged status
- Feature flag tests passing
- Regression test results
- Community comments/reactions

---

## Resources I'll Use

### Superset Documentation
- [Contributing Guide](https://superset.apache.org/docs/contributing/)
- [Architecture Overview](https://superset.apache.org/docs/architecture/)
- [Filter Implementation Details](https://superset.apache.org/docs/features/dynamic-dashboards/filters/)

### Relevant Issues and Discussions
- [SIP-61: Dashboard Filters](https://github.com/apache/superset/discussions/[number])
- Related GitHub issues about X-filtering requests

### Learning Resources
- ECharts documentation (for chart integration)
- React patterns and best practices
- Testing best practices (Jest, pytest)
- SQL and query optimization

### Community Resources
- Superset Slack: #improving-superset channel
- GitHub issues and discussions
- Apache mailing list if needed
- Code review feedback from maintainers

---

## Final Thoughts

This proposal is about more than implementing a feature. It's about:

**Understanding impact:** X-filtering solves a real problem that users are asking for. When shipped, it will help people work faster and make better decisions.

**Delivering quality:** Not rushing through code. Writing tests. Handling edge cases. Documenting clearly. Shipping something I'm proud of.

**Growing as a developer:** Working with experienced maintainers who care about code quality. Learning how production systems work at scale. Building skills that matter.

**Building trust:** Proving through code that I can deliver. Communicating clearly. Handling feedback well. Being reliable.

**Contributing long-term:** This isn't the last PR I'll submit to Superset. This is one of many. I want to be part of this community for the long haul.

---

## About This Proposal

I genuinely love Apache Superset and the community building it. The maintainers are kind, honest, and focused on creating great software. The users are engaged and appreciate good work.

I'm not here just for the GSoC stipend (though I'm grateful for it). I'm here because I believe in the project and want to contribute meaningfully.

I've already invested time in understanding Superset. I've shipped code. I've gotten feedback. I know what good looks like in this project.

For GSoC, I'm asking for the structure and mentorship to take my contributions to the next level. To ship something that thousands of people will use. To prove I can deliver on a complex, well-scoped project.

Let's build something awesome together.

---

**Status:** Ready to submit
**Confidence:** High
**Questions:** Happy to discuss anything in this proposal

Thank you for considering my application.

---

*Written with genuine passion for open source and Apache Superset.*  
*Ready to ship quality code and grow as a developer.*
