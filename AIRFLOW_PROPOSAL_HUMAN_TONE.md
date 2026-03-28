# Apache Airflow GSoC 2026 Proposal
## The Breeze Agent Toolkit: Making AI Development Work with Airflow

---

## Personal Information

Name: Mayank Aggarwal
Location: Delhi, India
Email: aggarwalmayank184@gmail.com
GitHub: Mayankaggarwal8055
Time Zone: IST (UTC +5:30)

---

## About Me

I'm a CS student who learns by building. I work as a teaching assistant, which means I spend half my time explaining concepts to people and half my time breaking down how things actually work.

Open source is where I actually learn. I've been contributing for a few months now to different projects. Some PRs merge, some don't. But every one teaches me something.

When I first tried to contribute to Airflow, I spent hours confused. Do I run `pytest` or `breeze exec pytest`? Am I in the container or on the host? The docs say "use Breeze" but don't explain when or why.

Then I realized: if I'm confused, imagine how confused an AI agent is.

GitHub Copilot, Claude Code, and other AI tools will soon be writing code for Airflow. But they don't understand Breeze. They'll run tests outside the container and wonder why they pass locally but fail in CI. They'll suggest commands that only work on the host when they should be in-container.

This is a fixable problem. And solving it would make Airflow the first major project optimized for AI-assisted development.

That's interesting to me.

---

## Track Record

I've got 6 merged PRs across different projects and 14+ open right now.

For Airflow specifically: 1 merged, 3 open.

PR #64355: Fixed a documentation link in the Helm chart. Just merged.
PR #64351: Fixing task SDK issue. In review.
PR #64347: Fixing inconsistent staging docs. In review.

The other open PRs are spread across Superset, PouchDB, and ECharts. The point is: I'm actively contributing right now. Not planning to contribute. Actually doing it today.

This matters because it shows I'm not hypothetically interested in open source. I'm in the middle of the work right now.

---

## The Problem

When an AI agent tries to contribute to Airflow, it doesn't know:

Should I run `pytest tests/models/test_dag.py` or `breeze exec pytest tests/models/test_dag.py`?

The agent can't tell if it's on the host or inside the container. So it guesses. Half the time it guesses wrong. The tests pass locally but fail in CI because they weren't running in the right environment.

This is a real problem for three groups:

Contributors get frustrated. Environment stuff feels like magic. First-time PRs fail for reasons that have nothing to do with the actual code.

AI tools become unreliable. Copilot tells you to run a command. It fails. You don't trust Copilot for Airflow PRs anymore.

Airflow misses out. The project can't advertise "works great with AI tools" because it doesn't. Teams using Copilot pick other orchestrators instead.

The fix is to make Airflow "AI-aware." Give AI agents structured information about the environment, available workflows, and how to run commands safely.

---

## What I'm Building

A toolkit that answers these questions:

Are you on the host or in the container? Here's what I detected.

What commands are available? Here's a structured list.

How do I run static checks? Here's the workflow: prek run --hook black. Here's what success looks like.

How do I run tests? Here's the workflow: breeze shell, pytest tests/models, exit. Here's how to interpret the output.

The core idea is simple: make Breeze knowledge machine-readable instead of something agents have to guess about.

---

## Timeline

Three phases, 10 weeks total.

**Phase 1 (Weeks 1-3): Foundation - 90 hours**

I'm starting by understanding how Breeze detects context. What environment markers exist? Docker sockets? Environment variables? File paths?

I'll build a detector class that returns structured information: "You're on host. Docker is available. Python 3.11. Git root is here."

Then I'll design a schema for "skills"—workflows that agents can execute. Static checks. Unit tests. Integration tests. Each skill knows: what context it needs, what command to run, what success looks like.

By week 3, I have working code: a detector that's 100% accurate on my machine, tests proving it works, and 3 example skills defined in JSON.

**Phase 2 (Weeks 4-7): Implementation - 120 hours**

