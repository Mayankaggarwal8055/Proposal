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

I am a CS student at IGNOU and I also work as a Teaching Assistant. A lot of how I learn comes from real systems, real reviews, and real contribution workflows, which is why open source has become such an important part of my growth as a developer.

I started contributing to Airflow because I wanted to understand a serious production-grade project, not just solve isolated coding problems. Very quickly, I ran into something that felt small on the surface but important underneath: Breeze is powerful, but it is not obvious when commands should run on the host and when they should run inside the container.

That confusion matters for people, and it matters even more for AI coding tools.

If a contributor is unsure whether to run `pytest` directly or use Breeze, an AI agent will be unsure too. It will guess, and sometimes that guess will be wrong. The result is familiar: tests pass locally, fail in CI, and the contributor loses confidence in both the workflow and the tool that helped them.

This proposal comes from that firsthand experience. I am not trying to invent an artificial problem. I ran into it while actually contributing to Airflow, and I think solving it would make contribution workflows clearer for humans while also preparing Airflow for AI-assisted development.

---

## Current Contributions

I am already contributing to Airflow while writing this proposal, which matters to me because it shows this is not theoretical interest. I am already in the codebase, already learning the review process, and already shipping small improvements.

### Airflow

- [PR #64355](https://github.com/apache/airflow/pull/64355) - Fix documentation link in Helm chart. Merged.
- [PR #64351](https://github.com/apache/airflow/pull/64351) - Fix task SDK issue. Open as of March 28, 2026.
- [PR #64347](https://github.com/apache/airflow/pull/64347) - Fix inconsistent staging documentation URLs. Open as of March 28, 2026.

### Other Recent Open Source Work

- [Superset PR #38762](https://github.com/apache/superset/pull/38762) - Improve cross-filter scope resolution and performance.
- [Superset PR #38743](https://github.com/apache/superset/pull/38743) - Remove transparency from the filter scope indicator popover.
- [Superset PR #38729](https://github.com/apache/superset/pull/38729) - Restore spacing for charts inside Tabs layout.
- [Superset PR #38656](https://github.com/apache/superset/pull/38656) - Remove double nested overflow scrollbars.
- [Superset PR #38649](https://github.com/apache/superset/pull/38649) - Fix developer docs links. Merged.
- [PouchDB PR #9193](https://github.com/pouchdb/pouchdb/pull/9193) - Guard `Error.stack` access when stack is inaccessible.
- [PouchDB PR #9186](https://github.com/pouchdb/pouchdb/pull/9186) - Update CouchDB Mango documentation link.
- [PouchDB PR #9185](https://github.com/pouchdb/pouchdb/pull/9185) - Surface HTTP 413 via paused/error events.
- [ECharts Website PR #19](https://github.com/apache/echarts-website/pull/19) - Add a scroll-to-top button for improved navigation.

These contributions are across different codebases and different types of work: docs, UI fixes, behavior fixes, and developer experience improvements. That gives me confidence that I can learn project conventions quickly and keep moving even when the workflow is unfamiliar at first.

---

## The Problem

Airflow has a mature development workflow, but a lot of that knowledge lives in documentation, community practice, and contributor experience. That works for humans after some onboarding. It does not work as well for AI agents that need structured guidance.

The most practical example is Breeze.

An agent trying to help with an Airflow PR needs to know things like:

- Am I on the host machine or inside the Breeze container?
- Should this command run directly, through `breeze`, or inside a Breeze shell?
- Are static checks and tests supposed to run in the same environment?
- If CI failed, was the problem in the code or in how the workflow was executed?

Without that context, the agent has to guess. Sometimes it guesses correctly. Sometimes it does not. The result is wasted time, confusing failures, and less trust in AI-assisted contribution workflows.

This hurts three groups:

- New contributors, because environment issues feel random and hard to debug.
- AI tools, because they appear unreliable when they are missing workflow context.
- Airflow itself, because contributor onboarding stays harder than it needs to be.

The core issue is not that Airflow lacks a workflow. The issue is that the workflow is not exposed in a machine-readable form that agents can consume reliably.

---

## What I Am Building

I want to build a Breeze Agent Toolkit that gives AI agents structured answers to the questions they currently guess about.

### 1. Environment Detection

The toolkit will detect whether it is running:

- on the host
- inside a Breeze container
- in CI

It will also capture helpful context such as Python version, Docker availability, repository root, and the recommended command style for the current environment.

Example result:

```json
{
  "is_inside_breeze": false,
  "is_ci_environment": false,
  "docker_available": true,
  "python_version": "3.11",
  "recommended_command": "breeze exec pytest tests/models",
  "reason": "Tests should run in the Breeze container for an Airflow-consistent environment"
}
```

### 2. Skill Definitions

The toolkit will expose common development workflows as structured skills.

Examples:

- static checks
- unit tests
- integration tests
- documentation verification

Each skill will describe:

- what context it requires
- what command template it uses
- what parameters it accepts
- what success and failure look like

This makes Breeze knowledge explicit instead of implicit.

### 3. Workflow Execution

The toolkit will execute supported workflows safely, capture output, and return structured results that an AI agent can understand.

That means an agent should not only know what command to run, but also how to interpret the result and what the likely next step is.

### 4. Auto-Sync and Documentation

If skill definitions drift away from the real Breeze CLI, the toolkit becomes less useful. So part of the project is making sure these definitions stay aligned through generation or drift checks, and documenting how agent-based tools can use the system.

---

## Technical Approach

The implementation will be Python-first and built around small, testable components.

At a high level, I expect the toolkit to have:

- an environment detector
- a skill schema and validator
- a workflow executor
- tests for detection logic and execution paths

I plan to use:

- Python for the implementation
- structured JSON or typed schema objects for skill definitions
- subprocess-based execution with controlled output parsing
- pytest for unit and integration tests

The main design goal is reliability, not unnecessary complexity. I want the toolkit to be easy to reason about, easy to extend, and closely tied to actual Airflow development workflows.

---

## Timeline

This project is scoped for 10 weeks and about 280 hours of work.

### Phase 1: Foundation (Weeks 1-3)

Focus:

- study Breeze internals and environment markers
- document how host, container, and CI contexts can be detected
- build the first version of the environment detector
- write tests for detection logic

Deliverable:

- a working detector that returns structured context and recommendations

### Phase 2: Skills and Execution (Weeks 4-7)

Focus:

- define the skill schema
- implement core workflows for static checks and tests
- build the executor that validates context before running commands
- parse outputs into structured results
- test the workflows against realistic Airflow development scenarios

Deliverable:

- a usable workflow layer that helps agents run the right commands in the right environment

### Phase 3: Polish, Sync, and Documentation (Weeks 8-10)

Focus:

- add auto-sync or drift detection so skills stay aligned with Breeze
- improve error handling and edge-case coverage
- write contributor and integration documentation
- prepare the project for upstream review

Deliverable:

- a documented, tested, review-ready toolkit

---

## Why I Think I Can Deliver This

First, I am already contributing to Airflow. I am still new to the project, but that is also part of why this idea is grounded. I am seeing the rough edges from the perspective of an active contributor rather than from a distance.

Second, I have experience moving across codebases. My recent PRs are not all in one ecosystem, and that has taught me how to read unfamiliar workflows, adapt to different review cultures, and keep momentum across projects.

Third, I like problems that sit between developer experience and system design. This project is not only about writing code. It is about taking workflow knowledge that exists informally and turning it into something structured, usable, and maintainable.

I also want to be honest about scope: I would rather deliver a smaller, reliable toolkit than promise a large system that is hard to maintain. That is why the proposal is centered on a few core workflows first, with sync and documentation as part of the finishing stage.

---

## Availability and Communication

I can dedicate 8 to 10 focused hours per day, 6 days a week, during the GSoC period. I do not have major exam conflicts in June through August, and my TA work is flexible enough that I can keep the project as my main focus.

My communication plan is simple:

- weekly mentor sync with working progress to show
- regular commits so progress stays visible
- quick communication when blocked instead of waiting too long

I care a lot about communicating early and honestly. If something is harder than expected, I will say it. If scope needs adjustment, I will raise that early rather than hide it.

---

## Success Criteria

By the end of the project, I want the following to be true:

- the toolkit can reliably distinguish host, Breeze container, and CI contexts
- core Airflow workflows are defined as structured skills
- agents can use those skills to run checks and tests more safely
- skill definitions stay aligned with Breeze through sync or drift detection
- documentation is clear enough for contributors and tool builders to adopt the system

If we reach that point, Airflow will have taken a real step toward being friendlier to AI-assisted development without compromising the correctness of its existing workflow.

---

## Final Thoughts

What makes this proposal interesting to me is that it sits at the intersection of contributor experience and the future of development tools.

Airflow already has the right workflow discipline. The missing piece is making that workflow legible to agents that are increasingly becoming part of how people write code. If we solve that well, we help new contributors today and make Airflow more ready for AI-assisted contribution tomorrow.

I want to build something practical, upstreamable, and useful from day one. I am already contributing to Airflow, and I would like the chance to keep doing that through a project that improves the developer experience in a concrete way.

---

**Ready to build this.**
