# IC Vault Kit

Everything you need for your personal AI workspace.

## Setup (5 minutes)

### 1. Create your vault folder

```
mkdir my-vault
cd my-vault
```

### 2. Create the structure

```
mkdir -p .claude/commands templates outputs Daily
```

### 3. Copy files

From this kit into your vault:

```
cp CLAUDE-template.md my-vault/CLAUDE.md
cp AGENTS-template.md my-vault/.claude/commands/AGENTS.md
cp welcome.md my-vault/welcome.md
cp commands/daily-note.md my-vault/.claude/commands/daily-note.md
cp templates/* my-vault/templates/
```

### 4. Personalize CLAUDE.md

Open `CLAUDE.md` and fill in your info. Your manager has a profile from your interview that has everything you need.

### 5. Start git (recommended)

```
cd my-vault
git init
git add .
git commit -m "Initial vault setup"
```

### 6. Start using it

```
claude
/daily-note
```

---

## What's Next

Read `welcome.md` for the full guide.

---

*Kit version: 2026-02-17*
