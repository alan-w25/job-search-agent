# Job Search Agent

End-to-end AI career coaching for Claude Code. Profile building, JD analysis, resume tailoring, cover letters, interview prep, networking strategy, skills gap closing, and search strategy auditing.

## Install

```bash
claude plugin marketplace add alan-w25/job-search-agent
claude plugin install job-search-agent@job-search-agent
```

Or clone manually:
```bash
git clone https://github.com/alan-w25/job-search-agent.git ~/.claude/plugins/job-search-agent
```

## Get Started

```
/job-search-agent:quick-start    → Guided questions to route you
/job-search-agent:help           → Full overview of everything available
/job-search-agent:status         → Profile completeness and application tracker
```

Or just describe your situation:
- "I want to apply for a Senior Data Engineer role at Stripe"
- "Help me prepare for an interview next Tuesday"
- "I got an offer — should I negotiate?"
- "Analyze this job description" (paste JD)
- "Write a cover letter for this role"
- "Who should I reach out to at this company?"
- "How do I close the Kubernetes gap?"
- "My job search isn't working — what am I doing wrong?"

## What's Inside

### Skills (8)

| Skill | What it does |
|:------|:-------------|
| **career-profile-builder** | Interrogate experiences or parse uploaded resume into structured profile. Achievement extraction heuristics for stuck users. Skill level calibration to catch over/under-reporting. |
| **job-analyzer** | Match scoring with company stage adjustment, JD decoding, JD freshness analysis, gap analysis, networking intelligence |
| **resume-tailor** | ATS-optimized resume with 70%+ keyword coverage, achievement triage scoring, over-qualification strategy, and LinkedIn sync |
| **cover-letter-writer** | Targeted cover letters with tone calibration by company culture, anti-pattern detection, and ATS-friendly formatting |
| **interview-coach** | Company research, tailored prep, mock interviews with performance tracking and plateau detection, recruiter coaching, rejection diagnosis with story resonance logging, offer negotiation |
| **networking-strategist** | Target company mapping, outreach messages, informational interview prep, referral scripting, LinkedIn optimization, warm intro chain mapping |
| **skills-gap-closer** | Gap prioritization across JDs, learning paths, portfolio project design, certification ROI analysis, quick wins, progress tracking with match score re-evaluation |
| **search-strategy-auditor** | Pipeline health analysis, targeting audit, velocity tracking, rejection pattern detection, market reality check, burnout detection |

### Commands (3)

| Command | Purpose |
|:--------|:--------|
| **/quick-start** | Guided entry point — asks your situation, routes to right skill |
| **/help** | Shows everything available |
| **/status** | Profile completeness and application tracker |

### Agent (1)

| Agent | Purpose |
|:------|:--------|
| **coach** | Orchestrates skills based on your situation with checkpoints between each step. Proactively suggests search audits, networking, and gap closing. |

## Pipeline

```
 1. Build profile        → /career-profile-builder
 2. Paste a JD           → /job-analyzer (match score, gaps, red flags)
 3. Close skill gaps     → /skills-gap-closer (if gaps identified)
 4. Tailor resume        → /resume-tailor (ATS-optimized, 70%+ keyword coverage)
 5. Write cover letter   → /cover-letter-writer (targeted, culture-calibrated)
 6. Network in           → /networking-strategist (warm intros, referrals)
 7. Apply                → (you do this)
 8. Research + prep      → /interview-coach (deep company research + tailored questions)
 9. Practice             → /interview-coach (mock with difficulty modes + tracking)
10. After interview      → /interview-coach (debrief, thank-you, rejection diagnosis)
11. Negotiate offer      → /interview-coach (evaluation + counter-offer scripts)
12. Audit your search    → /search-strategy-auditor (periodically)
```

## Structure

```
job-search-agent/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── career-profile-builder/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── profile-schema.md
│   ├── job-analyzer/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── jd-analysis-framework.md
│   │       └── deep-research-validation.md
│   ├── resume-tailor/
│   │   └── SKILL.md
│   ├── cover-letter-writer/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── cover-letter-patterns.md
│   ├── interview-coach/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── interviewer-perspective.md
│   │       ├── negotiation-scripts.md
│   │       ├── post-interview-coaching.md
│   │       ├── strategic-questions.md
│   │       └── deep-research-validation.md
│   ├── networking-strategist/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── outreach-templates.md
│   │       └── linkedin-optimization.md
│   ├── skills-gap-closer/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── learning-path-framework.md
│   └── search-strategy-auditor/
│       ├── SKILL.md
│       └── references/
│           └── audit-framework.md
├── commands/
│   ├── quick-start.md
│   ├── help.md
│   └── status.md
└── agents/
    └── coach.md
```

## What's New in v2.0

**4 new skills:**
- **cover-letter-writer** — targeted cover letters with company culture tone calibration
- **networking-strategist** — the hidden job market is 70%+ of roles; this skill helps you access it
- **skills-gap-closer** — stop working around gaps, start closing them
- **search-strategy-auditor** — meta-analysis of your entire job search strategy

**Deep reasoning improvements to all existing skills:**
- **career-profile-builder** — achievement extraction probes for users who undersell, skill level calibration to catch Dunning-Kruger in both directions
- **job-analyzer** — company stage modifiers (startup vs. enterprise scoring), JD freshness analysis (stale postings, repost patterns)
- **resume-tailor** — achievement triage scoring (what to cut when over page limit), over-qualification strategy (downleveling positioning)
- **interview-coach** — mock performance tracking with plateau detection, story resonance logging across real interviews

## Inspired By

- [career-helper](https://github.com/Zal4DW/career-helper) — Orchestrator pattern, deep research validation, interviewer perspective reports, mock interview difficulty modes
- [career-ops](https://github.com/santifer/career-ops) — STAR story accumulation, auto-pipeline from JD paste
- [Job Search OS](https://www.news.aakashg.com/p/job-search-os) — Surgical targeting philosophy

## License

MIT
