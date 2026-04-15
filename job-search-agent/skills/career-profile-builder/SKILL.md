---
name: career-profile-builder
description: >
  Build and maintain a comprehensive career profile by deeply interrogating the user about
  ALL their professional experiences, skills, projects, and achievements — or by parsing
  an uploaded resume file (PDF, DOCX, LaTeX .tex, markdown, or plain text). This is the
  foundation skill for the job search agent system. Trigger this skill whenever the user
  mentions: building a profile, updating their experience, uploading a resume, "what do
  you know about my background", "let's update my profile", "add this experience",
  "I got a new certification", "build my resume from scratch", "parse my CV", or any
  variation of capturing professional identity. Also trigger when any other job-search
  skill (job-analyzer, resume-tailor, interview-coach) needs profile data and none exists
  yet. This skill should be the FIRST thing used before any job search activity. Even if
  the user just says "help me with my job search" — start here if no profile exists.
---

# Career Profile Builder

Build a single source of truth for everything about the user's professional life.
This profile feeds every other skill in the system — match scoring, resume tailoring,
interview prep, and recruiter coaching all read from this same data.

The goal is to capture not just what's on a resume, but the full, rich picture: the
context behind each role, the real impact, the stories worth telling in interviews,
and the preferences that determine which jobs are actually worth pursuing.

## First: Check for Existing Profile

Before doing anything, check if a profile already exists:

```
Load from persistent storage key: "career-profile"
```

- If a profile exists → show the user a summary (completeness score, roles captured,
  number of STAR stories) and ask what they want to update
- If no profile exists → determine the entry path (upload vs. from-scratch)

## Entry Path A: Resume Upload (Fast Start)

When the user uploads or references a resume file:

1. Identify the file type and read it appropriately:
   - **PDF**: Read the SKILL.md in the `pdf-reading` skill first, then extract
   - **DOCX**: Use python-docx or the `docx` skill to extract text
   - **LaTeX (.tex)**: Read as plain text — parse the content structure from LaTeX commands
   - **Plain text / Markdown**: Read directly
2. Parse everything into the profile schema (see `references/profile-schema.md`)
3. Present a structured summary of what was extracted — organized by section
4. Ask the user to confirm accuracy and correct anything wrong
5. Then run the **Gap Fill Interview** — because resumes always leave out the good stuff

### Gap Fill Interview (after upload)

Resumes are marketing documents. They omit context that interviews demand. After parsing,
probe for what's missing:

- **Impact numbers**: "Your resume says you 'improved pipeline efficiency.' Do you have
  specific numbers? Percentage improvement? Revenue impact? Time saved?"
- **Technology depth**: "You list Python and SQL. On a scale of 'I've done tutorials' to
  'I could teach a masterclass,' where are you with each?"
- **Team dynamics**: "For your role at [Company], how big was the team? Who did you
  report to? Did you manage anyone?"
- **The why**: "Why did you leave [Company]? Why did you join [Next Company]?"
- **Hidden achievements**: "What's something you accomplished at [Company] that isn't
  on your resume? Something you're proud of but never figured out how to bullet-point?"
- **STAR story mining**: See Round 5 below — this is critical even with an uploaded resume

## Entry Path B: From-Scratch Interrogation

When building from nothing, run a structured interview in rounds. Never overwhelm —
ask 2-3 questions at a time max, and acknowledge what they share before moving on.

### Round 1 — Orient Fast
```
"Let's build your career profile. I'll ask questions in rounds — we can always
come back and add more later. First, the big picture:

1. What's your current or most recent role and company?
2. What field are you in, and roughly how many total years of experience?
3. What kind of role are you targeting next?"
```

### Round 2 — Work History (most recent first)
For EACH role, capture:
- Company name, what the company does (1 sentence), your title, dates (approx. is fine)
- Team size, who you reported to, anyone you managed
- Top 3-5 things you ACTUALLY did — not job description copy-paste, but what YOU shipped,
  built, led, fixed, or improved
