# Job Search Agent

End-to-end AI career coaching for Claude Code. Profile building, JD analysis, resume tailoring, interview prep, recruiter coaching, and offer negotiation.

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

## What's Inside

### Skills (4)

| Skill | What it does |
|:------|:-------------|
| **career-profile-builder** | Interrogate experiences or parse uploaded resume into structured profile |
| **job-analyzer** | Match scoring, JD decoding, gap analysis, networking intelligence |
| **resume-tailor** | ATS-optimized resume with 70%+ keyword coverage and LinkedIn sync |
| **interview-coach** | Company research, tailored prep, mock interviews, recruiter coaching, rejection diagnosis, offer negotiation |

### Commands (3)

| Command | Purpose |
|:--------|:--------|
| **/quick-start** | Guided entry point — asks your situation, routes to right skill |
| **/help** | Shows everything available |
| **/status** | Profile completeness and application tracker |

### Agent (1)

| Agent | Purpose |
|:------|:--------|
| **coach** | Orchestrates skills based on your situation with checkpoints between each |

## Pipeline

```
1. Build profile        → /career-profile-builder
2. Paste a JD           → /job-analyzer (match score, gaps, red flags)
3. Tailor resume        → /resume-tailor (ATS-optimized, 70%+ keyword coverage)
4. Apply                → (you do this)
5. Research + prep      → /interview-coach (deep company research + tailored questions)
6. Practice             → /interview-coach (mock with difficulty modes)
7. After interview      → /interview-coach (debrief, thank-you, rejection diagnosis)
8. Negotiate offer      → /interview-coach (evaluation + counter-offer scripts)
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
│   └── interview-coach/
│       ├── SKILL.md
│       └── references/
│           ├── interviewer-perspective.md
│           ├── negotiation-scripts.md
│           ├── post-interview-coaching.md
│           ├── strategic-questions.md
│           └── deep-research-validation.md
├── commands/
│   ├── quick-start.md
│   ├── help.md
│   └── status.md
└── agents/
    └── coach.md
```

## Inspired By

- [career-helper](https://github.com/Zal4DW/career-helper) — Orchestrator pattern, deep research validation, interviewer perspective reports, mock interview difficulty modes
- [career-ops](https://github.com/santifer/career-ops) — STAR story accumulation, auto-pipeline from JD paste
- [Job Search OS](https://www.news.aakashg.com/p/job-search-os) — Surgical targeting philosophy

## License

MIT
