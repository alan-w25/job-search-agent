# Career Profile Schema

This is the canonical schema for the career profile. All job search skills read from
this structure. Store as JSON in persistent storage under key `career-profile`.

```json
{
  "meta": {
    "last_updated": "2026-04-15T12:00:00Z",
    "version": 1,
    "completeness_score": 0,
    "completeness_breakdown": {
      "identity": 0,
      "work_history": 0,
      "skills": 0,
      "education": 0,
      "star_stories": 0,
      "preferences": 0,
      "reputation": 0
    }
  },

  "identity": {
    "name": "",
    "email": "",
    "phone": "",
    "location": "",
    "linkedin_url": "",
    "github_url": "",
    "portfolio_url": "",
    "other_links": [],
    "professional_summary": "",
    "elevator_pitch": ""
  },

  "work_history": [
    {
      "id": "work_1",
      "company": "",
      "company_description": "",
      "company_size": "",
      "company_stage": "",
      "industry": "",
      "title": "",
      "start_date": "",
      "end_date": "",
      "is_current": false,
      "location": "",
      "remote_status": "",
      "team_size": 0,
      "direct_reports": 0,
      "reporting_to_title": "",
      "responsibilities": [""],
      "key_achievements": [
        {
          "id": "ach_1",
          "description": "",
          "quantified_impact": "",
          "technologies_used": [],
          "is_resume_worthy": true
        }
      ],
      "reason_joined": "",
      "reason_left": "",
      "skills_used": [],
      "tools_used": [],
      "methodologies": [],
      "biggest_challenge": "",
      "proudest_moment": ""
    }
  ],

  "skills": {
    "expert": [],
    "proficient": [],
    "working_knowledge": [],
    "familiar": [],
    "soft_skills": [],
    "domain_expertise": []
  },

  "education": [
    {
      "id": "edu_1",
      "institution": "",
      "degree": "",
      "field_of_study": "",
      "graduation_date": "",
      "gpa": "",
      "relevant_coursework": [],
      "key_learnings_still_used": "",
      "honors_awards": []
    }
  ],

  "certifications": [
    {
      "name": "",
      "issuer": "",
      "date_earned": "",
      "expiry_date": "",
      "credential_id": ""
    }
  ],

  "projects": [
    {
      "id": "proj_1",
      "name": "",
      "description": "",
      "role": "",
      "technologies": [],
      "outcome": "",
      "url": "",
      "is_portfolio_piece": true,
      "source_experience_id": ""
    }
  ],

  "star_stories": [
    {
      "id": "star_1",
      "title": "Short memorable label",
      "category": "leadership|technical|conflict|failure|collaboration|initiative|pressure|customer",
      "situation": "",
      "task": "",
      "action": "",
      "result": "",
      "source_experience_id": "work_1",
      "skills_demonstrated": [],
      "industries_relevant_to": [],
      "strength": "strong|medium|needs_work",
      "interview_ready": true,
      "times_used": 0,
      "last_used_for": ""
    }
  ],

  "preferences": {
    "target_roles": [],
    "target_industries": [],
    "target_companies": [],
    "ideal_company_size": "",
    "ideal_company_stage": "",
    "preferred_work_style": "",
    "energizers": [],
    "drainers": [],
    "must_haves": [],
    "nice_to_haves": [],
    "dealbreakers": [],
    "salary_range": { "min": 0, "max": 0, "currency": "USD" },
    "equity_preference": "",
    "location_preferences": [],
    "remote_preference": "",
    "visa_status": "",
    "availability": "",
    "notice_period": ""
  },

  "reputation": {
    "known_for": [],
    "colleagues_come_to_me_for": [],
    "manager_would_say": "",
    "career_goals_2_3_years": "",
    "long_term_vision": "",
    "values": []
  },

  "application_tracker": [
    {
      "id": "app_1",
      "job_id": "",
      "company": "",
      "role": "",
      "date_applied": "",
      "status": "applied|phone_screen|technical|onsite|offer|rejected|withdrawn",
      "match_score": 0,
      "resume_version_used": "",
      "contacts": [],
      "notes": [],
      "interview_dates": [],
      "follow_up_dates": [],
      "outcome": ""
    }
  ]
}
```

## Key Design Decisions

1. **IDs everywhere**: Every work entry, achievement, story, and project has an ID so
   other parts of the system can cross-reference (e.g., a STAR story links to its
   source work experience).

2. **Application tracker is in the profile**: This creates a feedback loop — interview
   outcomes inform which STAR stories work, which skills are in demand, etc.

3. **Strength ratings on STAR stories**: "strong" stories get prioritized in interview
   prep; "needs_work" stories get flagged for the user to develop.

4. **Preferences are detailed**: Because match scoring uses them — dealbreakers
   automatically disqualify roles, must-haves heavily weight the score.

5. **Completeness breakdown is granular**: Each section has its own score so the user
   knows exactly where to invest more time.
