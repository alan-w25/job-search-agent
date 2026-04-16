---
name: skills-gap-closer
description: >
  Turn gap analysis from job-analyzer into actionable development plans with prioritized
  learning paths, portfolio projects, and certification recommendations. While other skills
  diagnose gaps and work around them, this skill helps you actually close them.
  Trigger this skill whenever the user says "close my skill gaps", "how do I learn this?",
  "make a development plan", "what should I study?", "upskill for this role",
  "learn [technology]", "what certifications should I get?", "build my portfolio",
  "how do I become qualified?", "close the gap", "skill development plan",
  "what's the fastest way to learn X?", "how do I get from here to there?",
  "what projects should I build?", "is this certification worth it?",
  "what should I learn next?", "help me become a stronger candidate",
  or any variation of wanting to improve skills to match target roles.
  Also trigger when the user has just reviewed a gap analysis from job-analyzer
  and wants to act on it rather than work around it.
tags:
  - development
  - learning
  - upskilling
  - portfolio
  - certifications
  - career-growth
---

# Skills Gap Closer

Turn diagnosis into action. This skill takes the gaps identified by job-analyzer and
builds concrete, time-boxed plans to close them — prioritized by which gaps block the
most opportunities, with the fastest credible paths to each skill level.

The philosophy: minimum viable skill. What is the least you need to do to credibly
claim a skill? Not academic completionism, but practical, interview-ready competence.

## Prerequisites

Before building any development plan:

1. Load the career profile from persistent storage (key: `career-profile`)
2. Check for JD analyses in the `applications/` folder — the more analyzed JDs, the
   better the prioritization (cross-referencing gaps across multiple target roles)
3. If no career profile exists -> tell the user: "I need your career profile to build
   an effective plan. Want to create one now?" and hand off to `career-profile-builder`
4. If no JD analyses exist -> the skill can still work from user-described gaps, but
   warn: "I'll build a plan based on what you've told me. For better prioritization,
   analyze a few target JDs first with job-analyzer so I can see which gaps matter most."

## Step 1: Gap Prioritization

Cross-reference gaps across ALL analyzed JDs to find which gaps block the most
opportunities.

### Scoring Framework

Score each gap on three axes (1-5 each):

| Axis | 1 | 3 | 5 |
|------|---|---|---|
| **Frequency** | Appears in 1 target JD | Appears in ~50% of target JDs | Appears in nearly every target JD |
| **Severity** | Listed as nice-to-have | Listed as preferred | Listed as must-have / dealbreaker |
| **Closability** | Requires 3+ months to credibly claim | Requires 2-4 weeks | Can credibly claim within 1 week |

**Priority Score = Frequency x Severity x Closability**

Maximum possible score: 125. Minimum: 1.

### Priority Tiers

- **Tier 1 (Score 60-125):** Close these first. High frequency, high severity, and
  achievable. These are the gaps that, if closed, unlock the most opportunities.
- **Tier 2 (Score 25-59):** Important but either less common, less critical, or harder
  to close quickly. Work on these in parallel with Tier 1 or after Tier 1 is done.
- **Tier 3 (Score 1-24):** Nice-to-haves or long-term investments. Don't let these
  distract from Tier 1 and 2.

### Output Format

Present as a ranked table:

```
GAP PRIORITIZATION (based on [N] analyzed JDs)

Rank | Skill Gap        | Freq | Severity | Closability | Score | Tier
-----|------------------|------|----------|-------------|-------|------
  1  | Kubernetes       |   5  |    4     |      3      |   60  |  1
  2  | Terraform        |   4  |    4     |      4      |   64  |  1
  3  | GraphQL          |   3  |    3     |      5      |   45  |  2
  4  | System Design    |   5  |    5     |      1      |   25  |  2
  ...
```

Follow the table with a plain-language summary: "If you close the Kubernetes and
Terraform gaps, you become qualified for [X] additional roles out of the [Y] you've
analyzed. That is the highest-leverage move right now."

## Step 2: Learning Path Generation

Read `@references/learning-path-framework.md` for templates, time benchmarks,
and resource recommendations.

For each Tier 1 and Tier 2 gap, generate a specific learning path with three levels:

### Quick Win (1-7 days)

The goal: go from "I haven't used this" to "I can speak to it credibly and have
touched it hands-on."

- Complete a focused tutorial or official quickstart
- Build a small demo project (under 4 hours of work)
- Complete a micro-certification (LinkedIn Learning certificate, Coursera module)
- Contribute a small fix to an open-source project using the technology
- Do 5-10 practice problems if it is an algorithmic/query skill

After this level, you can say: "I've worked with [skill] — here's what I built..."

### Working Knowledge (2-4 weeks)

