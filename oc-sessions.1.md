---
title: oc-sessions
section: 1
number: 1
header: General Commands Manual
footer: oc-sessions
date: November 2026
author: Rabie Jradi <rabie.j@pm.me>
---

## NAME

oc-sessions — list recent opencode sessions with ready-to-run launch commands

## SYNOPSIS

**oc-sessions** [*OPTIONS*] [*COUNT*]

## DESCRIPTION

**oc-sessions** lists your recent *opencode* sessions, newest first, together
with the `opencode --session=...` command needed to reopen each one. Sessions
are persisted by opencode in an SQLite database
(`~/.local/share/opencode/opencode.db`), so the listing works even after a
reboot or a lost terminal.

The `directory` column is informational: when a full session ID is passed,
opencode resolves the project from the session itself, so you can launch from
any directory.

## OPTIONS

`-a`, `--all`
: Include child/subagent sessions (they are hidden by default).

`-f`, `--format` *FORMAT*
: Output format: `pretty`, `tsv`, `json`, or `cmd` (see FORMATS).

`-h`, `--help`
: Show help.

*COUNT*
: Number of sessions to list (default 10).

## FORMATS

`pretty`
: Human-readable listing with launch commands (default).

`tsv`
: One row per session, `|`-separated: `updated|title|id|directory`.

`json`
: JSON array of `{updated, title, id, directory}` objects.

`cmd`
: Only the `opencode --session=<id>` launch lines, one per session —
  convenient for scripting.

## ENVIRONMENT

`OPENCODE_DB`
: Path to the opencode SQLite database (default:
  `~/.local/share/opencode/opencode.db`).

## EXAMPLES

List the 10 most recent sessions:

    oc-sessions

Get the ID of the most recent session:

    oc-sessions -f json 1 | jq -r '.[0].id'

Prepare the launch commands for the last 5 sessions:

    oc-sessions -f cmd 5

Find sessions belonging to one project:

    oc-sessions -f tsv 50 | grep -i myproject | cut -d'|' -f3

## INSTALL

Symlink the script into a directory on your `PATH`:

    ln -s ~/Code/OpenCodeRecentSessions/oc-sessions ~/.local/bin/oc-sessions

Install this man page:

    mkdir -p ~/.local/share/man/man1
    cp oc-sessions.1 ~/.local/share/man/man1/
