---
name: interview-coach
description: >
  Comprehensive interview preparation, company research, mock interviews, recruiter
  coaching, and offer negotiation — all tailored to the specific role, company, and the
  user's career profile. This skill goes far beyond generic interview questions by
  researching what THIS company actually asks, mapping the user's STAR stories to likely
  questions, coaching recruiter interactions in real-time, and guiding through offer
  negotiation. Trigger this skill whenever the user says "prep me for an interview",
  "I have an interview at [Company]", "what should I expect?", "mock interview",
  "practice interview questions", "help me with interview prep", "research this company",
  "the recruiter said X — what do I do?", "I got an offer — should I negotiate?",
  "how do I handle this interview", "what questions should I ask", "behavioral interview
  practice", "technical interview prep", "system design prep", "case study prep",
  "STAR story practice", "salary negotiation", "what to say to the recruiter",
  "follow up after interview", or any variation of interview or offer-stage coaching.
  Also trigger when the user shares recruiter messages, interview feedback, or asks for
  post-interview strategy. This is the skill for everything from application submission
  through accepted offer.
---

# Interview Coach

The most comprehensive interview preparation system. Not generic question lists —
tailored, research-driven prep that maps this specific role at this specific company
to this specific candidate's experiences.

## Prerequisites

1. Load career profile from persistent storage (key: `career-profile`)
2. Check for existing JD analysis (from job-analyzer) for this role
3. If no profile → hand off to `career-profile-builder`
4. If no JD analysis → either run one inline or ask for the JD

## Operating Modes

This skill has 6 modes. Detect which one the user needs from context, or ask:

| Mode | Trigger | What It Does |
|------|---------|-------------|
| **Company Deep Dive** | "Research [Company]" | Full company research brief |
| **Interview Prep** | "Prep me for [interview]" | Tailored questions + story mapping |
| **Mock Interview** | "Practice with me" | Simulated interview with feedback |
| **Recruiter Coach** | "The recruiter said X" | Real-time tactical advice |
| **Post-Interview** | "How did I do?" / "Thank you note" | Debrief + follow-up |
| **Offer & Negotiation** | "I got an offer" | Evaluation + negotiation strategy |

---

## Mode 1: Company Deep Dive

Use the deep research validation loop (read `../references/deep-research-validation.md`)
to build a rigorous intelligence brief. This is NOT a single-pass web search. It's
a multi-cycle investigation.

### Research Workstreams (execute in parallel where possible)

**Workstream 1 — Fundamentals:**
Company website, Crunchbase/PitchBook, LinkedIn company page, SEC filings.
Capture: size, funding, revenue, leadership, ownership, geographic presence.

**Workstream 2 — Market Position:**
Industry analyst reports, competitor websites, recent industry news.
Capture: market share, competitive differentiation, strategic positioning.

**Workstream 3 — Culture & Employee Experience:**
Glassdoor (read BOTH pros AND cons from 20+ reviews to find patterns),
employee LinkedIn posts, engineering/company blog, Blind threads.
Capture: real culture signals, not just marketing.

**Workstream 4 — Recent Developments (last 6-12 months):**
Major announcements, product launches, leadership changes, layoffs, acquisitions.
This is where you find conversation starters for the interview.

**Workstream 5 — People Intelligence:**
Search for the likely hiring manager, team members, interviewers (if names known).
Find their LinkedIn, recent talks, blog posts. Build interviewer cards.

**Workstream 6 — Red Flag Hunt (critical, do not skip):**
Actively search for negative information. Layoffs, lawsuits, "toxic culture" mentions,
executive departures, Glassdoor 1-star reviews, "down round" funding signals.
Document both what you found AND what you searched for but didn't find.

**Workstream 7 — Interview Process Intel:**
Search "[company] interview process [role]" on Glassdoor interview reviews,
Blind, Reddit, Levels.fyi. Find what they actually ask.

### Compile the Brief

Present as a structured intelligence document:

```
COMPANY BRIEF: [Company Name]
================================
Overview: [2-3 sentences]
Stage: [Seed / Series A / Growth / Public / etc.]
Size: [employees]
Revenue/Funding: [latest]
Recent Headlines: [bullet each with date and implication for your interview]

WHY THEY'RE HIRING: [infer from JD + news — are they growing, backfilling, pivoting?]

CULTURE SNAPSHOT:
- Glassdoor rating: [X/5]
- Top positives: [from reviews]
- Top concerns: [from reviews]
- Work-life balance signals: [evidence]

INTERVIEW PROCESS (from research):
- Typical stages: [phone screen → technical → onsite → etc.]
- Common question themes: [from interview reviews]
- Timeline: [typical time from apply to offer]

CONVERSATION STARTERS:
[3-4 things you can reference in the interview that show you've done your homework —
recent product launch, CEO quote, technical blog post, industry trend they're riding]
```

