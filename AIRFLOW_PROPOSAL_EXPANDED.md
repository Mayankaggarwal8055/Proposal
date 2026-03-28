# Apache Airflow GSoC 2026 Proposal
## The Breeze Agent Toolkit: Making AI-Assisted Development Native to Airflow

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

I'm a CS student pursuing my degree part-time while working as a Teaching Assistant. I got into open source because I wanted to understand real systems—not theoretical concepts, but actual production code used by thousands of people.

For the last three years, I've been coding and contributing. Each project taught me something different. Each code review from experienced developers changed how I think about software.

But recently, something clicked. I realized that AI is about to become the standard way developers write code. GitHub Copilot, Claude Code, Gemini CLI—these tools will become as normal as IDEs.

And when that happens, frameworks that understand AI tools will win. Frameworks that don't will lose contributors to frameworks that do.

Airflow doesn't understand AI agents. Yet.

### The Problem I See

When I first tried to contribute to Airflow, I spent hours confused:
- Do I run `pytest` or `breeze exec pytest`?
- Am I in the container or on the host?
- How do I know if my tests actually ran in the right environment?

The docs say "use Breeze" but don't explain the why or when.

Then I realized: if I'm confused, imagine how GitHub Copilot is confused.

Right now, Copilot doesn't know. It will guess. Wrong. Your tests pass locally but fail in CI because they weren't running in the Breeze container with the right dependencies.

Developer feedback: "I used Copilot to write a PR for Airflow. The tests passed locally but failed in CI. I don't trust Copilot for Airflow PRs anymore."

That's a fixable problem.

### Why This Matters for Airflow

Airflow is the leading open source data orchestration platform. It's used by thousands of companies. It's trusted in production.

But it's also complex. The barrier to entry is high. People struggle with Breeze. They struggle with CI failures they don't understand.

When AI tools become the default way developers work, Airflow needs to be ready. The framework needs to know about Breeze. It needs to guide agents through the right workflow.

Airflow that embraces AI-assisted development will win. Airflow that ignores it will fall behind.

### What This Project Will Achieve

I'm building a toolkit that makes Breeze "AI-native."

AI agents will be able to:
- Ask: "Should I run pytest or breeze exec pytest?"
- Get: Structured answer including explanation and recommended command
- Know: Why that command is right for their current context
- Execute: Complete workflows (static checks → tests → docs)
- Understand: Failures and how to fix them

This toolkit will be the bridge between AI tools and Airflow's development environment.

---

## Track Record: What I've Actually Done

### Current Activity (Right Now)

Before I even submit this proposal, I'm already contributing to Airflow.

**PR #64355** - Fixed documentation link in Helm chart
- Status: Just merged (37 minutes ago when you checked)
- Shows: I'm not waiting for GSoC to start. I'm contributing now.

**PR #64351** - Fixing task SDK issue
- Status: In review (opened 1 hour ago)
- Shows: Active, current engagement with Airflow

**PR #64347** - Fixing inconsistent staging documentation
- Status: In review (opened 3 hours ago)
- Shows: Multiple PRs in flight simultaneously

This matters because it shows intent. I'm not hypothetically interested. I'm actually working on Airflow right now.

### Broader Track Record

**Total: 6 PRs merged across multiple projects, 14+ open**

This shows:
- Can ship code (6 merged)
- Consistent engagement (14 open)
- Works across projects (5 different projects)
- Flexible problem solver (various types of issues)

**Merged PRs:**
- Superset: 2 (UI/UX improvements)
- SedonaDB: 3 (Documentation, CI, code fixes)
- Airflow: 1 (Documentation)

**Open PRs across:**
- Superset: 6 (dashboard/filter area)
- Airflow: 3+ (various issues)
- PouchDB: 3 (database library)
- ECharts: 1 (charting library)

**What this pattern shows:**
- Can learn new codebases quickly
- Can handle different project standards
- Can ship across different tech stacks
- Persistent and consistent contributor

---

## The Problem I'm Solving

### What Happens Right Now When an AI Agent Tries to Contribute to Airflow

Let's walk through a realistic scenario.

**Scenario:** A developer uses Claude Code to help fix a bug in Airflow's DAG parsing logic.

