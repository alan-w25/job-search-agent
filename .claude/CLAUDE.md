# Job Search Agent — Project Instructions

House style and conventions for editing skills, commands, and references.

## File Organisation

- **Skills** live under `job-search-agent/skills/{skill-name}/` with a `SKILL.md` and `references/` folder.
- **Commands** live under `job-search-agent/commands/`.
- **Agents** live under `job-search-agent/agents/`.
- **References** are loaded with `@references/filename.md` from within the owning skill.
- **Shared references** that multiple skills need are duplicated into each skill's `references/` folder. Skills should be self-contained.

## Content Rules

- **Never invent user data.** Use `[PLACEHOLDER]` or `{{LIKE_THIS}}` for missing info.
- **Cite sources** for all factual claims in research output with URLs and access dates.
- **No emojis** in skill output. Use text labels (PASS / FAIL / WARNING).
- **Address the user as "you"**, not by name: "Your resume highlights..." not "John's resume highlights..."
- **Quantify impact** wherever possible. "Improved pipeline efficiency by 40%" not "improved pipeline efficiency."

## Skill Conventions

- Every `SKILL.md` starts with YAML frontmatter: `name`, `description` (required), `tags` (optional).
- Each skill lists its capabilities in a table at the top.
- Each skill specifies what it reads from and writes to persistent storage.
- Reference files are loaded only when the relevant capability is invoked, not all at once.
- Output files are saved to `applications/{role-slug}/` for role-specific deliverables.
