# Interviewer Perspective Framework

Generate questions from the interviewer's viewpoint: what they're really assessing,
what makes a strong answer, what red flags they watch for, and how to THINK about
your answer rather than memorizing scripts.

## Why This Matters

Generic AI interview prep fails because it produces generic questions and scripted
answers. Real interviewers ask questions that probe specific things about the role,
the company's current challenges, and the gaps in your resume. This framework
generates questions from that intersection.

## For Each Question, Produce Five Sections

```
### Question: "{The interview question}"

**What the interviewer is really assessing:**
{The underlying competency or concern — be specific, not generic.
"Can you handle ambiguity" is generic.
"Can you make technical decisions when the data team is 3 months
behind on building the pipeline you need" is specific to the role.}

**What makes a strong answer:**
- {Criterion 1: what good looks like with specifics}
- {Criterion 2: what demonstrates mastery}
- {Criterion 3: what separates excellent from decent}

**Red flags they're watching for:**
- {Red flag 1: what makes them concerned}
- {Red flag 2: common mistake candidates make}
- {Red flag 3: what signals lack of fit for THIS company}

**How to think about your answer:**
{Not a script. A mental framework.
"Start by asking yourself: [opening question to frame thinking]
Then consider: [second layer]
Finally: [how to structure and conclude]"}

**Your experience to draw from:**
{Reference SPECIFIC experience from the user's profile/CV that
could be relevant to this question. Don't say "draw on your
leadership experience." Say "your work leading the data migration
at [Company] is perfect here — that was exactly the kind of
ambiguity they're testing for."}
```

## Question Generation Strategy

Generate 15-20 questions across these categories, weighted by interview stage:

### Stage Weights

| Stage | Behavioral | Technical | Situational | Culture | Motivation |
|-------|-----------|-----------|-------------|---------|------------|
| Phone screen | 30% | 10% | 10% | 20% | 30% |
| Hiring manager | 25% | 20% | 25% | 15% | 15% |
| Technical round | 10% | 50% | 30% | 5% | 5% |
| Panel / onsite | 30% | 20% | 20% | 20% | 10% |
| Executive / final | 20% | 10% | 20% | 25% | 25% |

### Three Sources for Every Question

Each question must be derived from the intersection of:

1. **JD requirements** — what skills/responsibilities are listed?
2. **Company context** — what's the company dealing with right now?
   (From company research: recent news, Glassdoor themes, product stage)
3. **Profile gaps** — where will they probe based on resume gaps?

A question that comes from only ONE source is generic.
A question from the intersection of all THREE is tailored.

**Example — Generic vs. Tailored:**

| Source | Generic | Tailored |
|--------|---------|----------|
| JD only | "Tell me about your data engineering experience" | — |
| JD + Company | — | "Our data team is migrating from Redshift to Snowflake while supporting 200 daily dbt models. Walk me through how you'd sequence that migration without breaking production." |
| JD + Company + Gaps | — | "I see you've worked primarily with Spark but this role is heavy Snowflake. Tell me about a time you picked up a new technology under production pressure — how did you ramp?" |

## Operating Rules

1. Never provide scripted answers — give thinking frameworks
2. Reference specific experience from the user's actual CV/profile
3. Tailor question difficulty to role level (exec vs. IC)
4. If company research exists, use it to make questions company-specific
5. Include at least 3 "gap-probing" questions based on JD analysis
6. Include at least 2 questions the user might not expect
7. Include follow-up probes for each behavioral question

## Quality Check

Before finalizing:
- [ ] Every question has all 5 sections filled
- [ ] "How to think" sections are frameworks, not scripts
- [ ] Red flags are honest and specific to this company/role
- [ ] Experience references pull from actual profile data
- [ ] Questions reflect the interview stage
- [ ] At least 3 questions use company research
- [ ] At least 3 questions probe identified gaps
