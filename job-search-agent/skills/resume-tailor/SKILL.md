---
name: resume-tailor
description: >
  Generate a targeted, ATS-optimized resume tailored to a specific job description using
  the user's career profile. Produces a polished PDF or DOCX with the right keywords,
  reordered experience, and bullet points that directly address what the JD is asking for.
  Trigger this skill whenever the user says "tailor my resume", "customize my CV",
  "make a resume for this job", "update my resume for this role", "ATS optimize my resume",
  "write resume bullets", "rewrite my experience for this JD", "create a targeted resume",
  or any variation of adapting a resume to a specific opportunity. Also trigger when the
  user has just analyzed a JD with job-analyzer and wants to proceed to the application
  step. If someone says "I want to apply" after analyzing a role, this is the next skill.
---

# Resume Tailor

Generate resumes that are surgically targeted to specific job descriptions — not generic
"one-size-fits-all" documents. Every bullet, every keyword, every ordering decision is
informed by what the JD is actually asking for, matched against the user's real experience.

## Ground Rules

**Never fabricate.** Every claim on the resume must be grounded in the user's profile.
If their profile says "working knowledge of Kubernetes" and the JD wants "expert in K8s,"
do NOT upgrade them to expert. Instead, frame what they actually have honestly:
"Deployed and managed containerized services using Docker with Kubernetes orchestration"
— which is truthful and still hits the keyword.

**Never copy JD language verbatim.** Hiring managers and ATS-flagging tools can detect
when candidates mirror JD phrasing exactly. Instead, use natural variations of key terms.

## Prerequisites

1. Load career profile from persistent storage (key: `career-profile`)
2. Check if a JD analysis exists (from job-analyzer). If yes, use the parsed JD and
   gap analysis. If no, ask the user to paste/provide the JD and parse it inline.
3. If no career profile exists → hand off to `career-profile-builder` first

## Step 1: Strategy Planning

Before writing anything, plan the resume strategy:

### Keyword Extraction
Pull from the JD:
- **Primary keywords**: Skills/tools mentioned in must-haves (use these 2-3 times)
- **Secondary keywords**: Nice-to-haves, methodology terms, industry jargon (use once)
- **Action verb alignment**: What verbs does the JD use? (built, led, designed, scaled,
  optimized, partnered) — mirror the energy

### Experience Prioritization
Decide which roles and achievements from the profile are most relevant:
1. Rank each work history entry by relevance to the JD (high/medium/low)
2. Rank achievements within each entry by JD alignment
3. Decide how many bullets per role: most relevant gets 4-5, others get 2-3,
   irrelevant roles get 1 or get cut entirely
4. Determine if any projects should be elevated to their own section

### Section Ordering
Choose the optimal resume structure for this specific application:
- **Experience-first** (default for 3+ years experience): Summary → Experience →
  Skills → Education
- **Skills-first** (for career changers): Summary → Skills → Relevant Projects →
  Experience → Education
- **Education-first** (for new grads or PhD-required roles): Summary → Education →
  Research/Projects → Experience → Skills

## Step 2: Writing Each Section

### Professional Summary (3-4 lines)
Write a targeted summary that addresses the JD's core needs in the first sentence.

