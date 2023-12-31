---
title: SQLite
description: A guide to using SQLite, a popular open-source self-contained relational database management system. It may include instructions on how to create and manage databases, tables, and queries using the SQLite3 shell or other interfaces, as well as best practices for optimizing performance and securing the database.
tags:
  - SQL
  - SQLite
  - Database_Management_System
  - Data_Management
  - Lightweight_DBMS
aliases:
---
# SQLite Cheat-Sheet

SQLite is a relational database contained in a C library. In contrast to many other databases, SQLite is not a client-server database engine. Rather, it's embedded into an end program.

SQLite generally follows the PostgreSQL ([[postgres]]) syntax but does not enforce type checking.

You can open a SQLite Database with `sqlite3 <filename>` directly.

## Commands

`.help` Shows all commands `.databases` Show all existing databases `.quit` Exists `.tables` Shows all tables `.backup` Backups current database