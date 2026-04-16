---
name: networking-strategist
description: >
  Strategic networking skill that helps you leverage existing relationships and build
  new ones to access the hidden job market — the 70%+ of roles filled through referrals,
  internal recommendations, and word-of-mouth before they ever hit public job boards.
  Covers target company connection mapping, personalized outreach message generation,
  informational interview preparation, referral request scripting, LinkedIn profile
  optimization, and warm introduction chain mapping. Trigger this skill whenever the
  user says "who do I know at [Company]", "help me network", "write a LinkedIn message",
  "write an outreach message", "help me get a referral", "optimize my LinkedIn",
  "review my LinkedIn profile", "prep me for a coffee chat", "informational interview",
  "how do I reach the hiring manager", "warm intro", "networking strategy",
  "connect me to someone at [Company]", "follow up with my contact", "who should I
  talk to", "build my network", "cold outreach", "LinkedIn connection request",
  "networking tracker", or any variation of leveraging professional relationships
  for a job search. Also trigger when the user has identified a target company and
  wants to find a path in through people rather than just applying online.
tags:
  - networking
  - outreach
  - linkedin
  - referrals
  - informational-interviews
  - relationship-building
---

# Networking Strategist

Strategic networking that actually works. Not "spray and pray" LinkedIn requests —
targeted, relationship-first outreach that opens doors the application portal never will.

The math is simple: 70%+ of roles are filled through referrals and internal
recommendations. A referred candidate is 4-5x more likely to be hired than a cold
applicant. Your network is not a nice-to-have — it is your primary job search channel.

## Ground Rules

**Networking is relationship-building, not transaction processing.** Every template
and strategy in this skill is designed to create genuine professional connections.
If you treat people as vending machines that dispense referrals, they will sense it
immediately and you will burn bridges.

**Never be dishonest about intent.** You can be strategic without being manipulative.
"I'm exploring opportunities in [field] and would love to hear about your experience
at [Company]" is honest and strategic. Pretending you just want to chat when you
actually want a referral is manipulative.

**Quality over quantity.** Five thoughtful conversations with well-targeted contacts
will produce more results than 50 generic LinkedIn requests.

**Respect people's time.** Always make asks specific, provide context, and make it
easy for people to help you.

## Prerequisites

1. Load career profile from persistent storage (key: `career-profile`)
2. Check for target companies from preferences and application tracker
3. If no profile exists, hand off to `career-profile-builder` first
4. If user has specific target companies, pull any existing JD analysis from `job-analyzer`

## Operating Modes

Detect which mode the user needs from context, or ask:

| Mode | Trigger | What It Does |
|------|---------|-------------|
| **Target Company Mapping** | "Who do I know at [Company]?" | Map connections and paths to the hiring team |
| **Outreach Messages** | "Write a message to..." | Generate personalized outreach for any context |
| **Informational Interview Prep** | "Coffee chat prep" | Questions, positioning, follow-up strategy |
| **Referral Request** | "How do I ask for a referral?" | Scripts for every relationship depth |
| **LinkedIn Optimization** | "Review my LinkedIn" | Full profile audit with specific fixes |
| **Warm Intro Chains** | "Find me a path to..." | Multi-hop connection mapping |
| **Networking Tracker** | "Track my networking" | Log contacts, interactions, follow-ups |

---

## Mode 1: Target Company Mapping

Build a connection map for a specific target company. The goal: identify WHO to
reach out to, in WHAT order, through WHAT path.

### Step 1: Gather Intelligence

Ask the user:
- What company are you targeting?
- Do you know anyone who works there (or used to)?
- Do you know the specific team or role?
- What school(s) did you attend? What companies have you worked at?

### Step 2: Connection Tiering

Organize potential contacts into tiers by warmth:

**Tier 1 — Warmest (reach out first):**
- Alumni from same school who work there now
- Former colleagues who moved to this company
- 1st-degree LinkedIn connections at the company
- Friends of friends you've actually met
- Success rate: 40-60% response rate when personalized

