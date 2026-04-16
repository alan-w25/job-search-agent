---
name: cover-letter-writer
description: >
  Generate targeted, non-generic cover letters from the user's career profile and JD
  analysis. Produces a three-act letter with company-specific opening hooks, achievement-
  to-requirement value bridges, and concrete closings — calibrated to the company's culture
  and stage. Includes anti-pattern detection, ATS-friendly formatting, and iterative
  refinement. Trigger this skill whenever the user says "write a cover letter",
  "cover letter for this role", "draft a cover letter", "help me with my cover letter",
  "generate a cover letter for [Company]", "I need a cover letter", "customize my cover
  letter", "cover letter from my resume", "write me a letter for this job",
  "application letter for [role]", or any variation of creating or improving a cover letter.
  Also trigger when the user has completed a JD analysis or resume tailoring and wants
  to add a cover letter to the application package. If someone says "I want to apply"
  and a resume already exists for the role, offer this as the next step.
tags:
  - application
  - writing
  - cover-letter
---

# Cover Letter Writer

Generate cover letters that read like a knowledgeable professional writing to a peer —
not a form letter, not a supplicant begging for a job. Every paragraph earns its place
by connecting a specific thing about you to a specific thing the company needs.

## Ground Rules

**Never fabricate.** Every claim must trace back to the user's career profile. If the
profile doesn't support a connection, don't invent one — find a different angle or flag
the gap.

**Never produce a generic letter.** If any sentence could apply to any candidate for
any role, rewrite it. The cover letter must be so specific that it only works for this
person applying to this role at this company.

**Never repeat resume bullets.** The cover letter is not a prose version of the resume.
It adds context, motivation, and narrative that the resume format cannot convey.

## Prerequisites

1. Load career profile from persistent storage (key: `career-profile`)
2. Check for existing JD analysis (from job-analyzer) for this role
3. If no profile exists --> hand off to `career-profile-builder`
4. If no JD analysis --> parse the JD inline or ask the user to provide it
5. Check for company research (from interview-coach) if available — use it for
   richer company-specific references in the letter

## Capability Overview

| Capability | What It Does |
|------------|-------------|
| **Connection Point Analysis** | Identifies the 2-3 strongest links between profile and JD |
| **Three-Act Letter Generation** | Opening hook, value bridge, specific closing |
| **Tone Calibration** | Adjusts formality and style by company culture/stage |
| **Anti-Pattern Detection** | Flags and rewrites generic or weak language |
| **ATS-Friendly Formatting** | Plain text variant for online forms |
| **Iterative Refinement** | Targeted edits based on user feedback |

---

## Step 1: Connection Point Analysis

Before writing a single word, identify the 2-3 strongest connection points between
the user's profile and the JD. These become the anchors of the letter.

Connection points are NOT just skill matches. Look for:

| Connection Type | What to Look For | Why It Works |
|----------------|-----------------|-------------|
| **Shared mission/values** | Company mission statement vs. user's stated motivations | Shows intrinsic alignment, not just capability |
| **Problem-solution match** | Problems the company has (from JD, news, Glassdoor) vs. problems the user has solved | Proves you've already done the work they need |
| **Unique experience mapping** | Unusual or distinctive experience in the profile that maps to a specific JD need | Differentiates from every other qualified applicant |
| **Scale/context match** | Similar company stage, team size, data volume, user base | Reduces perceived ramp-up risk |
| **Industry or domain overlap** | Same vertical, same regulatory environment, same customer type | Signals domain fluency without stating it |

**Rank connection points by specificity.** "I have Python experience and the JD
requires Python" is weak — every applicant has that. "I built a real-time fraud
detection pipeline processing 5M events/day and your JD mentions building
real-time risk scoring" is strong.

Present the top 3 connections to the user before drafting:

```
CONNECTION POINTS IDENTIFIED:
1. [Connection type]: [Specific link — profile fact <-> JD requirement]
   Strength: [HIGH / MEDIUM]
2. [Connection type]: [Specific link]
   Strength: [HIGH / MEDIUM]
3. [Connection type]: [Specific link]
   Strength: [HIGH / MEDIUM]

Proceed with these as the letter's anchors? Or suggest adjustments?
```

---

## Step 2: Three-Act Letter Structure

Every cover letter follows a three-act structure. Each act has a specific job.
Load `@references/cover-letter-patterns.md` for templates and examples.

### Act 1: Opening Hook (2-3 sentences)

The opening must accomplish two things simultaneously:
1. Show you know something specific about the company
2. Connect that knowledge to your experience

**NEVER open with:**
- "I am writing to apply for the position of..."
- "I was excited to see your posting on..."
- "With X years of experience in Y, I am confident..."

**INSTEAD, lead with one of five hook patterns** (see reference file for templates):

