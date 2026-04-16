---
name: search-strategy-auditor
description: >
  Meta-skill that audits your entire job search strategy — pipeline health, targeting
  accuracy, application velocity, rejection patterns, market alignment, and burnout risk.
  This is the "step back and look at the big picture" skill that turns raw application
  data into strategic insight. Trigger this skill whenever the user says "audit my search",
  "how is my job search going?", "what am I doing wrong?", "why am I not getting interviews?",
  "review my strategy", "am I applying to the right jobs?", "I keep getting rejected",
  "should I change my approach?", "job search check-in", "pipeline review", "search health
  check", "what should I do differently?", "I'm not making progress", "am I wasting my time?",
  "is my job search working?", "diagnose my job search", "why no callbacks?", "conversion
  rates", "application stats", "rejection analysis", or any variation of evaluating overall
  job search effectiveness. Also trigger when the user expresses frustration with their
  search, asks about application volume, or wants to understand patterns across multiple
  applications rather than analyzing a single one.
tags:
  - strategy
  - analytics
  - meta
  - pipeline
  - burnout
---

# Search Strategy Auditor

The big-picture diagnostic for your entire job search. Individual skills optimize single
applications — this skill optimizes the search itself. It finds the patterns you can't see
when you're in the middle of it: where your pipeline leaks, whether your targeting is off,
if you're burning out, and what the market is actually telling you.

Tone: honest and strategic. Like a career advisor who has seen 1,000 job searches and can
tell you in 30 seconds what you're doing wrong. Not harsh, but not sugarcoating. Every
observation is backed by data.

## Capabilities

| Capability | What It Does |
|---|---|
| **Pipeline Health Analysis** | Conversion rates at each stage vs. industry benchmarks |
| **Targeting Audit** | Match score distribution, role/company/seniority alignment |
| **Velocity Analysis** | Application pace, trends, follow-up timing |
| **Rejection Pattern Detection** | Cross-application gap mapping and root cause |
| **Market Reality Check** | Salary, demand, and market condition validation |
| **Strategy Recommendations** | Prioritized, effort-rated changes with expected impact |
| **Burnout Detection** | Velocity decay, rejection accumulation, engagement gaps |

---

## Prerequisites

1. Load career profile from persistent storage (key: `career-profile`) — specifically
   the `application_tracker` field.
2. If fewer than 3 applications tracked:
   - WARNING: Insufficient data for reliable pattern analysis.
   - Still offer targeting audit and velocity assessment using available data.
   - Skip pipeline conversion analysis and rejection pattern detection (sample too small).
3. If no career profile exists, hand off to `career-profile-builder`.
4. Scan `applications/` directory for all JD analyses and role-specific deliverables.

---

## Capability 1: Pipeline Health Analysis

Calculate conversion rates at each stage of the funnel. Load
`@references/audit-framework.md` for stage-specific diagnostics and benchmarks.

### Funnel Stages

```
Applications --> Screens --> Interviews --> Offers
```

### For Each Transition, Calculate and Report:

| Metric | Formula |
|---|---|
| Application to Screen rate | (screens / applications) x 100 |
| Screen to Interview rate | (interviews / screens) x 100 |
| Interview to Offer rate | (offers / interviews) x 100 |
| Overall conversion rate | (offers / applications) x 100 |

### Benchmark Comparison

| Transition | Healthy Range | Action Trigger |
|---|---|---|
| Application to Screen | 10-20% | Below 10% = resume or targeting issue |
| Screen to Interview | 40-60% | Below 40% = phone screen performance issue |
| Interview to Offer | 20-40% | Below 20% = interview performance issue |
| Overall (Application to Offer) | 2-8% | Below 2% = systemic issue across pipeline |

### Drop-Off Diagnosis

For each stage where conversion falls below benchmark:

1. Identify the exact drop-off point (e.g., "You convert 22% of applications to screens
   — that's healthy — but only 15% of screens to interviews. The leak is at the screen
   stage.")
2. Map to probable causes using the diagnostic framework in
   `@references/audit-framework.md`.
3. Recommend specific fixes tied to other skills in this system (e.g., "Run resume-tailor
   for each application" or "Practice with interview-coach mock interviews").

---

## Capability 2: Targeting Audit

Assess whether you are applying to the right roles. Present results as a Targeting
Health Score (0-100).

### Dimensions to Evaluate

**Match Score Distribution:**
- Pull match scores from all JD analyses in `applications/`.
- Average match score below 60% = WARNING: misaligned targeting. You are spending effort
  on roles where you are statistically unlikely to advance.
- Standard deviation above 20 = WARNING: inconsistent targeting. Your search lacks focus.

**Role Type Distribution:**
- Count distinct role titles/types across applications.
- All identical = WARNING: too narrow. If one niche dries up, you have no pipeline.
- 5+ distinct role types with no common thread = WARNING: too scattered. Hiring managers
  sense when someone is applying to everything.
- Sweet spot: 2-3 related role types with clear narrative connecting them.

**Company Stage Distribution:**
- Map each application to company stage: startup (seed/A), growth (B-D), late-stage/public,
  enterprise.
- Compare against profile preferences and experience history.
- Flag misalignment: "You have 8 years at large enterprises but 80% of your applications
  target Series A startups. Your resume signals won't resonate — reposition or refocus."

**Seniority Calibration:**
- Compare target role levels against profile experience.
- Applying 2+ levels above profile = WARNING: stretch applications are fine at 10-15% of
  pipeline, but above 50% wastes effort.
- Applying 2+ levels below profile = WARNING: you will be filtered as overqualified.
  Address this in cover letters or adjust targeting.

**Geographic / Remote Alignment:**
- Compare application locations against stated preferences.
- Flag contradictions: applying for on-site NYC roles with a "remote only, based in Austin"
  preference.

### Targeting Health Score Calculation

Start at 100. Deduct points:
- Average match score below 60%: -20
- Role type too narrow or too scattered: -15
- Company stage misalignment (>50% misaligned): -15
- Seniority miscalibration (>30% misaligned): -20
- Geographic conflict (>20% misaligned): -10
- Standard deviation of match scores above 20: -10

Report the score with specific adjustments:
"Targeting Health: 55/100. Main issues: match scores averaging 52% (-20), seniority
miscalibration on 40% of applications (-20), scattered role types (-15). Offset by
strong geographic alignment (+0 deduction)."

---

## Capability 3: Velocity Analysis

Track application pace and diagnose effort allocation problems.

### Metrics to Calculate

- **Applications per week** (rolling 4-week average and trend)
- **Time from JD discovery to application submission** (are you sitting on jobs too long?)
- **Time between stages** (application to screen callback, screen to interview scheduling)
- **Follow-up rate** (are you following up when you should be?)

### Velocity Benchmarks

| Pace | Classification | Diagnosis |
|---|---|---|
| <1 application/week | STALLING | Likely over-polishing, fear of rejection, or burnout. Perfection is the enemy of progress. A good-enough tailored application beats a perfect one submitted 2 weeks late. |
| 1-2 applications/week | SLOW | Acceptable only if targeting very senior roles or niche positions. Otherwise, increase output. |
| 3-7 applications/week | HEALTHY | Sweet spot. Enough volume for pipeline health, slow enough for meaningful tailoring. |
| 8-10 applications/week | HIGH | Sustainable only briefly. Check that tailoring quality isn't dropping. |
| >10 applications/week | SPRAYING | Almost certainly not tailoring. Conversion rates will crater. Slow down and focus on quality over quantity. |

### Trend Analysis

- Calculate week-over-week velocity change.
- Declining velocity for 3+ consecutive weeks = flag for burnout detection (see
  Capability 7).
- Sudden spike followed by crash = common boom-bust pattern. Recommend steady cadence.

### Follow-Up Timing

- No response after 1 week from application: normal, do nothing.
- No response after 2 weeks: send a follow-up if you have a contact.
- No response after screen/interview within 1 week: send a thank-you / check-in.
- Flag applications where follow-up is overdue.

---

## Capability 4: Rejection Pattern Detection

Load `@references/audit-framework.md` for the full pattern detection decision tree.

Map every rejection (or stall) to a gap type:

| Gap Type | Meaning | Source |
|---|---|---|
| **Skill gap** | You lack a required hard skill | JD analysis match scores, interview feedback |
| **Signal gap** | You have the skill but failed to communicate it | Resume review, interview debrief |
| **Fit gap** | Cultural, stage, or stylistic mismatch | Company research, interview feedback |
| **Process gap** | Timing, follow-up, or logistics failure | Application tracker timestamps |

### Pattern Analysis

After mapping all rejections:

**Dominant gap type is Signal:**
"You have the skills but aren't communicating them effectively. This is a positioning
problem, not a qualification problem. Focus on interview practice and resume narrative,
not more applications. Run interview-coach mock sessions and resume-tailor rewrites."

**Dominant gap type is Skill (concentrated):**
"The market is consistently telling you that {{SKILL_NAME}} is table stakes for your
target roles. You've been rejected for this gap in {{COUNT}} of {{TOTAL}} applications.
Consider: (a) close the gap — here's how, (b) reposition away from roles requiring it,
or (c) address it head-on in applications by showing adjacent experience."

**All rejections at the same stage:**
"You consistently drop off at the {{STAGE}} stage ({{COUNT}} of {{TOTAL}} rejections).
This is a {{STAGE}}-specific issue, not a general problem. The fix is targeted:
{{STAGE_SPECIFIC_FIX}}."

**Mixed patterns:**
Provide a breakdown by gap type with percentages and per-type recommendations. Example:
"40% skill gaps (concentrated in distributed systems), 35% signal gaps (your resume
undersells leadership experience), 25% fit gaps (you're applying to early-stage companies
but signaling enterprise). This needs a multi-pronged fix."

---

## Capability 5: Market Reality Check

Use `web_search` to validate whether your targets are realistic.

### Research Points

1. **Salary alignment:** Search levels.fyi, Glassdoor, and Blind for compensation data
   matching your target role + level + geography. Flag if your expectations are >20%
   above market median.
2. **Demand validation:** Search for job posting volume in your target role. Compare
   current month vs. 3 months ago vs. 6 months ago. Is demand growing or contracting?
3. **Skill demand:** Are your core skills in demand or declining? Check industry reports,
   Stack Overflow survey, LinkedIn skill trends.
4. **Market conditions:** Search for recent layoffs, hiring freezes, or industry shifts
   in your target sector. "You're targeting fintech, but the sector has seen 15,000
   layoffs in the past 3 months. Consider diversifying into adjacent sectors."
5. **Geographic factors:** If targeting specific locations, check cost-of-living vs.
   offered comp. Flag if remote roles are becoming scarcer in your target area.

### Output Format

Present as a Market Alignment Score with specific findings:
- ALIGNED: Your targets match market reality.
- STRETCH: Your targets are ambitious but achievable with strong positioning.
- MISALIGNED: Your targets conflict with current market conditions. Specific adjustments
  recommended.

---

## Capability 6: Strategy Recommendations

Synthesize all findings into a prioritized action plan. Maximum 5 recommendations to
avoid overwhelm.

### Recommendation Format

For each recommendation:

```
[PRIORITY: 1-5] [CATEGORY]
WHAT: Specific action to take
WHY: Data point from the audit that supports this
EXPECTED IMPACT: Quantified improvement estimate
EFFORT: Low / Medium / High
SKILL TO USE: Which skill in this system supports the action
```

### Categories

- **Targeting** — Change what you apply to
- **Positioning** — Change how you present yourself
- **Skill Development** — Close a gap the market is flagging
- **Process** — Fix timing, follow-up, or logistics
- **Mindset** — Address burnout, perfectionism, or motivation

### Prioritization Logic

1. Highest-impact, lowest-effort changes first.
2. Fix the biggest pipeline leak before optimizing smaller ones.
3. Never recommend more than 1 skill development item (takes too long for immediate ROI).
4. Always include at least 1 quick win (effort: Low) that can be done today.

---

## Capability 7: Burnout Detection

Load `@references/audit-framework.md` for burnout risk indicators and intervention
strategies.

### Indicators (flag if 2+ present)

- Application velocity declining for 3+ consecutive weeks
- Rejections accumulating (3+ in a row) without any strategy change
- Gaps of 10+ days between applications with no stated reason
- Match scores dropping (applying to increasingly misaligned roles out of desperation)
- Follow-up rate dropping (stopping engagement with active applications)

### Response Protocol

If burnout indicators detected:

1. **Acknowledge it.** "Your data shows signs of search fatigue. This is normal — job
   searching is one of the most psychologically demanding professional activities. It
   does not mean something is wrong with you."

2. **Quantify the pattern.** "Your weekly velocity has dropped from 5 to 1 over the
   past month, and your last 4 applications had match scores below 50% — down from your
   earlier average of 72%. This suggests you're applying less carefully, which is a
   classic burnout signal."

3. **Offer concrete next steps (not just "take a break"):**
   - Reduce target to 2 high-quality applications this week. Quality compounds.
   - Revisit your top 3 strongest applications and check for follow-up opportunities.
   - Run interview-coach on your most promising active application to re-engage.
   - Set a specific "search hours" window (e.g., 9-11am weekdays only) to prevent the
     search from consuming all mental bandwidth.
   - If 50+ applications with <2% conversion, seriously consider a strategy reset: new
     targeting, new positioning, or a skill-building pause.

---

## Periodic Check Trigger

Recommend running this audit:
- After every 5th application submitted
- After 2 weeks of active searching
- After 3 rejections without an interview
- When the user expresses frustration or confusion about their search

The coach agent should proactively suggest this skill when these thresholds are met.

---

## Output

### Save Location

Save the full report to `applications/search-audit-{{DATE}}.md`.

### Report Structure

```
# Search Strategy Audit — {{DATE}}

## Executive Summary
[2-3 sentence overview of search health]

## Pipeline Health
[Funnel metrics, benchmarks, drop-off diagnosis]

## Targeting Audit
[Targeting health score with dimension breakdown]

## Velocity Analysis
[Pace classification, trends, follow-up status]

## Rejection Patterns
[Gap type mapping, dominant pattern, root cause]

## Market Reality Check
[Alignment score, salary data, demand trends, conditions]

## Strategy Recommendations
[Top 5 prioritized recommendations in standard format]

## Burnout Check
[Risk level: LOW / MODERATE / HIGH, indicators, next steps if needed]

---

## Top 3 Changes to Make This Week
1. [Most impactful, actionable change]
2. [Second priority]
3. [Third priority]

Each item: what to do, why it matters, expected result.
```

### Closing

End every audit with the "Top 3 Changes to Make This Week" section. These must be:
- Actionable within 7 days
- Specific (not "apply to more jobs" but "submit 3 applications to Series B-D companies
  with match scores above 70%, using resume-tailor for each")
- Tied to a data point from the audit

---

## Data Flow

### Reads
- `career-profile` (application_tracker, preferences, skills, experience)
- All JD analyses in `applications/*/`
- Interview debrief notes (if any)
- Previous audit reports (to track trends across audits)

### Writes
- Audit report: `applications/search-audit-{{DATE}}.md`
- Optionally updates `career-profile` preferences if you agree to targeting changes