**Tier 2 — Warm (reach out after Tier 1):**
- 2nd-degree connections (connected through a mutual contact)
- People who share meaningful background (same bootcamp, same niche community,
  same open-source project, same volunteer org)
- People who have posted about the role/team/company publicly
- Success rate: 15-25% response rate when personalized

**Tier 3 — Cold but Strategic (reach out in parallel or after Tier 2):**
- Hiring managers for the target role
- Team leads on the target team
- Internal recruiters for the department
- People who recently posted the job listing
- Success rate: 5-15% response rate, but high value when they respond

### Step 3: Research Each Contact

Use web_search to find for each high-priority contact:
- LinkedIn profile (role, tenure, background overlaps)
- Blog posts, articles, or Medium posts they've written
- Conference talks or podcast appearances
- Open-source contributions or GitHub activity
- Recent professional achievements (promotions, project launches)
- Shared connections who could make an introduction

### Step 4: Build the Connection Map

Output a structured map:

```
TARGET: {{COMPANY_NAME}} — {{TARGET_ROLE}}

TIER 1 — DIRECT CONNECTIONS
1. {{NAME}} — {{ROLE}} at {{COMPANY}}
   Connection: {{HOW_YOU_KNOW_THEM}}
   Shared context: {{WHAT_YOU_HAVE_IN_COMMON}}
   Approach: {{RECOMMENDED_OUTREACH_METHOD}}
   Notes: {{ANYTHING_RELEVANT — recent posts, mutual connections}}

TIER 2 — WARM PATHS
1. {{NAME}} — {{ROLE}} at {{COMPANY}}
   Path: You -> {{MUTUAL_CONNECTION}} -> {{NAME}}
   Shared context: {{WHAT_YOU_HAVE_IN_COMMON}}
   Approach: {{ASK_MUTUAL_FOR_INTRO_OR_REACH_OUT_DIRECTLY}}

TIER 3 — STRATEGIC COLD OUTREACH
1. {{NAME}} — {{ROLE}} at {{COMPANY}}
   Why them: {{HIRING_MANAGER / TEAM_LEAD / POSTED_THE_ROLE}}
   Hook: {{SOMETHING_SPECIFIC_TO_PERSONALIZE_OUTREACH}}
   Approach: {{RECOMMENDED_CHANNEL_AND_MESSAGE_TYPE}}

RECOMMENDED SEQUENCE:
1. [Week 1] Reach out to Tier 1 contacts: {{NAMES}}
2. [Week 1] Ask {{MUTUAL_CONNECTION}} for intro to {{TIER_2_NAME}}
3. [Week 2] Follow up on Tier 1, send Tier 2 outreach
4. [Week 2-3] Cold outreach to Tier 3 if Tiers 1-2 haven't yielded
```

### Step 5: Prioritization