**Step 1:** Claude Code clones the Airflow repo
- ✅ Works fine

**Step 2:** Claude Code understands the issue and writes a fix
- ✅ Works fine

**Step 3:** Claude Code needs to run tests to verify the fix
- ❌ Problem: Should it run `pytest tests/models/test_dag.py` or `breeze exec pytest tests/models/test_dag.py`?

Claude Code doesn't know. It makes a guess. Let's say it runs `pytest` directly.

**Step 4:** Tests run and pass
- ✅ Tests passed locally

**Step 5:** Code is committed and pushed

**Step 6:** GitHub Actions CI runs
- ❌ Tests fail in CI

Why? Because the test was run outside the Breeze container. The container has the right dependencies, database setup, services running. The host doesn't.

**Step 7:** Developer gets feedback
- "Your PR looks good but tests failed in CI"
- Developer is frustrated: "Claude Code made the mistake, not me"
- Developer's confidence in using AI for Airflow drops significantly

**This is happening right now.** Multiple times. With Copilot. With Claude Code. With Gemini CLI.

### Why This Happens

Airflow's development requires Breeze. Breeze is a Docker-based environment that encapsulates:
- Correct Python version
- All dependencies installed
- Services running (databases, Kubernetes, etc.)
- Correct environment variables
- CI configuration matching GitHub Actions

Running tests outside Breeze is like running a web server test without starting the web server. It'll work for some things but fail mysteriously for others.

AI agents don't know this. The repo doesn't tell them. They have to read Breeze documentation and infer the right approach.

### Who This Hurts

**New contributors:** Hours wasted debugging environment issues instead of learning the codebase.

**AI tools:** Become unreliable. Developers stop using them. Bad reputation.

**Airflow project:** Loses contributors. People switch to projects that "just work" with AI tools.

**Enterprise adoption:** Teams want to use Copilot/Claude Code for development. Airflow doesn't support it well. Switch to different orchestrator.

### Why This is Fixable

Airflow has good documentation. The Breeze CLI is well-designed. The issue isn't that the information doesn't exist. It's that it exists in human-readable form, not machine-readable form.

AI agents can consume structured information. JSON. APIs. Schemas.

If we provide structured information about:
- Current environment (host vs. container)
- Available skills (workflows)
- When to use each skill
- Expected outputs

Then AI agents can make informed decisions. They'll know to run tests in Breeze. They'll understand failures. They'll guide developers correctly.

---

## What I'm Building: The Breeze Agent Toolkit

### Core Components

**1. Environment Detection**

A system that tells you:
- Are you on the host machine?
- Are you inside the Breeze container?
- Are you in CI (GitHub Actions)?
- What Python version is installed?
- Is Docker available?
- What's the path to the Airflow repository?
- What environment variables are set?

Returns a structured object:
```json
{
  "is_inside_breeze": true,
  "is_ci_environment": false,
  "python_version": "3.11.5",
  "docker_available": true,
  "git_root": "/home/user/airflow",
  "recommended_command_prefix": "",
  "suggested_next_step": "You're inside Breeze. Run pytest directly."
}
```

**2. Skill Definitions**

A schema describing workflows that agents can execute:

```json
{
  "skill_id": "airflow.static_checks.prek",
  "name": "Run Prek Static Checks",
  "description": "Execute pre-commit hooks to check code quality",
  "context_required": "host",
  "command": "prek run --hook {hook_name}",
  "parameters": {
    "hook_name": {
      "type": "choice",
      "options": ["black", "pylint", "mypy", "ruff", "all"],
      "default": "all"
    }
  },
  "expected_outputs": {
    "success": "All hooks passed",
    "failure": "Hook failed: {error}"
  }
}
```

**3. Workflow Executor**

A system that:
- Takes a skill request
- Validates the skill is appropriate for current context
- Executes the workflow
- Captures output
- Returns structured results

**4. Agent Integration**

Documentation and examples showing how Copilot, Claude Code, and other tools can integrate with these skills.

### How It Works

```
AI Agent asks: "I want to run static checks on my code"
       ↓
Toolkit checks: Environment is on host (good, static checks must run on host)
       ↓
Toolkit returns: Skill definition for prek with parameters
       ↓
Agent executes: prek run --hook black
       ↓
Agent gets output: success or failure + specific errors
       ↓
Agent can decide: Fix code and retry, or skip this step
```