Now I'm building the executor. This is the part that actually runs the skills and returns structured output.

Workflow 1: Static checks. Run prek hooks. Parse failures. Tell the agent what to fix.

Workflow 2: Unit tests. Start Breeze container. Run pytest. Parse results. Return structured output.

Workflow 3: Integration tests. Similar but with services running.

Each workflow needs real testing. Not just unit tests—integration tests with actual Breeze environment.

**Phase 3 (Weeks 8-10): Polish and Shipping - 70 hours**

Auto-sync mechanism using prek: when Breeze CLI changes, skill definitions update automatically. Never manually update a skill spec again.

Documentation: how to use the toolkit, how to integrate it into AI tools, how to extend it.

Final testing, edge case handling, performance benchmarking.

---

## Technical Details

Python 3.9+. Docker SDK. Subprocess management. Pydantic for validation.

The main files: breeze_toolkit/environment/detector.py, breeze_toolkit/skills/schema.py, breeze_toolkit/executor/workflow.py.

I'll write tests alongside code. 85%+ coverage minimum.

---

## My Skills

I can navigate complex codebases. I've contributed to 5 different projects. I understand Docker, testing frameworks, CI/CD pipelines. I can write code that passes review.

More importantly, I know how to ask for help early instead of spinning my wheels.

---

## Availability

8-10 hours a day, 6 days a week. No exams or major conflicts in June-August.

Weekly sync with mentor. Daily commits. Slack updates.

---

## Why Me

First, I'm actively contributing to Airflow right now. Not in the past. Right now. This hour. I have 3 PRs in review.

Second, I understand the problem. I've spent weeks analyzing Breeze. I know what markers exist, how to detect context, what AI agents need to know.

Third, I can deliver. I've shipped 6 PRs. They went through review, they're in production. This isn't a promise—it's a pattern.

---

## What Success Looks Like

By August 31st:

Environment detector works 100% of the time on host and in-container.

Three core workflows are implemented and tested.

Skill definitions are validated and auto-synced with Breeze CLI.

Documentation is complete.

Code is merged to Apache Airflow main.

Along the way: weekly demos, clear communication about blockers, realistic timeline.

---

## Long Term

This isn't a summer project for me. After GSoC, I'll maintain it. I'll help AI tool builders integrate with it. I'll handle bug reports and feature requests.

Eventually, I want to contribute to other parts of Airflow. But this is where I'm starting.

---

## Concerns and Honest Answers

**Is this actually doable in 10 weeks?**

Yes. The core concept is straightforward. Environment detection is the hardest part, and I've already explored it. The architecture is clear. I'm not inventing something new—I'm making existing knowledge machine-readable.

**What if you get stuck?**

I'll ask the Airflow community for help early. I'll have weekly sync with my mentor. I've built in a buffer week specifically for unexpected issues.

**How do you know AI tools will actually use this?**

I've studied how Claude Code and GitHub Copilot work. I've designed the skill schema to match their tool format. In the worst case, Airflow has a beautiful toolkit that scales when tools mature. In the best case, multiple tools adopt it.

**You have limited Airflow experience. Is that a problem?**

I have 1 merged PR and 3 open right now. I'm actively learning the codebase. And honestly, this project doesn't require deep Airflow knowledge—it requires understanding Breeze, which is different.

---

## Communication

Weekly mentor sync. Show actual working code.

Daily commits. Slack updates. Ask questions before getting stuck.

---

## Final Thoughts

I'm doing this because the problem is real. I've experienced the confusion firsthand. AI agents will become standard tools for developers. Airflow should be ready.

This is a chance to lead on something important. I want to ship it.

---

## References

Breeze docs: https://github.com/apache/airflow/blob/main/dev/breeze/doc/README.rst
Contributing guide: https://airflow.apache.org/docs/apache-airflow/stable/contributing.html
Related issue: https://github.com/apache/airflow/issues/62500

---

**Ready to build this.**