Rank all contacts by a composite score:
- **Warmth** (how strong is the connection or shared background): 40% weight
- **Proximity to hiring** (are they on the team, the hiring manager, or adjacent): 35% weight
- **Accessibility** (have they posted recently, do they seem active/responsive): 15% weight
- **Uniqueness** (can they tell you something you can't learn elsewhere): 10% weight

---

## Mode 2: Outreach Message Generation

Read `@references/outreach-templates.md` for templates and anti-patterns.

Generate personalized outreach messages for any context. Every message must be:

1. **Personalized** — Reference something specific about the recipient (their work,
   background, a post they wrote, a shared connection). Generic messages get ignored.
2. **Concise** — Respect their time. LinkedIn connection requests MUST be under 300
   characters. Emails should be under 150 words for initial outreach.
3. **Specific in the ask** — "Could I ask you 2-3 questions about the data platform
   team?" not "I'd love to pick your brain."
4. **Easy to say yes to** — Low commitment first. A 15-minute call, not a 1-hour lunch.

### Message Types

**LinkedIn Connection Request** (under 300 characters):
- Personalized to shared context (alumni, company, industry, mutual connection, cold)
- Must include: who you are, why you're reaching out, what you have in common
- Must NOT include: your life story, a job ask, anything generic

**Coffee Chat / Informational Interview Request:**
- Clear reason you're reaching out to THEM specifically
- Specific ask (15-20 minute call, 2-3 questions about X)
- Flexible on timing, offer multiple options
- Mention how you found them or what you have in common

**Follow-Up After Connection Accepted:**
- Thank them for connecting
- Brief context (1 sentence about you)
- Do NOT immediately ask for a referral — this is the #1 mistake
- Instead: ask a thoughtful question or share something relevant

**Referral Request** (only after establishing rapport):
- Reference your previous interaction(s)
- Make it easy: include your resume, the specific role link, and 2-3 talking points
  they can use to recommend you
- Give them an easy out ("No pressure at all — I completely understand if this
  isn't something you're comfortable with")

**Thank-You After Meeting:**
- Specific reference to something you discussed
- Share a follow-up on something they mentioned
- Leave the door open for future contact

**Keeping Warm:**
- Share an article relevant to something they care about
- Congratulate on a promotion, work anniversary, published post
- Reference a conversation topic from your last interaction

### Customization Inputs

For every message, gather:
- Recipient's name and current role
- How you're connected (or not)
- Shared background or context
- Your specific goal for this outreach
- Any recent activity from them to reference (posts, talks, achievements)
- The warmth level (Tier 1 / 2 / 3 from Mode 1)

---

## Mode 3: Informational Interview Prep

Prepare for coffee chats and informational interviews so you come across as a
thoughtful professional — not a desperate job seeker.

### What to Ask

**Industry and Role Questions (high value):**
- "What does a typical week look like for someone in your role?"
- "What are the biggest challenges your team is facing right now?"
- "How has the [specific technology/methodology] landscape changed in the last year?"
- "What skills or experiences do you think are most valuable for someone moving
  into this space?"
- "What do you wish you'd known when you were at my stage?"

**Company-Specific Questions (shows you've done research):**
- "I read about [Company]'s recent [initiative/launch/pivot]. How has that affected
  your team?"
- "What's the engineering culture like day-to-day? I saw [specific blog post/talk]
  and I'm curious if that matches your experience."
- "How does [Company] approach [specific technical challenge relevant to the role]?"

**Career Path Questions (builds rapport):**
- "What drew you to [Company] originally? What's kept you there?"
- "How did you transition from [their previous role] to [current role]?"
- "What advice would you give someone trying to break into [their field/level]?"

### What NOT to Ask

- "Are you hiring?" — They'll tell you if they want to
- "Can you refer me?" — Not in the first conversation, ever
- "What's the salary for...?" — Use Glassdoor/levels.fyi for this
- Anything easily found on the company website — Shows you didn't prepare
- "What does your company do?" — Immediately disqualifying
- Negative questions about the company — Save for after you have rapport

### How to Position Yourself

**Do:**
- Frame yourself as a curious professional exploring a specific direction
- Mention 1-2 relevant accomplishments naturally, not as a pitch
- Show you've done homework on the company AND the person
- Ask follow-up questions that show you're actually listening
- Take notes (ask permission first: "Mind if I jot down notes?")

**Do NOT:**
- Open with "I'm looking for a job" — even if that's why you're there
- Deliver your elevator pitch unprompted
- Dominate the conversation — aim for 70% them, 30% you
- Check your phone or seem distracted
- Go over the time you asked for unless they explicitly extend

### How to Naturally Signal Interest Without Asking for a Job

- "The work your team is doing on [X] is really interesting to me — it aligns
  with the direction I want to take my career."
- "If opportunities come up on your team, I'd love to be considered. But honestly,
  this conversation alone has been really valuable."
- "I'm keeping an eye on [Company] — the [specific thing] really resonates with
  what I'm looking for in my next role."

### Follow-Up Strategy

| Timing | Action |
|--------|--------|
| Within 24 hours | Send thank-you email/message referencing something specific |
| Within 1 week | Share something relevant (article, resource, connection) |
| 2-4 weeks later | Brief check-in or share an update on your search |
| Monthly | "Keeping warm" touchpoint — congratulate, share, or reference past conversation |
| When you land a role | Let them know and thank them for their help |

---

## Mode 4: Referral Request Scripting

Read `@references/outreach-templates.md` for referral request templates.

Referrals are the most valuable networking outcome — but asking too early or too
clumsily will backfire. A good referral request makes it easy and comfortable for
the person to say yes.

### When to Ask

- PASS: After at least one meaningful interaction (informational interview, worked
  together, shared a project or community involvement)
- PASS: When you've seen a specific role posted that matches your background
- PASS: When they've already offered ("Let me know if I can help with your search")
- FAIL: First conversation or message — always too early
- FAIL: When you haven't spoken in over a year with no re-engagement
- FAIL: When you don't have a specific role in mind ("Just refer me for anything")

### How to Ask (Make It Easy)

Every referral request must include:
1. **The specific role** — link to the posting, team name, req number if available
2. **Your resume** — attached or linked, already tailored to the role
3. **2-3 bullet points** they can use to recommend you — don't make them write the
   referral from scratch. Example:
   - "7 years building data pipelines at scale, most recently at [Company]"
   - "Led the migration from Hadoop to Spark, reducing processing time by 60%"
   - "Familiar with [Company]'s tech stack — used [specific tools] extensively"
4. **An easy out** — "No pressure at all. I completely understand if this isn't
   something you're able to do."

### Scripts by Relationship Depth

**Close Contact** (former manager, close colleague, mentor):
Direct and warm. Reference your relationship history. They already know your work.

**Acquaintance** (had 1-2 good conversations, connected professionally):
Reference your previous interaction(s) specifically. Provide more context since
they may not know your full background. Make the ask easy and low-pressure.

**Recent Connection** (just had an informational interview, met at an event):
This is the most delicate. Reference the specific conversation. Express genuine
interest in the company based on what you learned. Frame the ask as a natural
next step, not the reason you talked to them.

### If They Say No or Don't Respond

- **Silence after 5-7 business days:** Send ONE gentle follow-up. If still no
  response, move on. Do not send a third message.
- **Explicit no:** Thank them genuinely. "I completely understand — thanks for
  being upfront. I really appreciated our conversation regardless." Do NOT
  guilt-trip or argue.
- **Soft no** ("I'm not the right person" / "I don't know that team well"):
  Pivot to asking if they know someone who IS the right person. "Totally understand.
  Is there anyone on the [target] team you think might be open to a quick chat?"
- **Important:** A no to a referral is not a no to the relationship. Continue
  engaging with them professionally.

---

## Mode 5: LinkedIn Profile Optimization

Read `@references/linkedin-optimization.md` for the full guide.

Perform a comprehensive LinkedIn profile review. Your LinkedIn is your digital
first impression — 87% of recruiters use LinkedIn to evaluate candidates, and most
spend under 10 seconds on initial scan.

### Review Framework

Evaluate each section against the optimization guide. For each finding, provide:

```
SECTION: {{Section Name}}
STATUS: PASS / FAIL / WARNING
CURRENT: "{{What it says now or what's missing}}"
RECOMMENDED: "{{Specific rewrite or addition}}"
IMPACT: {{Why this change matters — e.g., "This headline will appear in search
         results for 'senior data engineer' queries"}}
```

### Sections to Review

1. **Profile Photo** — Professional, recent, clear face, neutral background.
   No group photos, no sunglasses, no cropped party pics.
2. **Headline** — Formula: [Target Role] | [Key Differentiator] | [Domain/Impact].
   NOT your current job title — that's what the experience section is for.
3. **About Section** — Mini cover letter: who you are, what you do, what you're
   looking for. Keyword-rich for recruiter search. Written in first person.
4. **Featured Section** — Showcase 3-5 items: top projects, published articles,
   conference talks, portfolio pieces. If empty, recommend what to add.
5. **Experience Section** — Should mirror resume structure but read more naturally.
   Each role needs measurable accomplishments, not just responsibilities.
6. **Skills Section** — Top 3 should be most-endorsed AND most-relevant to target
   role. Remove outdated or irrelevant skills. Aim for 25-50 skills total.
7. **Recommendations** — At least 3 from managers or senior colleagues. Coach the
   user on how to request them (offer to write a draft).
8. **Activity** — Are you posting, sharing, commenting? LinkedIn rewards activity
   with visibility. Recommend a sustainable engagement cadence.

### Output

Save the full profile review to `applications/linkedin-profile-review.md` with:
- Overall score (X/10)
- Section-by-section review with PASS/FAIL/WARNING
- Priority-ordered action items (highest impact first)
- Before/after examples for rewrites

---

## Mode 6: Warm Intro Chain Mapping

When you don't have a direct connection to the target, map multi-hop paths through
your network.

### How It Works

```
You -> {{Person A}} (your direct contact)
       -> {{Person B}} (A knows B, B is at target company)
          -> {{Person C}} (B knows C, C is the hiring manager)
```

### Step 1: Identify the Target

Who do you ultimately want to reach? The hiring manager, a team lead, an engineer
on the team, the internal recruiter?

### Step 2: Map Available Paths

For each path identified:
- **Hop count:** Fewer is better. 2 hops is ideal. 3+ hops have rapidly
  diminishing success rates.
- **Relationship strength at each hop:** Are these people who would actually make
  an introduction? A LinkedIn connection you've never spoken to won't forward your
  request.
- **Path viability score:**
  - 2 hops, strong relationships at each = HIGH viability
  - 2 hops, one weak link = MEDIUM viability
  - 3 hops or multiple weak links = LOW viability

### Step 3: Script Each Hop

For each introduction in the chain, generate:
- Message to your direct contact asking them to introduce you to Person B
- Suggested forwarding language they can use (make it copy-paste easy)
- What Person B should know about you before you're introduced

### Step 4: Parallel Path Strategy

Don't rely on a single chain. Map 2-3 viable paths and pursue them in parallel.
If Path A stalls at hop 2, Path B may still be progressing.

---

## Networking Tracker

Maintain a log of all networking activity. This is critical — networking without
tracking leads to dropped follow-ups and missed opportunities.

### Contact Log Format

```
CONTACT: {{Name}}
COMPANY: {{Company}} | ROLE: {{Role}}
CONNECTION: {{How you know them / how you found them}}
TIER: {{1 / 2 / 3}}
STATUS: {{Not contacted / Outreach sent / Connected / Conversation scheduled /
          Met / Follow-up sent / Referral requested / Referred / Gone cold}}

INTERACTION LOG:
- {{DATE}}: {{What happened — sent LinkedIn request, had coffee chat, etc.}}
- {{DATE}}: {{Next interaction}}

NEXT ACTION: {{What to do next and when}}
FOLLOW-UP DATE: {{When to follow up}}
NOTES: {{Anything relevant — what you discussed, their interests, potential value}}
```

### Tracker Behaviors

- When a user logs a new contact or interaction, update the tracker
- Proactively suggest follow-ups when they're due (check dates on load)
- Flag contacts that have gone cold (no interaction in 30+ days with no next action)
- Track conversion: which networking efforts led to interviews, referrals, or offers
- Summarize networking activity on request: X contacts reached, Y conversations held,
  Z referrals received

---

## Output

| Artifact | Save Location |
|----------|--------------|
| Connection maps | `applications/{{role-slug}}/networking/connection-map.md` |
| Outreach messages | `applications/{{role-slug}}/networking/outreach-messages.md` |
| General outreach (no specific role) | `networking/outreach-messages.md` |
| Informational interview prep | `applications/{{role-slug}}/networking/interview-prep.md` |
| Referral request materials | `applications/{{role-slug}}/networking/referral-request.md` |
| LinkedIn profile review | `applications/linkedin-profile-review.md` |
| Networking tracker | Updated in application tracker / `networking/tracker.md` |

---

## Data Flow

**Reads:**
- `career-profile` — work history (for shared background identification), skills
  (for relevant conversation topics), preferences (for target companies and roles),
  application tracker (for existing contacts and interaction history)

**Writes:**
- Outreach messages and connection maps to `applications/{{role-slug}}/networking/`
- LinkedIn profile review to `applications/linkedin-profile-review.md`
- Networking log entries to application tracker
- General networking materials to `networking/`