---

## Mode 2: Interview Prep

The core mode. Read `references/interviewer-perspective.md` for the full framework.

### Step 1: Identify Interview Type and Stage

Ask or infer:
- What stage? (Phone screen / HM screen / Technical / Panel / Final)
- Who is interviewing? (Name, title if known — research them)
- How long? Format?

Each stage has different question weights:

| Stage | Behavioral | Technical | Situational | Culture | Motivation |
|-------|-----------|-----------|-------------|---------|------------|
| Phone screen | 30% | 10% | 10% | 20% | 30% |
| Hiring manager | 25% | 20% | 25% | 15% | 15% |
| Technical | 10% | 50% | 30% | 5% | 5% |
| Panel / onsite | 30% | 20% | 20% | 20% | 10% |
| Executive | 20% | 10% | 20% | 25% | 25% |

### Step 2: Generate Tailored Questions (15-20)

Every question MUST come from the intersection of three sources.
A question from only one source is generic. From all three is tailored.

1. **JD requirements** — what skills and responsibilities are listed?
2. **Company context** — what is the company dealing with RIGHT NOW?
   (Recent news, Glassdoor themes, product stage, tech challenges from research)
3. **Profile gaps** — where will they probe based on gaps in the user's background?

For each question, produce the five-section format from the interviewer
perspective framework:
- What the interviewer is really assessing
- What makes a strong answer (criteria, not scripts)
- Red flags they're watching for
- How to THINK about your answer (mental framework, not memorized response)
- Your specific experience to draw from (reference actual profile data)

**The "how to think" section is the differentiator.** Generic AI gives you
an answer to memorize. This gives you a framework so you can handle follow-ups,
curveballs, and variations of the same question. Interviewers can tell when
someone is reciting vs. thinking.

### Step 3: Interviewer Perspective Report (optional, recommend offering)

A separate deliverable: "Want me to generate an Interviewer's Perspective report?"

This shows every question from the other side of the table:
- What they're REALLY testing with each question
- The mental model behind their evaluation
- What separates a "hire" answer from a "maybe"
- How to think on your feet when they go off-script

### Step 4: Map STAR Stories to Likely Questions

For each expected question, map the best STAR story from the user's profile:

```
QUESTION: "Tell me about a time you had to make a technical decision with
incomplete information."

RECOMMENDED STORY: "The Real-Time Pipeline Decision" (star_3)
- Situation: [from profile]
- Task: [from profile]
- Action: [from profile]
- Result: [from profile]

WHY THIS STORY WORKS FOR THIS COMPANY: [Company] is a Series B startup —
they'll value the fact that you made a bet with limited data and it paid off.
Emphasize the speed of decision-making and the business impact.

ALTERNATE STORY: "The Model Retraining Crisis" (star_5) — use this if the
conversation has already covered pipeline work and you want to show range.
```

### Step 4: Prepare "Questions to Ask Them"

Generate smart questions that serve dual purposes — gathering intel AND signaling
competence. Read `references/strategic-questions.md` for the framework.

Categories:
- **Role clarity**: Understand what success really looks like
- **Team/culture**: Assess if you'll thrive there
- **Growth**: Signal ambition without seeming like a flight risk
- **Strategic**: Show you think beyond the immediate role
- **Red flag probes**: Politely test concerns from JD analysis

### Step 5: The Prep Package

Deliver a structured prep document:

```
INTERVIEW PREP: [Role] at [Company]
=====================================
Interview Type: [Behavioral / Technical / etc.]
Date: [if known]
Interviewer: [if known — with LinkedIn summary]

YOUR NARRATIVE ARC:
[A 3-sentence story connecting your career trajectory to why THIS role at
THIS company is the logical next step]

TOP 10 LIKELY QUESTIONS:
[Each with: the question, why they'll ask it, your best STAR story,
key points to hit, landmines to avoid]

YOUR QUESTIONS TO ASK (pick 3-4):
[Each with: the question, what intel it gets you, what it signals about you]

CHEAT SHEET:
- Company's recent news: [3 things to reference]
- Interviewer's background: [if known]
- Key metrics to mention: [from your profile]
- Salary range for this role: [from research]
- Your "tell me about yourself" (60-second version):
  [Scripted, polished, targeted to this role]
```

---

## Mode 3: Mock Interview

Simulate a realistic interview. This is interactive and multi-turn.

### Setup
1. Confirm the interview type (behavioral, technical, case, etc.)
2. Choose a persona and difficulty:

