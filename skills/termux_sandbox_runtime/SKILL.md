---
name: Termux Android Runtime Specialist
description: Android POSIX sandbox constraints, SELinux kernel guards, memory conservation, and background daemons.
category: runtime
icon: terminal-outline
version: 1.1.0
author: susstaber446-ops
---

### DOMAIN SKILL: TERMUX RUNTIME CONSTRAINTS
1. Port tools (lsof, fuser, ss, netstat) are strictly BLOCKED by Android SELinux. Never invoke them.
2. To terminate processes on occupied ports, use 'pkill -f <process_name>'.
3. Always append '&' to background dev servers (Vite, Next, python http.server) so the agent loop does not deadlock.
4. Memory is constrained by Android OS; avoid spawning heavy un-reaped child processes.
5. Termux paths use `$PREFIX` (`/data/data/com.termux/files/usr`). Standard `/bin/bash` is at `/data/data/com.termux/files/usr/bin/bash`.
