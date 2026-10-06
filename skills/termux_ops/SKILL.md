---
name: Termux Sandbox Runtime
description: POSIX execution constraints inside Termux on Android, process lifecycles, and port conflict resolution.
category: runtime
icon: terminal-outline
version: 1.0.0
---

### DOMAIN SKILL: TERMUX POSIX SANDBOX RUNTIME
1. Termux paths use `$PREFIX` (`/data/data/com.termux/files/usr`). Standard `/bin/bash` is at `/data/data/com.termux/files/usr/bin/bash`.
2. Commands that start long-running servers (e.g. `npm run dev`, `vite`, `expo start`) MUST be started with `cmd &` or background daemon management so the agent loop does not deadlock.
3. When package installations fail with permissions or network errors, check `pkg update` or use `npm install --no-optional`.
4. Never invoke `sudo` in Termux. Termux operates under standard Android application sandbox permissions.