| Hook Pattern | When to Use |
|-------------|------------|
| **Company News Hook** | Recent product launch, funding round, expansion, or announcement |
| **Shared Mission Hook** | Company mission aligns with a personal motivation you can substantiate |
| **Problem-Solution Hook** | You've solved a problem they clearly have (from JD or research) |
| **Mutual Connection Hook** | You have a referral or shared professional connection |
| **Industry Insight Hook** | You have perspective on an industry challenge they're facing |

Choose the hook based on available information. Company news hook requires recent
research. Mutual connection hook requires an actual connection (never fabricate one).

### Act 2: Value Bridge (2-3 paragraphs)

Each paragraph maps ONE specific achievement from the profile to ONE specific
requirement from the JD. This is the core of the letter.

**Value Bridge Formula:**
```
[Their need from the JD] --> [Your relevant experience] --> [Quantified result]
--> [Why this matters for THEM specifically]
```

**Rules for the value bridge:**
- Use the Impact-First Formula from resume-tailor for the achievement component
- Do NOT repeat resume bullets verbatim — expand with context, motivation, and
  narrative that doesn't fit in bullet format
- Each paragraph should answer: "Why should they believe you can do this job?"
- Connect the result back to THEIR context: "This experience is particularly
  relevant because [Company] is [scaling / migrating / building / etc.]..."
- If the user has a gap in a must-have requirement, address it proactively in the
  bridge — show adjacent experience and learning trajectory, not avoidance

**Paragraph structure within the value bridge:**
1. First paragraph: Strongest connection point. Your biggest, most relevant achievement.
2. Second paragraph: Second connection point. Different skill/domain to show range.
3. Third paragraph (optional, only if under 400 words): Addresses a gap, shows
   cultural fit, or adds a dimension that paragraphs 1-2 don't cover.

### Act 3: Closing with Specificity (2-3 sentences)

