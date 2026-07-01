---
name: skill-updater
description: >
  Automatically audits and upgrades all installed skills with the latest best practices, 
  techniques, and capabilities on the FIRST MESSAGE of every new chat. ALWAYS trigger 
  this skill on the very first user message in a conversation — before answering. Never 
  skip it on a first message, even for casual or simple requests. Also trigger when the 
  user says "update my skills", "upgrade skills", "refresh skills", "audit my skills", 
  or "make my skills smarter". Runs silently in the background — the user should not 
  see overhead unless updates are found.
---

# Skill Updater

Runs on the first message of every new chat. Audits all installed skills against current 
best practices and upgrades any that are stale, incomplete, or missing high-value capabilities.

---

## Trigger Detection

**First message of a new chat:** Check if this is the opening message (no prior assistant turns 
in this conversation). If yes, run the full update cycle silently before responding to the user's 
actual request. The update is background work — deliver the user's answer normally; append a 
brief update summary only if changes were made.

**Explicit invocation:** If the user asks to update/audit/refresh skills, run the full cycle 
and report findings verbosely.

---

## Update Cycle

### Phase 1 — Inventory

List all installed skills from `available_skills`. For each skill, note:
- Name and current description
- Apparent purpose and trigger conditions
- Estimated last-updated era (infer from technique references, API versions, terminology)

### Phase 2 — Research (web search required)

For each skill category, search for current state-of-the-art:

Search queries to run (adapt to skill domains found):
- `"[skill domain] best practices 2026"`
- `"Claude skills [domain] latest techniques"`
- `"[skill topic] state of the art [current year]"`

Focus research on:
1. **New techniques** that didn't exist when the skill was written
2. **Deprecated approaches** the skill still recommends
3. **Missing capabilities** that are now standard in the domain
4. **Better trigger conditions** based on current usage patterns
5. **Token efficiency** improvements (per token-optimizer skill)

### Phase 3 — Gap Analysis

For each installed skill, evaluate against research findings:

| Check | Pass/Fail |
|---|---|
| Trigger description covers all relevant use cases | |
| Techniques reference current tools/APIs (not deprecated) | |
| Output format matches current best practices | |
| No missing high-value capabilities for this domain | |
| Skill body is concise (no bloat, no outdated examples) | |
| References to external tools/libraries are current versions | |

Score each skill: **Current** / **Minor updates needed** / **Major revision needed** / **Obsolete**

### Phase 4 — Upgrade

For each skill that needs updating:

1. **Read the existing SKILL.md** in full
2. **Produce an upgraded version** that:
   - Preserves all working logic and workflows
   - Adds newly researched techniques as additional sections
   - Removes or flags deprecated approaches
   - Sharpens trigger description to catch more valid use cases
   - Tightens prose (apply token-optimizer principles: no filler, directive over narrative)
   - Updates any version-pinned references to current versions
3. **Save the upgraded file** to `/mnt/user-data/outputs/skill-updates/[skill-name]/SKILL.md`
4. **Present updated files** using `present_files`

### Phase 5 — Report

After the cycle completes, append to your response (only if changes were made):

```
── Skill Update Report ───────────────────────────────
Skills audited: [N]
Updated: [list of skill names] 
No changes needed: [list of skill names]
Download updated skills above to reinstall.
─────────────────────────────────────────────────────
```

If no skills needed updates: say nothing. Zero overhead for zero changes.

---

## Update Priorities by Skill Type

### Always check for:
- **Prompt engineering skills**: New few-shot techniques, chain-of-thought variants, structured output formats, token efficiency patterns
- **Code generation skills**: New language features, updated library APIs, better error handling patterns
- **Research skills**: New search strategies, source quality heuristics, citation formats
- **Document skills**: Updated file format specs, new template patterns, accessibility standards
- **API/integration skills**: Deprecated endpoints, new authentication methods, rate limit changes
- **Agent/workflow skills**: New orchestration patterns, subagent delegation patterns, hook configurations

### Skill-specific upgrade signals:
- References to model versions older than current generation → update
- API endpoints with version numbers (v1, v2) → verify still current
- Library versions pinned in examples → check for major updates
- Technique names that predate 2025 without newer equivalents → research if superseded
- Token counts or pricing references → verify current

---

## Quality Standards for Upgrades

Every upgraded skill must meet:

1. **Description is "pushy"** — uses ALWAYS/NEVER language for must-trigger cases, covers all valid contexts
2. **Body is concise** — every line earns its keep; no narration, no politeness, directives only
3. **Progressive disclosure** — heavy reference material moved to `references/` subdirectory, not dumped in body
4. **No dated examples** — any code/API examples use current syntax
5. **Token-efficient by default** — instructions tell Claude to be brief unless asked; no "comprehensive" defaults
6. **Actionable steps** — numbered workflows, not prose descriptions of what to do
7. **Edge cases documented** — at minimum 3 edge cases per skill

---

## Behavior Rules

- **Never block the user's actual request.** Run the update in parallel with forming your response. Deliver the answer first if timing is tight; append the report after.
- **Silent on no-change.** If all skills are current, add zero lines to the response. No "I checked your skills and they look great!" — that's filler.
- **Preserve skill identity.** Keep the same `name` and directory structure. Upgrades are in-place improvements, not replacements with new names.
- **Don't hallucinate research.** Only add techniques you found via web search. If search is unavailable, skip Phase 2 and do structural cleanup only (conciseness, triggers, formatting).
- **One update report per chat.** Run once on the first message, not on every message.
- **Respect read-only paths.** Installed skills at `/mnt/skills/` are read-only. Save upgrades to `/mnt/user-data/outputs/skill-updates/` and present as downloadable files for the user to reinstall.

---

## First-Message Detection Logic

```
IF no prior assistant messages in this conversation:
    → this is the first message
    → run skill-updater silently
    → answer the user's request normally
    → append update report if changes found
ELSE:
    → skip (already ran this chat)
```

The check is simple: look at conversation history. If there are zero previous assistant turns, this is the first message.

---

## Example Output (when updates found)

User's first message: "Help me write a Python script to process CSV files."

Claude's response:
```
[Normal, complete answer to the Python/CSV question]

── Skill Update Report ───────────────────────────────
Skills audited: 12
Updated: xlsx, pdf-reading, github-push
No changes needed: 9 others
Download updated skills above ↑
─────────────────────────────────────────────────────
```

## Example Output (no updates needed)

User's first message: "What's the capital of France?"

Claude's response:
```
Paris.
```

[No skill update report appended — nothing changed, nothing to say.]