The goal: discuss the technology intelligently in interviews, answer follow-up
questions, and compare trade-offs.

- Complete a structured course or bootcamp module
- Build a portfolio project (see Step 3: Portfolio Project Design)
- Write a blog post or tutorial about what you learned (forces deeper understanding
  and creates a public artifact)
- Read the official documentation's architecture/concepts section
- Understand common pitfalls and when NOT to use the technology

After this level, you can say: "I've built [project] with [skill] and understand
the trade-offs between [X] and [Y]..."

### Proficient (1-3 months)

The goal: list as "proficient" on your resume without exaggeration.

- Build or extend a project with production-like complexity (error handling, testing,
  monitoring, deployment)
- Earn a recognized certification (if ROI-positive — see Step 4)
- Contribute meaningfully to open source (not just typo fixes)
- Solve complex problems or edge cases in the technology
- Be able to mentor someone at the Quick Win level

After this level, you can say: "I've used [skill] in a production-like context and
can design solutions around it..."

### Path Specifics

Each learning path MUST include:

1. **Specific resources**: Named courses, documentation URLs, book titles — not
   "find a tutorial online"
2. **Estimated time commitment**: Total hours at each level (e.g., "Quick Win: ~8 hours
   over 3 days")
3. **Verification checkpoint**: How you know you have reached the level
   - Quick Win: "Can you explain what [skill] does and show something you built?"
   - Working Knowledge: "Can you compare [skill] to alternatives and explain trade-offs?"
   - Proficient: "Can you debug a non-trivial issue and design a solution from scratch?"
4. **Resume language**: Exactly how to describe your level on a resume at each stage

## Step 3: Portfolio Project Design

For each Tier 1 gap, design a portfolio project that demonstrates the skill in an
interview-ready way.

### Project Requirements

Every portfolio project must be:

- **Completable in 1-2 weeks** of part-time work (10-20 hours)
- **Demonstrable in 3 minutes** during an interview ("Let me show you what I built...")
- **Realistic**: Uses the technology the way a production team would, not toy examples
- **Relevant**: Connected to your target domain when possible
- **STAR-ready**: When complete, you can describe it as a Situation-Task-Action-Result story

### Project Brief Structure

For each project, provide:

```
PROJECT: [Descriptive Name]
TARGET GAP: [The skill this project demonstrates]
TIME ESTIMATE: [X] hours over [Y] days

PROBLEM STATEMENT:
[What real-world problem does this solve? 2-3 sentences.]

TECH STACK:
- [Primary technology — the gap skill]
- [Supporting technologies from your existing skills]

SCOPE — MVP (must complete):
- [ ] [Feature 1 — core demonstration of the skill]
- [ ] [Feature 2 — shows understanding beyond hello-world]
- [ ] [Feature 3 — demonstrates production awareness]

STRETCH GOALS (if time permits):
- [ ] [Stretch 1 — shows deeper proficiency]
- [ ] [Stretch 2 — impressive but not required]

INTERVIEW TALKING POINTS:
- "I built this to solve [problem]. The key technical decision was [X] because [Y]."
- "The most interesting challenge was [Z]. I solved it by..."
- "If I were to extend this, I'd add [feature] because [reason]."

STAR STORY:
- Situation: [Context]
- Task: [What you set out to do]
- Action: [Technical decisions and implementation]
- Result: [Outcome, metrics if applicable]
```

### Project Examples by Skill Type

When designing projects, match the pattern to the skill category:

**Missing cloud/infra experience:**
Deploy an existing application (or build a simple one) on the target cloud platform
with CI/CD pipeline, infrastructure-as-code, monitoring, and auto-scaling. This hits
multiple cloud skills in one project.

**Missing data engineering experience:**
Build a data pipeline that ingests real-world data (public APIs, open datasets),
transforms it, loads it into a warehouse, and produces a dashboard or report.
Shows end-to-end understanding.

**Missing ML/AI experience:**
Build a prediction model using real data from your target domain. Deploy it as an
API endpoint. Focus on the full lifecycle: data prep, model selection, evaluation,
deployment — not just model accuracy.

**Missing frontend experience:**
Create a dashboard or interactive tool for data from your domain. Shows you can
build user-facing applications, not just backend systems.

**Missing backend/API experience:**
Build a REST or GraphQL API for a realistic domain problem with authentication,
validation, error handling, and tests. Deploy it somewhere accessible.

**Missing DevOps experience:**
Containerize an application, set up CI/CD with GitHub Actions or similar, implement
infrastructure-as-code, add monitoring and alerting. The "production readiness" project.

## Step 4: Certification ROI Analysis

For each certification relevant to your gaps, assess whether it is worth the investment.

### ROI Assessment Framework

Evaluate each cert on:

| Factor | Assessment |
|--------|-----------|
| **JD Frequency** | How many of your target JDs mention this cert? (count and %) |
| **Cost** | Exam fee + study materials + prep course if needed |
| **Time Investment** | Hours of study + exam time |
| **Industry Credibility** | Is it respected by hiring managers, or is it resume padding? |
| **Shelf Life** | How long before it expires or becomes irrelevant? |
| **Alternative Signals** | Could a portfolio project signal the same competence? |

### ROI Tiers

**HIGH ROI** — Pursue if you have the bandwidth
- Appears in 50%+ of your target JDs
- Recognized industry-wide by hiring managers (not just recruiters)
- Costs under $500 and under 40 hours of focused prep
- Remains valid for 2+ years
- Examples: AWS Solutions Architect Associate, Google Cloud Professional, Kubernetes CKA

**MEDIUM ROI** — Consider if it aligns with your timeline
- Appears in 20-50% of target JDs
- Some recognition, may help pass recruiter screens
- Reasonable cost relative to your budget
- Examples: Terraform Associate, dbt Analytics Engineering, Snowflake SnowPro Core

**LOW ROI** — Skip unless you need the structured learning
- Rarely mentioned in your target JDs (<20%)
- So common it does not differentiate you
- Value is mainly in the learning process, not the credential itself
- Examples: Generic "completion certificates" from MOOCs, vendor-neutral basics

**NEGATIVE ROI** — Avoid
- Costs significant time or money but does not move the needle
- Outdated or from unrecognized issuers
- Time spent here is time NOT spent on higher-impact activities
- Examples: Expensive bootcamp certificates with no industry recognition,
  certs for technologies not in your target JDs

### Output Format

```
CERTIFICATION ROI ANALYSIS

Cert                          | JD Freq | Cost   | Hours | Credibility | ROI Tier
------------------------------|---------|--------|-------|-------------|----------
AWS Solutions Architect Assoc | 7/10   | $300   |  40   | High        | HIGH
Terraform Associate           | 4/10   | $70    |  20   | Medium      | HIGH
Snowflake SnowPro Core        | 3/10   | $175   |  30   | Medium      | MEDIUM
PMP                           | 1/10   | $555   |  80   | Low (for eng)| NEGATIVE

RECOMMENDATION: Pursue AWS SA Associate first — it appears in 70% of your target
JDs and is the single most recognized cloud certification. Skip PMP — only 1 of
your 10 target roles mentions it, and your time is better spent on technical skills.
```

## Step 5: Quick Win Identification

Identify gaps that can be credibly closed in DAYS, not weeks. These are the
highest-closability items — skills adjacent to what you already know.

### Adjacent Skill Pattern

The key insight: many "gaps" are actually skills you effectively have but have not
explicitly claimed. Quick wins make existing adjacent skills explicitly claimable.

Scan the user's profile for these patterns:

**Language/Framework Adjacency:**
- You know Python but have not listed pandas -> Do one Kaggle notebook this weekend
- You know JavaScript but have not listed TypeScript -> Convert a small project,
  takes ~4 hours
- You know React but have not listed Next.js -> Build one page with Next.js, ~6 hours
- You know Java but have not listed Kotlin -> Rewrite one class, Kotlin is designed
  for Java developers

**Tool/Platform Adjacency:**
- You have used Docker but never listed Kubernetes -> Do the official k8s tutorial
  and deploy your existing containerized project, ~8 hours
- You have SQL experience but have not mentioned window functions -> Practice 10
  problems on LeetCode or HackerRank, ~3 hours
- You have used GitHub but not GitHub Actions -> Add a CI pipeline to an existing
  repo, ~2 hours
- You have used REST APIs but not GraphQL -> Build one GraphQL endpoint wrapping
  an existing REST service, ~4 hours

**Methodology/Process Adjacency:**
- You manage projects but do not have Agile on your profile -> Take the 2-hour
  LinkedIn Learning course and update your profile
- You write tests but have not listed TDD -> Describe your existing practice
  using TDD terminology
- You do code reviews but have not listed "mentoring" -> Reframe what you already do
- You have shipped features but have not listed A/B testing -> If you have used
  feature flags, that is A/B testing adjacent

**Credential Quick Wins:**
- LinkedIn Skill Assessments -> 15 minutes each, adds a badge to your profile
- Cloud provider free-tier certifications -> Some vendors offer free foundational certs
- Open-source contributions -> Even documentation PRs count and show up on GitHub

### Output Format

```
QUICK WINS (closable this week)

Gap              | You Already Have   | Action                         | Time  | Impact
-----------------|--------------------|--------------------------------|-------|--------
pandas           | Python (proficient)| Complete 1 Kaggle notebook     | 3 hrs | +2 JD matches
Kubernetes       | Docker (working)   | k8s tutorial + deploy project  | 8 hrs | +3 JD matches
TypeScript       | JavaScript (prof.) | Convert small project to TS    | 4 hrs | +2 JD matches
Agile/Scrum      | Project management | LinkedIn Learning course       | 2 hrs | +1 JD match
Window Functions | SQL (proficient)   | 10 practice problems           | 3 hrs | +1 JD match
```

Present these with urgency: "You could credibly claim [X] additional skills by
the end of this week with about [Y] total hours of focused work. That would
improve your match scores across [Z] target roles."

## Step 6: Progress Tracking

Track skill development over time and show concrete improvement in match scores.

### Profile Updates

When the user completes a learning milestone, update their career profile:

- **Quick Win completed**: Move skill from unlisted to "familiar" or from "familiar"
  to "working_knowledge"
- **Working Knowledge reached**: Move skill to "working_knowledge", add portfolio
  project to projects section
- **Proficient reached**: Move skill to "proficient", add certification to
  certifications section if applicable

Always confirm with the user before updating: "You've completed the Kubernetes
quick win path. Want me to update your profile to list Kubernetes at the
'familiar' level and add the deployment project to your portfolio?"

### Match Score Re-evaluation

After profile updates, re-score against all saved JD analyses:

```
PROGRESS REPORT

Skill Closed: Kubernetes (familiar -> working_knowledge)
Time Invested: 22 hours over 2 weeks

Impact on Match Scores:
  Role                              | Before | After | Change
  ----------------------------------|--------|-------|--------
  Senior Backend Engineer, Stripe   |   62%  |  74%  |  +12%
  Platform Engineer, Datadog        |   58%  |  71%  |  +13%
  SRE, Cloudflare                   |   55%  |  68%  |  +13%
  DevOps Engineer, HashiCorp        |   48%  |  65%  |  +17%

  Average improvement: +13.75% across 4 target roles
  New roles now in "Good Match" range: 2 (Stripe, Datadog)
  ROI: 22 hours invested -> 2 additional viable applications
```

### Cumulative Tracking

Maintain a running summary:

```
SKILLS DEVELOPMENT SUMMARY

Total time invested: [X] hours over [Y] weeks
Skills closed: [list]
Match score improvement: [before average] -> [after average] (+[delta]%)
Additional roles now in range: [N]
Certifications earned: [list]
Portfolio projects completed: [list]
```

## Output

### Development Plan

Save the complete development plan to:
`applications/skills-development-plan-{{DATE}}.md`

Structure:
1. Gap prioritization table
2. Tier 1 learning paths with timelines
3. Portfolio project briefs
4. Certification recommendations
5. Quick wins list
6. Weekly schedule recommendation

### Portfolio Project Briefs

Save individual project briefs to:
`applications/portfolio-projects/{{PROJECT_SLUG}}.md`

### Suggested Timeline

Present a realistic week-by-week plan:

```
SUGGESTED TIMELINE

Week 1: Quick Wins
  - [Quick win 1] (X hours)
  - [Quick win 2] (X hours)
  - Start [Tier 1 learning path] (X hours)

Week 2-3: Tier 1 Gap — [Skill Name]
  - Complete [course/tutorial]
  - Build [portfolio project] MVP
  - [Specific milestones]

Week 4: Tier 1 Gap — [Skill Name] (continued)
  - Complete portfolio project
  - Write blog post / documentation
  - Update profile, re-score matches

...
```

## Data Flow

| Direction | Key / Path | Description |
|-----------|-----------|-------------|
| Reads | `career-profile` | Current skills, experience, application tracker |
| Reads | `applications/*/jd-analysis.md` | All JD analyses for cross-referencing gaps |
| Writes | `applications/skills-development-plan-{{DATE}}.md` | Full development plan |
| Writes | `applications/portfolio-projects/{{PROJECT_SLUG}}.md` | Individual project briefs |
| Writes | `career-profile` (with approval) | Updated skills, projects, certifications |

## Tone

Practical and motivating. Like a mentor who has been where you are and knows the
fastest path forward. Focus on "minimum viable skill" — what is the least you need
to do to credibly claim this? Not academic completionism, but strategic investment
of your limited time.

Be honest about timelines. Do not promise "learn Kubernetes in a weekend" if that
is not realistic. But also do not let perfectionism stall progress — "familiar" is
a valid and useful skill level, and it is infinitely better than "not listed."

Every recommendation should pass the test: "If you follow this plan, will a hiring
manager find your claim credible in an interview?" If yes, ship it. If no, the plan
needs more depth.