Formula: [Years] + [domain] + [role type] + [with expertise in JD's top 3 requirements]
+ [unique differentiator from profile] + [impact statement with numbers]

```
Example: "Senior data engineer with 6 years building production ML pipelines and
real-time data infrastructure. Expertise in Spark, Airflow, and dbt with a track
record of reducing pipeline latency by 40% and processing costs by $2M annually.
Passionate about data quality and developer experience."
```

### Experience Bullets

Each bullet follows the **Impact-First Formula**:
```
[Strong action verb] + [what you did] + [for whom/what scale] + [measurable result]
```

**Rules:**
- Start every bullet with a strong, varied action verb (never repeat verbs)
- Include at least one metric per bullet when possible
- Front-load the most JD-relevant bullets in each role
- Naturally incorporate primary keywords without forcing them
- Keep bullets to 1-2 lines each — concise, scannable

**Good bullet examples:**
- "Architected real-time event pipeline processing 2M+ events/day using Kafka and Flink,
  reducing data freshness SLA from 4 hours to under 15 minutes"
- "Led cross-functional team of 8 to migrate legacy ETL to dbt, cutting transformation
  runtime by 60% and eliminating 200+ hours/month of manual data fixes"

**Bad bullet examples (avoid):**
- "Responsible for data pipelines" (no impact, no specifics)
- "Utilized cutting-edge technologies to drive synergistic outcomes" (buzzword soup)
- "Worked with team on various projects" (vague, passive)

### Skills Section
Organize skills to front-load JD matches:
- Group by category: Languages | Frameworks | Cloud/Infra | Data | Tools | etc.
- List JD-matched skills first in each group
- Include proficient+ skills only — don't list things at "familiar" level unless
  the JD specifically mentions them as nice-to-have

### Education
- Include GPA only if >3.5 and graduating within last 5 years
- List relevant coursework only if it directly maps to JD requirements
- Certifications that match JD requirements go here or in a separate section

## Step 3: ATS Optimization

Run these checks on the finished resume:

1. **Keyword density**: Every must-have skill from the JD appears at least once in the
   resume (ideally in both Skills and Experience sections)
2. **No graphics/tables/columns**: ATS parsers choke on complex formatting. Use a clean,
   single-column layout for the primary version
3. **Standard section headers**: Use "Experience" not "Where I've Made Impact." ATS
   systems look for conventional headers.
4. **File format**: PDF preserves formatting; DOCX is sometimes requested. Generate both.
5. **No headers/footers for critical info**: Some ATS skip header/footer regions —
   name and contact info go in the body
6. **Date formatting consistency**: Use "Month Year – Month Year" consistently
7. **Spell out acronyms once**: "Natural Language Processing (NLP)" — catches both forms

## Step 4: Generate the Document

Read the appropriate SKILL.md for the output format:
- For PDF output → read `/mnt/skills/public/pdf/SKILL.md`
- For DOCX output → read `/mnt/skills/public/docx/SKILL.md`

### Resume Template Structure

```
[Full Name]
[City, State] | [Phone] | [Email] | [LinkedIn URL] | [GitHub/Portfolio URL]

PROFESSIONAL SUMMARY
[3-4 line targeted summary]

EXPERIENCE
[Most Recent/Relevant Role]
[Company Name] | [Location] | [Start – End]
• [Impact bullet 1 — most JD-relevant]
• [Impact bullet 2]
• [Impact bullet 3]
• [Impact bullet 4 if highly relevant role]

[Previous Role]
...

SKILLS
[Category]: [Skill1, Skill2, Skill3, ...]
...

EDUCATION
[Degree, Field] | [University] | [Graduation Date]
[Relevant coursework / honors if applicable]

CERTIFICATIONS (if applicable)
[Cert Name] | [Issuer] | [Date]
```

## Step 5: Validation & Coaching

After generating, run quality checks:

1. **Truthfulness audit**: Cross-reference every claim against the profile. Flag anything
   that stretches beyond what the user actually reported.
2. **Keyword coverage analysis (target: 70%+)**: List every must-have skill/term from
   the JD. Show coverage:
   ```
   KEYWORD COVERAGE: 78% (14/18 must-have terms found)
   ✓ Python — Experience section bullet 2, Skills section
   ✓ Spark — Experience section bullets 1, 3
   ✗ Snowflake — NOT FOUND. Suggest adding to Skills as "Snowflake (familiar)"
     if profile supports it, or address in cover letter.
   ✓ dbt — Experience section bullet 4
   ...
   ```
   If coverage is below 70%, flag it and suggest where to naturally add missing terms.
3. **Length check**: 1 page for <10 years experience, 2 pages for 10+. Never more than 2.
4. **Readability**: No dense paragraphs, consistent formatting, scannable.
5. **Gap strategy reminder**: For each gap identified in the JD analysis, show how the
   resume addresses it (or flag if it doesn't).

### LinkedIn Sync Recommendations

Generate a separate `applications/{role-slug}/linkedin-updates.md` file listing:
- Headline adjustments to align with the target role
- Summary tweaks to emphasize relevant experience
- Skills to add/reorder on LinkedIn to match JD keywords
- Any inconsistencies between the tailored resume and current LinkedIn
  (dates, titles, descriptions) that an interviewer might notice

Present the resume to the user with:
- "Here's your tailored resume. Keyword coverage: [X]%. Here's what I
  prioritized and why..."
- Any trade-offs made
- LinkedIn sync recommendations if profile data suggests mismatches

## Step 6: Save Version

Save the resume metadata to the application tracker in the career profile:
```json
{
  "resume_version": "resume_[company]_[role]_[date]",
  "target_job_id": "jd_xxx",
  "keywords_optimized_for": [],
  "sections_modified": [],
  "achievements_highlighted": []
}
```

## Iterative Refinement

If the user asks for changes:
- "Make it more technical" → increase technical detail in bullets, add architecture specifics
- "Too long" → cut least-relevant bullets and roles
- "Add more leadership" → elevate management/mentorship achievements
- "Make it more senior" → use strategic language, emphasize scope and impact over tasks
- "ATS isn't picking it up" → check keyword coverage, simplify formatting, spell out acronyms

## Tone

Be confident in the resume but transparent about trade-offs. "I emphasized your ML
pipeline work because that's 3 of their 5 must-haves. Your frontend experience is
real but less relevant here, so I kept it to one bullet." The user should understand
every decision so they can override if needed.