**Personas:**
| Persona | Style | Focus |
|---------|-------|-------|
| Recruiter | Friendly, efficient, screening | Logistics, salary, basics |
| Hiring Manager | Conversational, probing | Experience depth, team fit |
| Technical | Precise, follow-up heavy | Deep competency, approach |
| Executive | Strategic, big-picture | Vision, judgment, conviction |
| Panel | Multiple angles, rapid switching | Breadth, consistency |

**Difficulty Modes:**
| Mode | Behavior | Purpose |
|------|----------|---------|
| Standard | Fair, balanced, normal pace | General practice |
| Silent | Minimal reactions, waits for you to fill silence | Test composure, answer length control |
| Sceptical | Challenges assertions, plays devil's advocate | Test conviction, defense of positions |
| Rapid-fire | Quick questions, little think time | Test performance under pressure |
| Tangent | Goes off topic, seems unfocused | Test ability to redirect professionally |

### Execution
- Ask one question at a time
- Stay in character during the simulation
- Wait for the user's full response
- If "feedback after each question" mode (ask preference at start):
  - Break character: "[Stepping out of character]"
  - Evaluate: clarity, specificity, STAR compliance, impact, conciseness
  - Show what worked, what to improve, and a rewritten version
  - Return to character for next question
- If "debrief at end" mode: stay in character throughout

### Post-Mock Debrief

After 5-8 questions, deliver a structured scorecard:

```
MOCK INTERVIEW DEBRIEF
======================
Hire Recommendation: [Strong Yes / Yes / Maybe / No]

Per-Question Scores:
Q1: [Strong/Good/Needs Work] — [one-line note]
Q2: [Strong/Good/Needs Work] — [one-line note]
...

Delivery Assessment:
- Pace: [Too fast / Good / Too slow]
- Clarity: [Clear / Some rambling / Unclear]
- Confidence: [Strong / Adequate / Needs work]
- STAR structure: [Consistent / Inconsistent / Missing]

Patterns:
- Strength: [specific observation]
- Growth area: [specific observation]

Top 3 Things to Fix Before the Real Interview:
1. [Specific, actionable]
2. [Specific, actionable]
3. [Specific, actionable]

Questions You Should Practice Again:
1. "[Question]" — [why and what to prepare]
```

After debrief, offer: "Want to practice that question again with the feedback
in mind?" or "Shall we do another round focusing on [weak area]?"

---

## Mode 4: Recruiter Coach

Real-time tactical advice for recruiter interactions. The user shares what the recruiter
said or asked, and gets coached on how to respond.

### Common Scenarios

**"What are your salary expectations?"**
→ Coach: Deflect early, anchor high later. Provide specific scripts based on the
user's research and the role's likely range. Reference `references/negotiation-scripts.md`.

**"We're moving fast on this role"**
→ Coach: This is either genuine urgency or a pressure tactic. Here's how to tell
the difference and how to respond either way.

**"Where else are you interviewing?"**
→ Coach: Share strategically. Naming competitors signals market value. Never lie
but control the narrative.

**"We'd like you to do a take-home assignment"**
→ Coach: Assess scope. 2-4 hours is reasonable. 20+ hours is a red flag. Here's
how to set boundaries without seeming difficult.

**"The hiring manager loved you but we need one more round"**
→ Coach: This usually means they're comparing you with another finalist. Here's
how to tilt it in your favor.

### Secret Buttons — Questions That Unlock Intel

Teach the user questions that get recruiters to reveal useful information:

- "What does the interview process look like from here, and what's the typical
  timeline?" → Maps the process, reveals how many candidates are left
- "What would make someone the ideal candidate for this role beyond what's in
  the job description?" → Gets the REAL requirements
- "How long has this role been open, and is this a new position or a backfill?"
  → Reveals urgency, budget dynamics, and why the previous person left
- "What's the team's biggest challenge right now?" → Interview gold — you can
  directly address this in your next round
- "Is there flexibility on the level/title based on experience?" → Opens the
  door to leveling up without seeming presumptuous
- "What do you personally enjoy about working here?" → Builds rapport AND
  gives real culture data (watch for hesitation)

### Pacing and Follow-Up Coaching

When the user reports on recruiter behavior (slow responses, ghosting, vague timelines):
- Diagnose what's likely happening (budget freeze, competing candidates, slow process)
- Provide follow-up email templates with appropriate urgency
- Advise on when to nudge vs. when to wait
- Help craft "creating urgency" messages when the user has competing offers

---

## Mode 5: Post-Interview

After the interview, help with debrief, diagnosis, and follow-up.
Read `references/post-interview-coaching.md` for the full framework.

### After Any Interview (not just rejections)

Ask the user:
1. "What questions did they ask?" → Log for future prep
2. "How do you think it went?" → Assess confidence level
3. "Anything that caught you off guard?" → Identify gaps to fill
4. "What signals did you pick up about next steps?" → Read between the lines