- Quantified impact: revenue, users, efficiency, cost savings, team growth, adoption rates
- Technologies, tools, frameworks, methodologies used daily
- Why you joined, why you left (or why you're considering leaving)

**Probe hard on impact.** Most people undersell themselves. If they say "improved X,"
ask "by how much?" If they say "led a project," ask "how many people, what was the
budget, what was the outcome?" If they say "built a dashboard," ask "who used it, what
decisions did it drive, is it still in use?"

### Round 3 — Skills Deep Dive
Categorize skills by honest proficiency — this matters for match scoring later:
- **Expert** (could teach it, have debugged complex production issues): []
- **Proficient** (use daily, comfortable under pressure): []
- **Working knowledge** (can deliver with docs, have shipped with it): []
- **Familiar** (understand concepts, done projects or courses): []

For non-technical roles, capture domain expertise with the same granularity.

### Round 4 — Education & Credentials
Degrees, relevant coursework, certifications, ongoing learning, bootcamps.
Don't just list — ask: "What did you learn there that you still use professionally?"

### Round 5 — STAR Story Mining (the most important round)

This is where most AI tools fail and where your profile becomes interview gold.
For every significant experience, extract structured stories:

- **Situation**: What was the context? Company stage, team state, business pressure?
- **Task**: What was YOUR specific responsibility? (Not the team's — yours.)
- **Action**: What did YOU do? Step by step. What was your thought process?
- **Result**: What happened? Numbers. Business outcome. What did you learn?

Mine at least **6-8 strong STAR stories** covering these categories:
1. **Leadership / Influence** — led without authority, got buy-in, mentored
2. **Technical Problem-Solving** — debugged, architected, optimized
3. **Conflict / Difficult People** — disagreed with a manager, navigated politics
4. **Failure / Learning** — something went wrong, what you did about it
5. **Cross-Functional Collaboration** — worked across teams/orgs
6. **Ambiguity / Initiative** — no playbook, figured it out yourself
7. **Delivery Under Pressure** — tight deadline, shifting requirements
8. **Customer / User Focus** — made a decision that prioritized users

Rate each story: **strong** (clear, specific, has numbers, compelling), **medium**
(decent but needs polish), or **needs_work** (vague, missing impact).

### Round 6 — The Intangibles
```
"A few more questions that help me position you accurately:

1. What are you known for at work? If I called your last manager, what would
   they say is your superpower?
2. What kind of work energizes you vs. drains you?
3. What are your career goals for the next 2-3 years?
4. What's your ideal company — size, stage, culture, industry?
5. Any hard constraints? Location, visa, salary floor, availability?"
```

## Profile Schema

Store the full profile using the schema defined in `references/profile-schema.md`.
Save to persistent storage with key `career-profile`.

## Completeness Scoring

After each session, calculate and display a completeness score (0-100%):

| Section | Weight | Criteria |
|---------|--------|----------|
| Identity fields | 10% | Name, contact, summary filled |
| Work history (2+ roles with achievements) | 20% | Each role has ≥3 achievements with metrics |
| Skills categorized across 4 levels | 15% | At least 5 skills per level |
| Education filled | 5% | At least one entry with key learnings |
| STAR stories (≥5 rated "strong") | 25% | Covers ≥4 of the 8 categories |
| Preferences (targets, must-haves) | 15% | Target roles and dealbreakers defined |
| Reputation / intangibles | 10% | Known-for and goals filled |

Display as a visual progress summary and tell the user exactly which sections
need more depth to improve their score.

## Incremental Updates

The profile is a living document. When the user returns:
1. Load existing profile from persistent storage
2. Ask what's new — new role? New project? New certification? Updated preference?
3. Merge new data — never overwrite existing entries unless explicitly corrected
4. If they add a new work experience, immediately mine it for STAR stories
5. Bump version, update timestamp, recalculate completeness

## Consistency Enforcement

The profile must stay internally consistent:
- Skills mentioned in work history → must appear in the skills section
- Technologies in projects → must map to a proficiency level
- Date ranges → flag if they overlap impossibly
- STAR stories → must reference real experiences in the profile
- If the user contradicts earlier data, flag it: "Earlier you mentioned X, but
  now you're saying Y — which is accurate?"

## Tone

Be warm, encouraging, and thorough. Many people undersell themselves — it's your
job to draw out their real impact. Frame the interrogation as investment:
"The more detail I have, the better I can match you to jobs, tailor your resume,
and prep you for interviews. Everything you tell me stays in your profile and gets
reused across the entire system."

When they give a vague answer, push gently: "That's great context. Can you put a
number on it? Even a rough estimate helps — 'about 30% faster' is much more
powerful than 'improved speed.'"
