# JD Analysis Framework

## Parsed JD Structure

When analyzing a job description, extract into this structure:

```json
{
  "job_id": "jd_[timestamp]",
  "raw_text": "",
  "parsed": {
    "title": "",
    "company": "",
    "location": "",
    "remote_policy": "remote|hybrid|onsite|not_specified",
    "seniority_level": "intern|entry|mid|senior|staff|principal|director|vp|c_suite",
    "department": "",
    "reports_to": "",
    "team_size_hints": "",
    "compensation": {
      "salary_range": "",
      "equity": "",
      "bonus": "",
      "benefits_highlights": []
    },
    "requirements": {
      "must_have_skills": [
        { "skill": "", "years": 0, "category": "technical|domain|soft|credential" }
      ],
      "nice_to_have_skills": [],
      "education_requirement": "",
      "experience_years_required": "",
      "experience_years_preferred": ""
    },
    "responsibilities": [],
    "day_in_the_life": "",
    "success_metrics": [],
    "growth_path": "",
    "culture_signals": [],
    "red_flags": [],
    "green_flags": [],
    "hidden_signals": [],
    "negotiation_intel": []
  },
  "analysis": {
    "match_score": 0,
    "score_breakdown": {},
    "gaps": [],
    "strengths": [],
    "competitive_positioning": {},
    "win_condition": "",
    "recommended_action": ""
  }
}
```

## Skill Categorization Rules

When comparing JD skills to profile skills:

| JD Requirement | Profile Level Needed | Match? |
|----------------|---------------------|--------|
| "Expert in X" / "deep experience" | Expert or Proficient | Yes if proficient+ |
| "Experience with X" / "proficiency in" | Proficient or Working Knowledge | Yes |
| "Familiarity with X" / "exposure to" | Any level including Familiar | Yes |
| "Nice to have: X" | Any level | Bonus points, not required |
| "X years of Y" | Check work history dates + skill level | Within ±2 years is fine |

## Industry Inference Rules

If the JD doesn't explicitly state the industry, infer from:
- Company name → look up if known
- Tech stack clues (HIPAA = healthcare, PCI = finance, etc.)
- Domain vocabulary ("underwriting" = insurance/finance, "clinical" = healthcare)
- Regulatory references (SOX, GDPR, FDA, etc.)

## Seniority Calibration

Job titles are inconsistent across companies. Calibrate seniority by looking at:
- Years of experience requested (not just title)
- Scope of responsibility (IC vs. people management vs. strategy)
- Decision-making authority implied
- Budget/P&L ownership mentioned
- "Cross-functional leadership" usually means Staff+ or Director+
- "Mentoring junior engineers" usually means Senior
- "Defining technical strategy" usually means Staff/Principal

## Red Flag Severity Scoring

- **Critical** (might want to skip): Multiple critical red flags, compensation well below
  market for the seniority, role has been reposted 3+ times
- **Moderate** (proceed with awareness): Single red flags, vague scope, no clear reporting
  structure — worth probing in the interview
- **Minor** (normal): Slightly inflated requirements (industry standard), corporate jargon,
  generic language about culture