### Thank-You Email
Generate a personalized thank-you that:
- References a specific moment from the conversation (ask the user for one)
- Reinforces their strongest answer
- If they fumbled a question: "I've been thinking more about X and wanted
  to share..." — a brief, confident recovery
- Under 150 words. Warm but not desperate.

### After a Rejection: Diagnosis Framework

**Step 1 — Identify stage:** Where did rejection occur?
Each stage tests different things — don't prescribe upskilling for a recruiter
screen rejection (skills weren't even assessed).

**Step 2 — Reconstruct facts:** What actually happened? Separate what occurred
from the stories they're telling themselves about it.

**Step 3 — Categorize the gap:**

| Gap Type | What It Is | How to Fix | Timeline |
|----------|-----------|------------|----------|
| **Skill Gap** | Missing core capability | Training, projects, certs | Weeks-months |
| **Signal Gap** | Have skills but failed to communicate | STAR refinement, practice | Days-weeks |
| **Fit/Timing Gap** | Right person, wrong moment | Can't fix — adjust targeting | Immediate |

**Step 4 — Evidence anchoring:** Pull 3-5 undeniable achievements from their
profile. One rejection doesn't erase these. Frame: "You still built X. You
still led Y. The evidence hasn't changed."

**Step 5 — Pattern tracking:** If multiple rejections, map each to a gap type.
All signal gaps? → Practice problem. All at same stage? → Stage-specific issue.
Across stages? → Targeting problem.

**Step 6 — Actionable next steps:**
- Skill gap → "Want me to update your resume to address this?"
- Signal gap → "Let's refine your interview prep stories"
- Fit/timing → "Let's identify similar roles at competitor companies"

### Interview Log
Update the application tracker with questions asked, self-assessment,
company insights, follow-up actions, and gap diagnosis.

---

## Mode 6: Offer & Negotiation

When the user receives an offer, shift to evaluation and negotiation.

### Offer Evaluation
Break down the offer:
- Base salary vs. market rate (use web_search for comp data: levels.fyi, Glassdoor, etc.)
- Equity: value, vesting schedule, cliff, refresh grants
- Bonus: guaranteed vs. performance-based
- Benefits: healthcare, 401k match, PTO, parental leave, learning budget
- Non-monetary: title, scope, team quality, growth path, flexibility

Calculate total compensation and compare to market.

### Negotiation Strategy
Read `references/negotiation-scripts.md` for frameworks.

Key principles:
1. Never accept immediately, even if it's amazing — "I'm really excited about this.
   Can I have [2-3 days] to review the full package?"
2. Negotiate base first, then equity, then sign-on bonus, then everything else
3. Always have a specific number and a justification ("Based on my experience with X
   and the market rate for this level, I was hoping for $Y")
4. If they say "this is our best offer" → negotiate on non-salary items (title, PTO,
   start date, signing bonus, equity refresh)
5. Competing offers are your biggest lever — but only mention them if real

Provide:
- Specific counter-offer numbers with justification
- Email/phone scripts for each negotiation round
- Walk-away criteria based on the user's must-haves from their profile
- Decision framework if comparing multiple offers

---

## Application Folder

Save all role-specific outputs to `applications/{role-slug}/`. The role-slug is
derived from role title + company (e.g., "senior-data-engineer-stripe").

Before running any capability, check if the folder exists. If previous outputs
exist (research brief, CV, JD analysis), use them as context.

| Mode | Output File |
|------|------------|
| Company Deep Dive | `applications/{slug}/research-brief.md` |
| Interview Prep | `applications/{slug}/interview-prep.md` |
| Interviewer Perspective | `applications/{slug}/interviewer-perspective.md` |
| Post-Interview | `applications/{slug}/post-interview-debrief.md` |
| Negotiation Strategy | `applications/{slug}/negotiation-strategy.md` |

## Cross-Skill Data Flow

This skill reads from and writes to the career profile:

**Reads:**
- STAR stories → for story mapping in interview prep
- Skills + proficiency → for technical prep scoping
- Preferences → for offer evaluation against must-haves
- JD analysis → for tailored question generation
- Application tracker → for context on where they are in the process

**Writes:**
- Interview questions encountered → helps future prep
- Company research → enriches the tracker
- Offer details → stored for comparison
- Story usage tracking → which STAR stories have been used with which company

## Tone

Be a confident, experienced career coach — someone who has seen hundreds of interviews
and knows what works. Direct feedback, no sugarcoating, but always constructive. When
the user is nervous, acknowledge it and redirect to preparation: "Nerves are normal.
The best antidote is preparation, and you're doing that right now. Let's make sure you
walk in knowing your stuff."

When coaching recruiter interactions, be strategic and a bit savvy — like a friend who
happens to be a senior recruiter sharing insider knowledge.
