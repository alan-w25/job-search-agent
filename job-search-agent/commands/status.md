---
description: Show your career profile completeness and application tracker status
---

# Job Search Agent — Status

Check the current state of the user's job search.

## Steps

1. Load career profile from persistent storage (key: `career-profile`)
2. If no profile exists: "No profile found yet. Run **/quick-start** or **/career-profile-builder** to get started."
3. If profile exists, show:

```
YOUR PROFILE
Completeness: [X]%
Roles captured: [N]
STAR stories: [N] ([N] strong, [N] medium, [N] needs work)
Last updated: [date]

APPLICATIONS
[Company] — [Role] — [Status] — Match: [Score]
[Company] — [Role] — [Status] — Match: [Score]
...

SUGGESTED NEXT STEP
[Based on what's missing or where they are in the pipeline]
```

4. Also check for any `applications/` folders in the current working directory and list saved outputs.
