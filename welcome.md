 # Welcome to Your Vault

Your personal AI workspace. It's private, it's yours, and it's here to make your day easier.

---

## What Is This?

Think of it like a personal assistant that knows how you work. It knows your preferences, your tools, and what you don't want it to touch. The more you use it, the more useful it gets.

| Your Vault | LS-Brain |
|------------|----------|
| Private to you | Shared with everyone |
| Experiment freely | Has to work for the org |
| Break things, learn | Stable and validated |

---

## Quick Start (2 minutes)

1. Open your terminal
2. Navigate to your vault folder
3. Type `claude`
4. Type `/daily-note`

That's it. You just created your first daily note.

---

## What's In Here

```
CLAUDE.md                → How AI knows you (preferences, guardrails)
.claude/commands/        → Your skills (things AI can do for you)
templates/               → Reusable formats
outputs/                 → Files AI creates for you
Daily/                   → Your daily notes
```

**The most important file is `CLAUDE.md`.** It tells AI how to work with you. Your manager built it from your interview. You can change it anytime.

---

## Your First Week

| When | What to Do |
|------|-----------|
| Day 1 | Run `/daily-note`. Look around. Get comfortable. |
| Day 2-3 | Use `/daily-note` at the start and end of each day. |
| End of Week 1 | Notice what feels repetitive in your work. That's a skill idea. |
| Week 2 | Tell your manager or post in #brain about the skill idea. |

---

## Skills You Have

| Skill | What It Does |
|-------|-------------|
| `/daily-note` | Creates today's note, carries forward yesterday's open items |
| `/daily-note recap` | End of day summary of what got done |

More skills can be added as you find things to automate.

---

## Building Your Own Skills

When you notice something you do over and over, that's a skill waiting to happen. Two ways to build one:

**Path A (DIY):** Copy `templates/skill-template.md` into `.claude/commands/`, fill it in, test it.

**Path B (Get Help):** Describe the friction to your manager or post in #brain. Someone will help you build it.

---

## Help

- **Vault questions:** Ask your manager or post in #brain on Slack
- **Claude help:** Type `/help` in Claude
- **Something broke:** Don't panic. Type `git log` to see history, `git checkout .` to undo changes.

---

*Your vault was set up on: {{date}}*