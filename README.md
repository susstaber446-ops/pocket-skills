# Pocket IDE • Central Agent Domain Skills Repository

Welcome to your central repository for **Autonomous Agent Domain Skills** in [Pocket IDE](https://github.com/aqibmehedi007/Pocket_IDE).

## How Pocket IDE Discovers this Repo
Pocket IDE's **Pocket Store Hub (Skills tab)** automatically detects this repository on your GitHub account (`pocket-skills`).
It traverses `skills/*/SKILL.md` and parses the YAML frontmatter and modular system prompt instructions.

## Active Skills in this Repository

| Skill ID | Category | Description |
| :--- | :--- | :--- |
| **react_native_expert** | `framework` | Production React Native, Expo SDK, safe area, and Hermes rules |
| **termux_sandbox_runtime** | `runtime` | Termux Android POSIX constraints, background ports, and apt |
| **android_cli_master** | `system` | Android SDK, ADB commands, Gradle builds, and logcat |
| **git_workflow_hygiene** | `git` | Atomic git commits, rebasing, stash hygiene, and branch safety |
| **sqlite_db_architect** | `database` | SQLite foreign keys, query indexing, WAL mode, and migrations |

## How to Add a New Skill
Create a folder `skills/<skill_name>/SKILL.md` containing:
```markdown
---
name: Your Skill Name
description: Short summary of what this skill teaches the agent
category: framework
icon: flash-outline
version: 1.0.0
---

### DOMAIN INSTRUCTIONS: YOUR DOMAIN
1. Step-by-step authoritative rules...
```
Pocket IDE will immediately pick it up and present a **GET** / **Enable** button in your mobile Pocket Store!
