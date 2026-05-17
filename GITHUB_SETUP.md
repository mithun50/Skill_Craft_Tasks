# GitHub Setup Guide

This document explains how to clone, navigate, and contribute to this repository.

---

## Prerequisites

- Git installed on your machine. Download from https://git-scm.com
- A GitHub account
- GitHub CLI (optional but recommended). Download from https://cli.github.com

---

## Cloning the Repository

Using HTTPS:

```bash
git clone https://github.com/mithun50/Skill_Craft_Tasks.git
```

Using SSH (if you have SSH keys configured):

```bash
git clone git@github.com:mithun50/Skill_Craft_Tasks.git
```

Using GitHub CLI:

```bash
gh repo clone mithun50/Skill_Craft_Tasks
```

---

## Repository Structure

```
Skill_Craft_Tasks/
|
|-- SCT_PE_1/
|   └── README.md          # Task 01: Writing Better Prompts
|
|-- SCT_PE_2/
|   └── README.md          # Task 02: Prompting for Creativity
|
|-- SCT_PE_3/
|   └── README.md          # Task 03: Prompting for Task Automation
|
|-- SCT_PE_4/
|   └── README.md          # Task 04: Simulating an Assistant
|
|-- README.md              # Repository overview
|-- LICENSE                # MIT License
|-- .gitignore             # Files excluded from version control
|-- .gitattributes         # Line ending and binary file rules
└── GITHUB_SETUP.md        # This file
```

---

## Navigating the Tasks

Each task lives in its own folder. Open the README.md inside any task folder to read the full documentation, prompts, examples, and reflections for that task.

```bash
# Navigate to a task folder
cd SCT_PE_1

# Open the README
cat README.md
```

---

## Viewing Commit History

To see the full commit history with dates:

```bash
git log --oneline --graph --decorate
```

To see a specific commit in detail:

```bash
git show <commit-hash>
```

---

## Branching Convention

This repository uses a single `main` branch since it is a documentation-only internship project. If you fork and want to suggest changes, create a branch named after the task:

```bash
git checkout -b update/SCT_PE_1
```

---

## Making Changes (for contributors or forks)

1. Fork the repository on GitHub
2. Clone your fork locally
3. Create a new branch for your changes
4. Make your edits
5. Commit with a clear message
6. Push to your fork
7. Open a pull request against the original repository

```bash
git add .
git commit -m "SCT_PE_X: describe your change"
git push origin update/SCT_PE_X
```

---

## Useful Git Commands

| Command | Description |
|---------|-------------|
| `git status` | Show changed files |
| `git log --oneline` | Compact commit history |
| `git diff` | Show unstaged changes |
| `git pull` | Fetch and merge latest changes |
| `git stash` | Temporarily save uncommitted changes |

---

## GitHub CLI Quick Reference

```bash
# View repository info
gh repo view

# List all commits
gh api repos/mithun50/Skill_Craft_Tasks/commits --jq '.[].commit.message'

# Open repository in browser
gh repo view --web
```

---

## Contact

For questions about this repository, open an issue on GitHub or reach out via the profile at https://github.com/mithun50 or by email at mithungowda.b7411@gmail.com
