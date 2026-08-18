# Tool-Independent Portfolio Migration Plan

## Why This Exists

I built a large part of my portfolio using Cursor, but I no longer have access to the premium account I was using. The free tier is too limited for my workflow.

The goal is **not** to migrate from Cursor dependency to another AI-agent dependency.

The goal is to make the portfolio:

- independent of Cursor
- independent of Codex
- independent of Claude
- independent of any single AI product
- easy to back up
- easy to move
- understandable by me
- usable with normal developer tools
- safe to modify with or without AI

The portfolio itself should be the durable thing.

AI tools should remain optional, replaceable helpers.

---

# The Core Architecture

The recommended setup is:

```text
YOUR PORTFOLIO
│
├── Git repository              ← source of truth
├── GitHub remote               ← backup + history
├── README.md                   ← project overview
├── AGENTS.md                   ← optional AI instructions
├── docs/
│   ├── DESIGN_SYSTEM.md
│   ├── ARCHITECTURE.md
│   ├── CONTENT_GUIDE.md
│   └── DECISIONS.md
│
└── actual portfolio code
      │
      ├── Cursor
      ├── VS Code
      ├── Codex
      ├── Claude Code
      ├── future AI tools
      └── plain terminal
```

The important principle is:

> **Git is the foundation. AI is a detachable layer.**

My portfolio is not a “Cursor project.”

It is a normal code repository that can be opened by many different tools.

---

# Phase 1 — Rescue the Current Portfolio

Do not restructure the project yet.

First, locate the actual portfolio folder.

It will likely contain files or folders such as:

```text
package.json
src/
app/
public/
components/
.git/
```

Open the terminal inside the project and run:

```bash
git status
```

Possible outcomes:

### Outcome A — Git is already set up

You may see something like:

```text
On branch main
```

This is good.

The project is already a Git repository.

### Outcome B — Git is not set up

You may see:

```text
fatal: not a git repository
```

That is also fine.

Git can be initialized.

### Outcome C — Many modified files are listed

This probably means Cursor already changed many files that have not yet been saved into a Git commit.

That is useful information.

---

# Phase 2 — Create a Permanent Save Point

If Git is already active:

```bash
git add .
git commit -m "Portfolio snapshot before leaving Cursor"
```

A Git commit should be understood as:

> Save this exact state of my portfolio as a permanent checkpoint.

This is not AI memory.

This is not Cursor chat history.

It is an actual project snapshot.

If Git has never been initialized:

```bash
git init -b main
git add .
git commit -m "Initial portfolio snapshot"
```

Before committing, make sure sensitive information is not being included.

Common things that should usually remain private:

```text
.env
API keys
passwords
private credentials
```

These should normally be excluded using `.gitignore`.

---

# Phase 3 — Put the Repository Somewhere I Control

Create a private GitHub repository and push the project there.

The structure then becomes:

```text
MY COMPUTER
portfolio/

        +

GITHUB
portfolio/
```

This means there are at least two copies of the project.

If Cursor disappears tomorrow, the portfolio still exists.

If Codex disappears, the portfolio still exists.

If I change computers, I can clone the repository.

If I stop using AI entirely, the portfolio still works.

---

# GitHub Desktop Is Recommended

If Git commands feel intimidating, use GitHub Desktop.

The mental model is:

```text
Portfolio folder
      ↓
GitHub Desktop
      ↓
Commit changes
      ↓
Push
      ↓
Private GitHub repository
```

The three Git concepts I actually need to understand are:

```text
COMMIT
"Save this version"

PUSH
"Back up my commits to GitHub"

RESTORE
"Go back if something breaks"
```

I do not need to become a Git expert.

---

# Phase 4 — Move Important Knowledge Out of Cursor

A major source of lock-in is when important knowledge lives only inside:

```text
Cursor chats
Cursor rules
Agent conversations
my memory
```

That knowledge should move into the repository.

Create:

```text
docs/
├── DESIGN_SYSTEM.md
├── ARCHITECTURE.md
├── CONTENT_GUIDE.md
└── DECISIONS.md
```

---

# DESIGN_SYSTEM.md

This file should describe the visual system of the portfolio.

Examples:

```text
Typography
Colors
Spacing
Grid
Breakpoints
Motion
Image treatment
Components
```

This should explain the important visual rules so they do not need to be rediscovered every time.

---

# ARCHITECTURE.md

This file should describe how the project is structured.

Examples:

```text
Where pages live
Where components live
How case studies work
How images are organized
What should be reused
What should not be changed casually
```

The goal is for a human or AI tool to understand the project without relying on old chats.

---

# CONTENT_GUIDE.md

This file should describe content conventions.

Examples:

```text
Case study structure
Writing style
Project metadata
Image naming
How project cards work
Tone of voice
```

This keeps content decisions separate from implementation details.

---

# DECISIONS.md

This file records important decisions and why they were made.

Example:

```md
## 2026-08-16 — Case study width

Decision:
Case-study text uses a narrower maximum width than project imagery.

Why:
This improves reading comfort while preserving visual impact.

Do not:
Make all content full-width just to simplify CSS.
```

This file is valuable because it preserves **reasoning**, not just implementation.