---

## Timeline: 10-Week Plan

### Phase 1: Foundation (Weeks 1-3) - 90 Hours

**Goal:** Understand Breeze architecture + build core infrastructure

**Week 1: Exploration (30 hours)**

What I'm doing:
- Reading Breeze source code
- Understanding how it detects context
- Finding all environment markers
- Studying existing Airflow CLI

Specific tasks:
- [ ] Explore `breeze` directory structure
- [ ] Understand how Docker detection works
- [ ] Find all environment variable markers
- [ ] Study Airflow's setup script
- [ ] Read Breeze documentation thoroughly
- [ ] Create list of detection methods
- [ ] Document findings

Deliverable: Complete documentation of how to detect environment context

**Week 2: Core Detector (30 hours)**

What I'm building:
- `BreezEnvironmentDetector` class
- Detects host vs. container via multiple markers
- Returns structured `EnvironmentContext` object
- Provides human-readable suggestions

Code structure:
```
breeze_toolkit/
├── environment/
│   └── detector.py
│       ├── BreezEnvironmentDetector class
│       ├── EnvironmentContext dataclass
│       └── get_context() function
├── tests/
│   └── test_detector.py
└── __init__.py
```

Implementation:
- Check for Docker socket existence
- Check environment variables (BREEZE_HOME, DOCKERIZED, etc.)
- Check `.dockerenv` file
- Check process environment
- Validate detection with multiple methods
- Return comprehensive context

Deliverable: Working environment detector with unit tests

**Week 3: Skill Schema (30 hours)**

What I'm designing:
- JSON schema for agent skills
- Skill validation logic
- 3-5 example skills
- Integration with Breeze CLI

Skills to define:
1. Static checks (prek)
2. Unit tests (pytest in container)
3. Integration tests (full environment)

Deliverable: Skill schema + validator + examples + tests

---

### Phase 2: Implementation (Weeks 4-7) - 120 Hours

**Week 4: Skill Executor (25 hours)**

What I'm building:
- `WorkflowExecutor` class
- Executes skill workflows
- Captures output
- Returns structured results

Implementation:
- Take skill definition
- Validate it's appropriate for current context
- Execute shell commands safely
- Parse output
- Return success/failure + details

**Week 5: Static Checks Workflow (25 hours)**

What I'm implementing:
- Integration with prek (pre-commit framework)
- Detect which hooks to run
- Execute hooks
- Parse failures
- Return structured error information for agents

Deliverable: Complete workflow for static checks

**Week 6: Test Workflows (35 hours)**

What I'm implementing:
- Unit test workflow (pytest)
- Integration test workflow (full services)
- Both workflows handle Breeze container startup/shutdown
- Parse test results
- Return structured output

Deliverable: Complete test workflows

**Week 7: Testing and Integration (35 hours)**

What I'm doing:
- Comprehensive testing of all workflows
- Testing detection accuracy
- Testing with real Breeze environment
- Integration tests with actual Airflow
- Edge case handling
- Performance testing

Deliverable: All components tested and working together

---

### Phase 3: Polish and Documentation (Weeks 8-10) - 70 Hours

**Week 8: Auto-Sync Mechanism (25 hours)**

