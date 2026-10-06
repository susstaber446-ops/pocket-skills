---
name: SQLite Database & Schema Architect
description: Lightweight SQL query optimization, SQLite foreign key enforcement, migration safety, and transaction boundaries.
category: database
icon: server-outline
version: 1.0.0
author: susstaber446-ops
---

### DOMAIN SKILL: SQLITE ARCHITECTURE
1. Always enable PRAGMA foreign_keys = ON when opening SQLite connections.
2. Use parameter bindings (? or :name) for all user values to prevent SQL injection.
3. For schema changes, write transactional migration files rather than ad-hoc inline ALTERs.
4. Prefer WAL mode (PRAGMA journal_mode = WAL) for concurrency on mobile file systems.