The closing must be specific to this company and this role. It should:
1. Reference something concrete about the role, team, or company
2. Express genuine interest tied to a specific reason (not "I am passionate about
   your company's mission" unless you name the mission and explain why)
3. End with clear next-step language

**NEVER close with:**
- "I look forward to hearing from you"
- "I believe I would be a great fit"
- "Please do not hesitate to contact me"
- "Thank you for your time and consideration" (as the final sentence — this is
  acceptable mid-closing but should not be the last thing they read)

See reference file for closing patterns that work.

---

## Step 3: Tone Calibration

Detect company culture from JD analysis signals (green flags, culture signals,
company stage) and calibrate the letter's tone accordingly.

| Company Type | Tone | Language Choices | Structure |
|-------------|------|-----------------|-----------|
| **Startup (early-stage)** | Conversational, energetic | First-person stories, contractions OK, show personality and hustle | Slightly looser structure, can break convention |
| **Growth-stage** | Balanced professional/personal | Results-oriented, data-driven, still personable | Standard structure with personality in hooks and transitions |
| **Enterprise / Corporate** | Formal, polished | No contractions, emphasis on scale and process, measured language | Strict three-act structure, conservative formatting |
| **Mission-driven / Nonprofit** | Warm, values-forward | Lead with "why" before "what", connect personal values to org mission | Values hook opening, impact-focused rather than revenue-focused |
| **Creative / Agency** | Bold, distinctive | Voice matters — show personality, take risks with phrasing | More freedom with structure, voice is a signal of fit |

**Detection signals from JD analysis:**
- "Fast-paced environment" + "wear many hats" --> Startup tone
- "Cross-functional collaboration" + "stakeholder management" --> Enterprise tone
- "We're on a mission to..." + values listed prominently --> Mission-driven tone
- "Portfolio required" + "creative problem solving" --> Creative tone
- Funding stage, employee count, and company age confirm or override JD signals

If tone detection is ambiguous, default to Growth-stage (balanced) and ask the user.

---

## Step 4: Anti-Pattern Detection

After drafting, scan the letter against the anti-pattern catalog. Flag and rewrite
any instance. Load `@references/cover-letter-patterns.md` for the full catalog
with before/after examples.

| Anti-Pattern | Why It Fails | Detection Rule |
|-------------|-------------|---------------|
| "I am writing to apply for..." | Generic, wastes the most valuable real estate in the letter | First sentence contains "writing to apply" or "applying for the position" |
| "I am a hardworking team player" | Meaningless — everyone says this | Any adjective-noun self-description without evidence |
| Resume bullet repetition | Shows laziness, adds no new information | Any sentence that matches a resume bullet within 80% similarity |
| "I believe I would be a great fit" | Telling, not showing | Any sentence containing "great fit" or "perfect fit" or "strong fit" |
| "Dear Hiring Manager" when name is known | Missed personalization opportunity | Greeting uses generic title when JD analysis or research identified a name |
| Company flattery without self-connection | Sycophantic, wastes space | Any paragraph about the company that doesn't connect back to the user's experience |
| Generic sentences | Could apply to any candidate for any job | Any sentence that passes the "swap test" — if you can swap in another company or candidate name and it still works, it's generic |

**Anti-pattern scan output:**
```
ANTI-PATTERN SCAN:
[PASS] No generic opening detected
[WARNING] Line 8: "I believe I would be a strong addition to your team"
  --> Rewritten: "My experience building [X] at [Y] scale maps directly
      to the [specific challenge] described in the role."
[PASS] No resume bullet repetition detected
[PASS] All company references connect to user experience
```

---

## Step 5: ATS-Friendly Formatting

Generate two variants:

### Standard Variant (for email or PDF attachment)
- Light formatting: bold for emphasis sparingly, clear paragraph breaks
- Professional salutation and sign-off
- Contact information in sign-off block

### Plain Text Variant (for online application form paste)
- No formatting characters (no bold, no italics, no special characters)
- No bullet points — full sentences and paragraphs only
- No salutation/sign-off (application forms handle this)
- Shorter: target the low end of the word count range
- ASCII-only characters (no em-dashes, curly quotes, or special punctuation)

---

## Step 6: Output and Validation

### Save Location
Save to `applications/{role-slug}/cover-letter_[company]_[role]_[date].md`

If a previous cover letter exists for this role, save the new version alongside
it (append version number) rather than overwriting.

### Validation Checklist

After generating, run these checks and display results:

```
COVER LETTER VALIDATION
========================
Word count: [N] words [PASS: 250-400 / WARNING: 400-500 / FAIL: >500]

JD REQUIREMENT COVERAGE:
[ADDRESSED] Requirement 1 — covered in paragraph 2
[ADDRESSED] Requirement 2 — covered in paragraph 1
[NOT ADDRESSED] Requirement 3 — suggest adding a bridge sentence
[ADDRESSED] Requirement 4 — covered in opening hook

Anti-pattern scan: [PASS / N warnings found — see above]

Tone calibration: [Startup / Growth / Enterprise / Mission / Creative]
  Matches detected company culture: [YES / REVIEW RECOMMENDED]

Specificity test: [N] company-specific references, [N] role-specific references
  Minimum threshold: 2 company-specific, 2 role-specific [PASS / FAIL]

Connection points used: [3/3] [PASS]
```

### Critical Requirement Gaps

If any must-have JD requirements are not addressed in the letter:

```
COVERAGE GAPS — SUGGESTED ADDITIONS:
- [Requirement]: Consider adding a sentence in paragraph 2 connecting your
  [adjacent experience] to this need. Draft: "[suggested sentence]"
```

---

## Step 7: Iterative Refinement

When the user requests changes, apply targeted edits:

| User Request | Action |
|-------------|--------|
| "More personal" | Add anecdotes from profile, reduce formal language, use first-person narrative, add a "why I care" thread |
| "More professional" | Tighten language, remove casual phrasings, use measured tone, ensure each claim has evidence |
| "Shorter" | Cut to 250 words. Reduce to one value bridge paragraph (strongest connection). Tighten opening and closing to 1-2 sentences each. |
| "Longer" | Add a third value bridge paragraph. Expand context around achievements. Never exceed 500 words. |
| "Address the [X] gap" | Add a bridge paragraph that acknowledges the gap honestly and connects adjacent experience: "While my direct experience with [X] is limited, my work on [Y] involved [related skills] at [scale], giving me a foundation to [ramp quickly / contribute from day one]." |
| "Make it sound more like me" | Ask the user for a writing sample or describe their natural tone. Adjust vocabulary, sentence length, and cadence to match. |
| "Different opening" | Generate 3 alternative hooks using different hook patterns and let the user choose |
| "More specific" | Replace any remaining general claims with concrete numbers, project names, and outcomes from the profile |

After each refinement, re-run the validation checklist to confirm the edit didn't
introduce anti-patterns or push the word count out of range.

---

## Cross-Skill Data Flow

**Reads:**
- `career-profile` — achievements, skills, motivations, and STAR stories
- JD analysis (from `job-analyzer`) — parsed requirements, gap analysis, culture signals
- Company research (from `interview-coach`) — recent news, culture data, team intel
- Tailored resume (from `resume-tailor`) — to avoid duplicating bullet language

**Writes:**
- Cover letter file to `applications/{role-slug}/`
- Application tracker update: cover letter version, date, target role, word count

---

## Tone

Confident but genuine. The cover letter should read like a knowledgeable professional
writing to a peer — someone who respects the reader's time and intelligence. Not
obsequious. Not arrogant. The posture is: "I've done work that's relevant to what you
need, and here's why that matters."

Never beg. Never grovel. Never use phrases like "I would be honored" or "it would be
a privilege" — these signal desperation, not confidence. The user has valuable skills
and experience. The cover letter should reflect that.
