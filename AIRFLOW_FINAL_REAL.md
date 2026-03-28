# Apache Airflow GSoC 2026 Proposal
## Breeze Agent Toolkit: Making AI-Assisted Development Work with Airflow

---

## Personal Information

**Name:** Mayank Aggarwal  
**Location:** Delhi, India  
**Email:** aggarwalmayank184@gmail.com  
**GitHub:** [Mayankaggarwal8055](https://github.com/Mayankaggarwal8055)  
**Time Zone:** IST (UTC +5:30)

---

## About Me

I'm a CS student at IGNOU and a Teaching Assistant. I've been learning how open source actually works for the last few years.

I started this summer trying to contribute to Airflow. Very quickly, I hit a practical wall: I didn't understand how Breeze works. When do I run `pytest`? When do I run `breeze exec pytest`? Why do tests pass locally but fail in CI?

The docs say "use Breeze" but don't explain why or when.

Then I realized: if I'm confused about this, AI agents are going to be completely lost.

I'm currently actively contributing to Airflow. I have one PR merged (just happened 37 minutes ago). I have 2-3 more PRs in review right now. I'm not planning to contribute to Airflow—I'm doing it right now.

This project came from me actually hitting this problem while working on Airflow.

---

## My Current Contributions

### Merged (Just happened)

**PR #64355 - Fix documentation link in Helm chart**
[GitHub Link](https://github.com/apache/airflow/pull/64355)
- Status: Merged 37 minutes ago
- What: Updated a broken link in Helm chart docs
- Shows: I'm actively contributing to Airflow right now, not theoretically interested

### Open Now

**PR #64351 - Fix task SDK only**
[GitHub Link](https://github.com/apache/airflow/pull/64351)
- Status: In review (opened 1 hour ago)
- What: Task SDK issue fix
- Shows: Active, ongoing contribution

**PR #64347 - Fix inconsistent staging documentation URLs**
[GitHub Link](https://github.com/apache/airflow/pull/64347)
- Status: In review (opened 3 hours ago)
- What: Documentation inconsistencies
- Shows: I'm not waiting. I'm contributing right now.

### Also contributed to Superset (6 open, 2 merged):

**Open:**
- [PR #38762](https://github.com/apache/superset/pull/38762) - Improve cross-filter scope (20 reviews)
- [PR #38743](https://github.com/apache/superset/pull/38743) - Remove transparency from filter indicator
- [PR #38741](https://github.com/apache/superset/pull/38741) - Normalize tooltip font (18 reviews)
- [PR #38729](https://github.com/apache/superset/pull/38729) - Fix spacing in Tabs
- [PR #38656](https://github.com/apache/superset/pull/38656) - Remove double scrollbars

**Merged:**
- [PR #38745](https://github.com/apache/superset/pull/38745) - Larger JSON metadata editor
- [PR #38649](https://github.com/apache/superset/pull/38649) - Fixed doc links

### Other projects:

**PouchDB (3 open):**
- [PR #9193](https://github.com/pouchdb/pouchdb/pull/9193) - Guard Error.stack access
- [PR #9186](https://github.com/pouchdb/pouchdb/pull/9186) - Update CouchDB docs link
- [PR #9185](https://github.com/pouchdb/pouchdb/pull/9185) - Surface HTTP 413 error

**ECharts (1 open):**
- [PR #19](https://github.com/apache/echarts-website/pull/19) - Add scroll-to-top button

**SedonaDB (3 merged):**
- [PR #710](https://github.com/apache/sedona/pull/710) - Fix Rust docs
- [PR #676](https://github.com/apache/sedona/pull/676) - Remove unused CI scripts
- [PR #463](https://github.com/apache/sedona/pull/463) - Fix typo in GPU code

---

## The Actual Problem

Here's what actually happened to me.

I cloned Airflow. Found a small bug. Fixed it. Wanted to test it.

First instinct: `pytest tests/models/test_dag.py`

It ran. Tests passed. Committed code. Pushed PR.

GitHub Actions ran. **Tests failed in CI.**

Why? The test needs stuff that only exists inside the Breeze container. Database setup. Service configuration. Dependencies installed. Environment variables.

Running pytest outside Breeze is like testing a web server without starting the web server. Some things work. Some things mysteriously fail.

Now imagine GitHub Copilot or Claude Code encounters this. It doesn't know the difference. It runs pytest directly. Tests pass locally. Tests fail in CI. User thinks Copilot is broken. Stops using it for Airflow.

That's happening right now. Multiple times. With multiple tools.

**The root cause:** Airflow's development requires Breeze. But agents don't know that. The repo doesn't tell them. They have to figure it out by reading Breeze documentation and guessing.

---

## What I'm Building

A simple toolkit that answers three questions for AI agents:

1. **Am I on the host or inside the container?**
   - Detects environment (Docker socket, environment variables, etc.)
   - Returns structured answer

2. **What workflows are available to me?**
   - Lists skills: static checks, unit tests, integration tests
   - For each skill: what command, what context needed, expected output

3. **How do I run each workflow?**
   - For each skill: exact steps, command templates, success indicators

**Example output:**

Agent asks: "Should I run pytest?"

Toolkit returns:
```
{
  "is_inside_breeze": false,
  "recommended_command": "breeze exec pytest tests/models",
  "reason": "Tests need Breeze container environment",
  "next_step": "First run: breeze shell"
}
```

Agent asks: "What skills do I have?"

Toolkit returns:
```
{
  "available_skills": [
    {
      "skill": "static_checks",
      "command": "prek run --hook black",
      "context": "host",
      "description": "Run pre-commit hooks"
    },
    {
      "skill": "unit_tests",
      "command": "pytest {test_path}",
      "context": "container",
      "description": "Run unit tests in Breeze"
    }
  ]
}
```

Agent can then execute workflows reliably.

---

## Timeline

**10 weeks, 280 hours**

### Phase 1: Foundation (Weeks 1-3)

**Week 1:** Learn how Breeze works
- Read Breeze CLI code
- Understand environment detection
- List all markers (Docker socket, env vars, etc.)
- Document how to detect context reliably

Deliverable: Documentation of detection approach

**Week 2-3:** Build environment detector
- Class that detects: host vs container, Python version, Docker available, git root, etc.
- Returns structured object
- Comprehensive tests
- Works accurately on real Breeze and host

Deliverable: Working detector with tests

### Phase 2: Implementation (Weeks 4-7)

**Week 4:** Design skill definitions
- JSON schema for skills
- Create 3 example skills: static checks (prek), unit tests, integration tests
- Validator that ensures skills are well-formed

Deliverable: Skill schema + examples

**Week 5:** Build executor
- Takes a skill and executes it
- Captures output
- Parses results
- Returns structured result object

Deliverable: Working executor

**Week 6:** Implement core workflows
- Static checks workflow (prek integration)
- Unit tests workflow (pytest in container)
- Integration tests workflow

**Week 7:** Comprehensive testing
- Test detection on real Breeze
- Test workflows with actual Airflow
- Edge cases
- Performance

Deliverable: All components working together

### Phase 3: Documentation & Polish (Weeks 8-10)

**Week 8:** Auto-sync mechanism
- prek hook that generates skills from Breeze CLI
- Drift detection
- Integration into CI

Ensures skills never go out of sync with Breeze.

**Week 9:** Documentation
- How to use the toolkit
- How AI tools integrate
- API documentation
- Examples

**Week 10:** Testing and fixes
- Final testing
- Bug fixes
- Performance optimization

---

## What Success Looks Like

- Environment detection works 100% accurately
- Skill definitions are validated and complete
- Skills auto-sync with Breeze CLI (no manual updates)
- AI tools can reliably use the toolkit to guide contribution workflows
- Documentation is clear and complete
- All code passes tests
- No regressions in existing Airflow functionality

---

## Technical Details (High Level)

**Stack:**
- Python 3.9+
- Docker SDK (detecting container)
- Subprocess (executing commands)
- Pydantic (schema validation)
- JSON (skill definitions)

**Architecture:**
```
AI Agent
   ↓
Breeze Toolkit API
├─ Environment Detection
├─ Skill Management
└─ Workflow Executor
   ↓
Airflow Development Environment (Host or Container)
```

**Key challenges:**
- Detecting environment reliably (multiple markers needed)
- Parsing output from different tools
- Handling errors gracefully
- Keeping skills in sync with Breeze CLI

---

## Honest Assessment

**I'm new to Airflow.** I have 1 merged PR and 2-3 in review. But I've already been working on the codebase for a few weeks and I'm actively learning.

**This project doesn't require deep Airflow expertise.** It requires understanding Breeze, which I've been analyzing separately. It requires system design thinking, which I have from working on multiple projects.

**If this gets too complex,** I can reduce scope: focus on environment detection first (the core piece), add workflow automation second. The detection part alone is valuable.

**I might discover new challenges.** That's why I have a buffer week and I'll communicate early if problems arise.

---

## Why Me

I'm actually experiencing this problem right now. I'm contributing to Airflow right now. I'm not hypothetically interested—I'm actively working in this space.

I've proven I can learn new codebases quickly (contributed to 5 different projects). I've proven I can ship code (6 merged PRs). I can write clear code that passes review.

I understand the problem deeply because I hit it while actually working on Airflow.

---

## Real Talk

This project is interesting because it's about the future: AI tools becoming standard for development. Airflow that embraces this will stay relevant. Airflow that ignores it might lose contributors to frameworks that don't have this friction.

I'm not trying to sound like a visionary. I'm just noticing that Copilot exists. Claude Code exists. GitHub doesn't make people use Copilot with Airflow—Copilot should just work.

Right now it doesn't. This fixes it.

---

## Communication

**Weekly:** Mentor sync. Show working code. Talk about blockers.

**Daily:** GitHub commits. Slack updates. Ask for help when stuck.

**Honest:** If I'm behind, I'll say it early. If something's harder than expected, I'll mention it. If there's a better approach, I'll explain it.

---

## What You Get

- Working environment detection system
- Skill definition schema with examples
- Complete workflow executor
- Auto-sync mechanism (skills stay in sync with Breeze)
- Clear documentation
- Someone who sticks around

---

**I'm ready to start. I'm actually already started.**

Let me ship this.
