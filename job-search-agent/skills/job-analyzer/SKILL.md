---
name: job-analyzer
description: >
  Analyze job descriptions to produce a detailed match score against your career profile,
  identify skill gaps, decode hidden signals in JD language, and determine if a role is
  worth pursuing. Use this skill whenever the user pastes a job description, shares a job
  posting URL, says "am I qualified for this?", "should I apply?", "what are my chances?",
  "analyze this JD", "how well do I match?", "rate this role", "is this a good fit?",
  "score this job", "what do you think of this role?", or any variation of evaluating a
  job opportunity. Also trigger when the user wants to compare multiple jobs side by side,
  decode what a JD is really saying, or understand the hidden meaning behind corporate
  job listing language. If someone pastes a wall of text that looks like a job description,
  trigger this skill even if they don't explicitly ask for analysis.
---

# Job Analyzer

Parse job descriptions with precision, score match against the user's career profile,
and surface insights that generic AI analysis misses — especially the hidden signals
baked into JD language that reveal what a company actually wants vs. what they wrote down.

## Prerequisites

Before analyzing any JD:

1. Load the career profile from persistent storage (key: `career-profile`)
2. If no profile exists → tell the user: "I need your career profile first to give you
   an accurate match. Want to build one now?" and hand off to `career-profile-builder`
3. If profile completeness is below 40% → warn that match scoring may be unreliable
   and suggest fleshing out the profile after this analysis

## Step 1: JD Ingestion

Accept job descriptions via:
- **Pasted text**: User drops the JD into chat — the most common path
- **URL**: User shares a job posting link — use web_fetch to extract content
- **File**: User uploads a PDF/screenshot — read the content

Parse into a structured analysis object. Read `references/jd-analysis-framework.md`
for the full parsing template.

## Step 2: Decode the JD — Reading Between the Lines

This is the critical differentiator. Generic AI just matches keywords. This skill
decodes what companies actually mean, informed by real hiring patterns.

### Deep Research Validation

When analyzing the company behind the JD, use the multi-cycle research validation
loop (read `../references/deep-research-validation.md`). Don't just take the JD
at face value — research the company to understand the real context behind the
posting. A 5-minute web search can reveal:
- The role has been posted for 90 days (red flag)
- The company just had layoffs (context changes everything)
- The hiring manager posted on LinkedIn about their team's biggest challenge
  (interview gold)
- Glassdoor reviews mention the team this role is on specifically

### Hidden Signal Detection

Scan the JD for these patterns and flag them with explanations:

**Red Flags (proceed with caution):**
- Unrealistic requirement combos ("10 years React" — React is ~11 years old)
- Title/responsibility mismatch (VP title but IC responsibilities, or vice versa)
- "Wear many hats" + low salary range = understaffed, you'll burn out
- "Fast-paced" appearing 3+ times = chaotic, no processes
- "Competitive salary" with no range = they're hoping you'll lowball yourself
- Role has been posted for 60+ days = something is wrong (internal politics, budget)
- "Rockstar/ninja/10x" = immature engineering culture, likely no work-life balance
- No mention of manager or team structure = the org might not know what it wants
- "Other duties as assigned" is a large portion = scope creep guaranteed

**Green Flags (good signs):**
- Specific tech stack with versions = they know what they need, team is technical
- Mentions mentorship or growth = they invest in people
- Clear success metrics ("in 90 days you'll...") = they've thought about the role
- Team size and structure mentioned = organizational maturity
- Salary range posted = transparency, likely better culture
- "We're building X because Y" = they can articulate why the role exists

**Negotiation Intel (for later use by interview-coach):**
- "Competitive compensation" = room to negotiate, no firm ceiling
- Explicit salary bands = less room but more transparency
- "Equity/RSU" mentioned = negotiate on vesting schedule and refresh
- Title flexibility language ("Senior or Staff depending on experience") = can level up
- Multiple locations listed with different comp = geo-arbitrage opportunity

Save decoded signals to the analysis for the interview-coach skill to reference later.

## Step 3: Match Scoring

Score the match on a 0-100 scale using weighted categories:

| Category | Weight | How to Score |
|----------|--------|-------------|
| **Hard Skills Match** | 30% | % of must-have skills at proficient+ level in profile |
| **Experience Level** | 20% | Years + seniority alignment (±2 years is acceptable) |
| **Domain Knowledge** | 15% | Industry/vertical overlap from work history |
| **Responsibility Fit** | 15% | How closely past work maps to listed responsibilities |
| **Soft Skills / Culture** | 10% | JD culture signals vs. user preferences |
| **Education / Creds** | 10% | Degree/certification requirements met |