A future human or AI can understand why something exists.

---

# Phase 5 — Keep AI Instructions Optional

Create a lightweight:

```text
AGENTS.md
```

This should not contain the only source of truth.

It should point agents toward the human-readable documentation.

Example:

```md
# Portfolio Agent Instructions

Read before modifying this project:

- docs/DESIGN_SYSTEM.md
- docs/ARCHITECTURE.md

The owner is a product designer with limited engineering experience.

When making changes:

- Preserve the existing design system.
- Reuse components before creating new ones.
- Avoid large refactors unless required.
- Do not add dependencies casually.
- Make the smallest clean change.
- Verify desktop and mobile behavior.
- Explain major changes in plain language.
```

If a future AI tool does not automatically recognize `AGENTS.md`, I can simply say:

> Read `AGENTS.md` and the `/docs` folder before making changes.

This avoids tool lock-in.

---

# Phase 6 — Use a Normal Editor as the Baseline

Install VS Code alongside Cursor.

The reason is not that VS Code is a better AI tool.

The reason is that it is a normal, widely supported code editor.

The setup becomes:

```text
                 MY REPOSITORY
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
      VS Code     Cursor       Codex
        │                         │
    manual edits              AI edits

        └───────────┬───────────┘
                    ↓
                   Git
                    ↓
                  GitHub
```

The project should still work if every AI tool is removed.

That is the test for independence.

---

# Recommended Role of Each Tool

## VS Code

Use for:

- browsing files
- small manual edits
- normal project work
- inspecting code
- running the project
- staying independent of AI

## ChatGPT

Use for:

- design thinking
- UX critique
- writing case studies
- understanding technical concepts
- planning architecture
- reviewing prompts before implementation
- translating technical language into product-design language

## Codex / Claude Code / Cursor Agent

Use only when useful for:

- modifying many files
- implementing a feature
- debugging
- refactoring
- searching a large codebase
- running tests
- fixing implementation issues

These agents are optional workers, not owners of the project.

---

# The Correct Hierarchy

The ideal hierarchy is:

```text
ME
│
├── Design decisions
├── Portfolio documentation
│
└── Git repository
       │
       ├── VS Code
       ├── Cursor
       ├── Codex
       ├── Claude
       └── future tools
```

Avoid this:

```text
Cursor
  ↓
my portfolio
```

Also avoid:

```text
Codex
  ↓
my portfolio
```

The AI should sit **below the repository**, not above it.

---

# Recommended Workflow Before AI Changes

Suppose I want to redesign the homepage hero.

Use this workflow:

```text
1. Confirm the current portfolio works.

2. Commit the current state:
   "Homepage before hero redesign"

3. Think through the design direction.

4. Use ChatGPT for critique or planning if useful.

5. Ask an AI coding agent:
   "Implement this direction."

6. Review the actual site.

7a. If good:
    Commit:
    "Redesign homepage hero"

7b. If bad:
    Revert or restore the previous version.
```

This makes AI experimentation reversible.

---

# Before Giving an AI a Large Task

First run:

```bash
git status
```

Then create a clean checkpoint.

After the AI finishes:

```text
Review the site
Review the changed files
Check desktop
Check mobile
Check for broken behavior
```

If it looks correct:

```text
Commit it.
```

If it looks wrong:

```text
Discard or restore the changes.
```

The important idea is:

> Never let an AI change a large amount of working code without first having a safe checkpoint.

---

# Migration Checklist

Do these in order:

- [ ] Do not delete Cursor yet.
- [ ] Find the current portfolio project folder.
- [ ] Run `git status`.
- [ ] Confirm whether Git is already initialized.
- [ ] Check that secrets such as `.env` files are not being committed.
- [ ] Create a Git snapshot of the current working portfolio.
- [ ] Create a private GitHub repository.
- [ ] Push the portfolio to GitHub.
- [ ] Install GitHub Desktop if command-line Git feels uncomfortable.
- [ ] Install or open VS Code.
- [ ] Open the same portfolio folder in VS Code.
- [ ] Verify the portfolio runs outside Cursor.
- [ ] Create `/docs`.
- [ ] Create `DESIGN_SYSTEM.md`.
- [ ] Create `ARCHITECTURE.md`.
- [ ] Create `CONTENT_GUIDE.md`.
- [ ] Create `DECISIONS.md`.
- [ ] Create a lightweight `AGENTS.md`.
- [ ] Move important Cursor-specific knowledge into the repository documentation.
- [ ] Keep Cursor only as an optional editor.
- [ ] Try Codex or Claude only as optional implementation tools.
- [ ] Continue committing before and after meaningful changes.

---

# Long-Term Goal

The final portfolio should satisfy this test:

> If Cursor, ChatGPT, Codex, Claude, or any other AI tool disappeared tomorrow, could I still open my portfolio, run it, understand its structure, edit it, and recover an older version?

If the answer is yes, the portfolio is properly independent.

---

# Final Principle

The durable assets are:

```text
My code
My Git history
My documentation
My design decisions
My content
My assets
```

The disposable tools are:

```text
Cursor
Codex
Claude
ChatGPT
future AI agents
```

The portfolio belongs to me.

The tools are replaceable.