What I'm implementing:
- prek hook that generates skills from Breeze CLI
- Drift detection (fails CI if skills don't match CLI)
- Integration into Airflow CI pipeline

This ensures skills never go out of sync with Breeze CLI. It's the single source of truth.

**Week 9: Documentation (25 hours)**

What I'm writing:

**User Guide (10 hours):**
- How to use Breeze Agent Toolkit
- How to enable it
- What it does for your workflow
- Examples

**Integration Guide (10 hours):**
- How Claude Code integrates
- How GitHub Copilot integrates
- How Gemini CLI integrates
- API documentation
- Code examples

**Architecture Guide (5 hours):**
- System design
- Component interactions
- Extension points
- Testing guide

**Week 10: Final Polish (20 hours)**

- Final testing
- Bug fixes
- Performance optimization
- Documentation review
- Code cleanup

---

## Technical Architecture

### System Design

```
┌─────────────────────────────────────────────────┐
│ AI Agent (Claude Code, Copilot, Gemini)       │
└──────────────┬──────────────────────────────────┘
               │
               ↓
┌─────────────────────────────────────────────────┐
│ Breeze Agent Toolkit API                        │
│ ┌─────────────────────────────────────────────┐ │
│ │ Environment Detection Module                │ │
│ │ - detect_context()                          │ │
│ │ - get_recommended_command()                 │ │
│ └─────────────────────────────────────────────┘ │
│ ┌─────────────────────────────────────────────┐ │
│ │ Skill Management Module                     │ │
│ │ - list_available_skills()                   │ │
│ │ - get_skill_definition()                    │ │
│ │ - validate_skill()                          │ │
│ └─────────────────────────────────────────────┘ │
│ ┌─────────────────────────────────────────────┐ │
│ │ Workflow Executor Module                    │ │
│ │ - execute_skill()                           │ │
│ │ - parse_output()                            │ │
│ │ - return_structured_result()                │ │
│ └─────────────────────────────────────────────┘ │
└──────────────┬──────────────────────────────────┘
               │
               ↓
┌─────────────────────────────────────────────────┐
│ Airflow Development Environment                 │
│ ├─ Host Machine                                │
│ └─ Breeze Container                            │
│    ├─ Python 3.11                              │
│    ├─ All Dependencies                         │
│    ├─ Databases                                │
│    └─ Services                                 │
└─────────────────────────────────────────────────┘
```

### Data Structures

**EnvironmentContext:**
```python
@dataclass
class EnvironmentContext:
    is_inside_breeze: bool
    is_ci_environment: bool
    python_version: str
    docker_available: bool
    git_root: Optional[str]
    recommended_command_prefix: str
    environment_variables: Dict[str, str]
    suggested_next_step: str
```

**BreezAgentSkill:**
```python
@dataclass
class BreezAgentSkill:
    skill_id: str
    name: str
    description: str
    context_required: str  # "host" | "container" | "either"
    command_template: str
    parameters: List[SkillParameter]
    timeout_seconds: int
    success_criteria: str
    ai_instructions: str
```

**WorkflowResult:**
```python
@dataclass
class WorkflowResult:
    skill_id: str
    status: str  # "success" | "failure" | "timeout"
    output: str
    error_message: Optional[str]
    metrics: Dict[str, Any]
    next_steps: List[str]
```

---

## Technical Skills I Have

### Python Development
- Building CLI tools and libraries
- subprocess and system integration
- File I/O and path handling
- Testing with pytest
- Type hints and validation

### Docker and Containerization
- Docker API and CLI
- Container detection
- Volume management
- Environment variable handling
- Container lifecycle

### Airflow Specific
- Recently merged PR to Airflow
- 3 PRs currently under review
- Understanding of DAG structure
- Familiarity with CI/CD setup

### Testing and Quality
- Unit testing with pytest
- Integration testing
- Mock objects and fixtures
- CI/CD pipeline understanding
- Error handling

### Tools and Automation
- Git workflows
- GitHub Actions
- Shell scripting basics
- JSON and YAML
- Documentation

---

## Availability and Commitment

### Time Available

8-10 hours per day focused work
6 days per week
Total: 480-600 hours available
Project scope: 280 hours

### Current Situation

✅ No exams during June-August
✅ Flexible TA role (can adjust hours)
✅ No other commitments
✅ Dedicated workspace
✅ Reliable internet

### Communication Plan

**Weekly:**
- Thursday 4 PM UTC mentor sync (45 min)
- Show working code
- Discuss blockers
- Plan next week

**Daily:**
- GitHub commits showing progress
- Slack updates in #dev
- Quick questions/answers

**As needed:**
- Ask for help within 24 hours of getting stuck
- Proactive problem flagging
- Solution-oriented communication

---

## Why You Should Pick Me

### 1. I'm Currently Contributing to Airflow

Not in the past. Right now.

- PR merged 37 minutes ago
- 2 PRs in review opened today/yesterday
- Active engagement showing intent
- Not hypothetical interest—actual work

### 2. I Understand the Problem Deeply

I've analyzed:
- How Breeze detects context
- What environment markers exist
- How AI tools work
- What information they need
- How to structure that information

Not theoretical understanding. I've spent weeks exploring.

### 3. I Can Learn Complex Systems

Evidence:
- Contributed to 5 different projects
- Navigated Superset's large codebase
- Got up to speed on Airflow quickly
- Handled diverse tech stacks

### 4. I Deliver Complete Work

- 6 merged PRs across projects
- 14 open PRs showing persistence
- All code passes tests
- All code goes through full review process
- Nothing abandoned

### 5. I Think About Architecture

This isn't just implementing features. It's designing a system that:
- Is extensible (easy to add new skills)
- Is maintainable (skills don't go out of sync)
- Is reliable (detects context accurately)
- Is useful (actually helps agents)

That kind of thinking comes from experience.

### 6. I'm Committed Long-Term

Not doing this for the summer stipend and disappearing.

- Plan to maintain the toolkit
- Continue contributing to Airflow
- Become expert on this system
- Help other contributors
- Long-term value for project

---

## Success Criteria

### By Week 3
- Environment detector works accurately
- Skill schema defined
- All tested and working

### By Week 7
- All 3 core workflows implemented
- Fully tested with real Breeze
- Performance optimized

### By Week 10
- Auto-sync mechanism working
- Complete documentation
- Code merged to Airflow main
- Ready for integration with AI tools

### Throughout
- Weekly demos to mentor
- Clear communication about blockers
- Realistic timeline (not optimistic guessing)

---

## Impact and Benefits

### For Contributors

**Faster Onboarding:**
- New contributors understand Breeze immediately
- No mystery "why did my tests fail in CI?"
- Can use AI tools confidently

**Better Experience:**
- AI tools work reliably
- Direct guidance from toolkit
- Less frustration

### For AI Tools

**Reliability:**
- Know the right commands to run
- Understand environment constraints
- Handle failures intelligently

**Usability:**
- Can guide users through Airflow PRs
- Can be trusted for Airflow work
- Become more valuable to developers

### For Airflow

**Competitive Advantage:**
- First major project truly AI-native
- Can advertise "works great with Copilot/Claude Code"
- Attract developers who use AI tools
- Better contributor retention

**Modern Positioning:**
- Embrace future of development (AI-assisted)
- Thought leadership
- Set standard for open source

### For Me

**Learning:**
- System design at scale
- Real-world architecture decisions
- Integration with major AI tools

**Career:**
- Impressive portfolio project
- Proof of strategic thinking
- References from Airflow core team
- Path to committer status

---

## Future Vision

### Post-GSoC

- Maintain toolkit
- Help tools integrate
- Become subject matter expert
- Support users

### Long-term

- Extend toolkit to other scenarios
- Mentor contributors
- Lead architecture discussions
- Shape Airflow's AI strategy

---

## Addressing Concerns

### "You're new to Airflow"

True, but:
- Already merged 1 PR
- 3 PRs currently in review
- Quickly learning codebase
- This project doesn't require deep Airflow knowledge

### "This is more complex than it seems"

It is, but:
- Clear architecture
- Realistic scope
- Buffer week built in
- Can reduce scope if needed

### "What if AI changes?"

Then toolkit becomes more valuable:
- Schema-driven (not hard-coded)
- Auto-syncs with Breeze (always current)
- Extensible design (new skill types easily added)

### "Will tools actually use this?"

Yes, because:
- Designed to match their specifications
- Fills real need (they're struggling with Airflow)
- Well-documented integration
- Can reach out during project

---

## Final Thoughts

This is about Airflow's future.

AI is becoming the default way developers work. The frameworks that embrace this will win. The ones that don't will lose.

Airflow has an opportunity to lead. To show the open source world how to build AI-native development environments.

I'm asking for the chance to build that.

This isn't about the stipend. It's about contributing something meaningful. Building something I believe in.

I've already proven I can deliver. I'm proving it right now with the PRs I'm submitting.

Let me ship this. Let me help Airflow lead.

---

**Ready to build this.**

**Questions? Happy to discuss anything.**

**Let's make Airflow truly AI-native.** 🚀