### Score Interpretation

Present the score with a clear recommendation:

- **85-100 — Strong Match**: "You check nearly every box. Apply with confidence. Focus
  your resume on direct experience overlap."
- **70-84 — Good Match**: "Solid fit with a few gaps. Apply and address gaps proactively
  in your cover letter. Here's how to bridge each one..."
- **55-69 — Stretch Role**: "This is a reach but doable if you're excited about it.
  You'll need to lean hard on transferable skills. Worth it if the company/role excites you."
- **40-54 — Significant Gaps**: "Major skill or experience gaps. Consider only if you have
  a strong referral or a compelling 'why me' angle."
- **Below 40 — Not a Fit**: "This isn't your strongest shot. But check if the JD is
  inflated — if the 'must-haves' feel aspirational, the real bar may be lower."

### Gap Analysis

For each gap between the JD and the profile, provide:

1. **The gap**: What they want that you don't have
2. **Severity**: Must-have (critical) vs. nice-to-have (minor)
3. **Bridge strategy**: How to position around it
   - Adjacent skill? ("Haven't used Snowflake, but I've built 3 data warehouses in BigQuery")
   - Rapid learning evidence? ("Picked up Terraform in 2 weeks for a migration project")
   - Overqualified in a compensating area? ("Less Kubernetes, but I designed the CI/CD
     pipeline that 40 engineers use daily")
4. **Talking point**: A 1-2 sentence positioning statement for the resume or interview

## Step 4: Competitive Positioning

Beyond match scoring, tell the user how they likely stack up:

- **Your Strongest Angles**: The 3-4 areas where you exceed what they're asking for
- **Your Differentiators**: Things in your profile that most candidates won't have
- **Their Likely Candidate Pool**: Based on requirements, who else is probably applying?
  ("This is a senior ML engineer role at a mid-stage startup — you're competing with
  people who have 5-8 years of ML experience, likely coming from FAANG or other startups")
- **Your Win Condition**: The specific narrative that makes you the best candidate
  ("Your edge is that you've done exactly this at a similar company stage — most
  candidates will have the technical chops but not the startup context")

## Step 5: Output

Present the analysis in this order:

1. **One-line verdict**: "78/100 — Good Match. Apply, but address the Kubernetes gap."
2. **Score breakdown table**: Category | Score | Notes
3. **Red flags / Green flags**: From the JD decoding
4. **Gap analysis with bridge strategies**: Actionable, not just diagnostic
5. **Competitive positioning**: Where you stand relative to the field
6. **Recommended next steps**: Ranked list
   - "Tailor your resume for this role" → triggers `resume-tailor`
   - "Research [Company] deeply" → triggers `interview-coach`
   - "Skip this one — here's why" (if score is very low)

## Step 6: Save to Application Folder

Save the full analysis to `applications/{role-slug}/jd-analysis.md` where
role-slug is derived from the role + company (e.g., "senior-ml-engineer-stripe").

Also update the application tracker in the career profile with a summary.

If the user decides to apply, suggest the next steps in sequence:
1. "Tailor your resume for this role" → triggers `resume-tailor`
2. "Research [Company] deeply" → triggers `interview-coach` Mode 1
3. "Who should I connect with there?" → networking intelligence

### Networking Intelligence Trigger

When match score is 70+ and the user is serious about applying, proactively offer:
"Want me to identify who to connect with at [Company]? I can find the likely
hiring manager, relevant team members, and mutual connection paths."

Use web_search to find:
- Hiring manager (search "[company] [department] [director/VP/manager]" on LinkedIn)
- Team members in similar roles (shows team size, who you'd work with)
- Company alumni who share the user's background (warm intro paths)
- Internal recruiters handling this req

Present in 3 tiers:
- **Tier 1 (highest value):** Hiring manager, direct team leads
- **Tier 2 (warm paths):** Alumni, mutual connections, internal recruiters
- **Tier 3 (broader network):** Industry peers at the company, conference contacts

## Multi-Job Comparison

When the user wants to compare multiple roles:
1. Analyze each one individually using the same framework
2. Present a comparison table: Role | Company | Score | Salary | Red Flags | Verdict
3. Rank them by "adjusted score" = match score × preference alignment
4. Give a recommendation: "If I had to pick one, I'd prioritize [X] because..."

## Tone

Be honest but constructive. If the match is bad, say so — but frame it as useful data,
not discouragement. "This isn't your strongest match, but the analysis reveals what
skills are in demand for roles like this. That's useful intel for your search strategy."

Never inflate scores to make the user feel good. Accuracy builds trust, and trust
makes the whole system more valuable over time.
