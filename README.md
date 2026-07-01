# 🔄 Skill Updater

> Auto-audits and upgrades installed Claude skills on the first message of every chat.

---

## 🧠 What it does

Runs silently on the **first message of every new conversation**. Before answering the user's actual question, it:

1. 📋 **Inventories** every installed skill
2. 🔎 **Researches** current best practices per domain (web search)
3. 🧪 **Gap-analyzes** each skill against findings
4. ⬆️ **Upgrades** anything stale, incomplete, or missing capabilities
5. 📊 **Reports** — only if changes were made

---

## ⚙️ Update Cycle

| Phase | Action |
|---|---|
| 1️⃣ Inventory | List all skills, note purpose + estimated era |
| 2️⃣ Research | Web search `"[domain] best practices [year]"` per skill category |
| 3️⃣ Gap Analysis | Score: Current / Minor updates / Major revision / Obsolete |
| 4️⃣ Upgrade | Rewrite stale skills, preserve working logic, tighten prose |
| 5️⃣ Report | One-line summary — silent if nothing changed |

---

## 🎯 Trigger Conditions

- ✅ **Always** — first message of any new chat (silent background pass)
- ✅ **Explicit** — "update my skills", "audit my skills", "refresh skills"

---

## 🚦 Behavior Rules

- 🤫 **Zero overhead on zero changes** — no "I checked and everything's great!" filler
- 🚫 **Never blocks** the user's actual request — answer first, report after
- 📁 **Read-only respect** — installed skills at `/mnt/skills/` are never touched directly; upgrades save to `/mnt/user-data/outputs/skill-updates/`
- 🔬 **No hallucinated research** — only cites techniques actually found via search
- 1️⃣ **One report per chat** — runs once, not every message

---

## 📤 Example Output

**When updates are found:**
```
[Normal answer to user's question]

── Skill Update Report ───────────────────────────────
Skills audited: 12
Updated: xlsx, pdf-reading, github-push
No changes needed: 9 others
Download updated skills above ↑
─────────────────────────────────────────────────────
```

**When nothing needs updating:**
```
Paris.
```
*(No report appended — nothing changed, nothing to say.)*

---

## 📦 Install

```bash
npx skills add ./skill-updater
```

Or upload via **Claude.ai → Settings → Features → Custom Skills**.

---

<p align="center">Keeps your skill library current without you asking.</p>
