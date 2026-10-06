---
name: SQLite Database Architect
description: Lightweight SQL optimization, PRAGMA foreign keys, parameter binding, and WAL concurrency mode.
category: database
icon: server-outline
version: 1.0.0
---

### DOMAIN SKILL: SQLITE ARCHITECTURE
1. Always enable `PRAGMA foreign_keys = ON;` upon opening every SQLite connection.
2. Always enable WAL mode (`PRAGMA journal_mode = WAL;`) for high concurrency and zero reader locks on mobile storage.
3. Use parameter bindings (? or :name) for all user values to prevent SQL injection vulnerabilities.
4. Write database migrations as numbered SQL files (e.g., `001_init.sql`, `002_add_index.sql`) rather than mutating schemas in-place.
