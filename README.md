# OpenCodeRecentSessions

A small CLI tool that lists your recent [opencode](https://opencode.ai) sessions with ready-to-run launch commands. Sessions are persisted by opencode in an SQLite database, so this works even after a reboot or a lost terminal.

## Usage

```bash
oc-sessions           # list 10 most recent sessions, child/subagent sessions excluded
oc-sessions 20        # list last 20 sessions
oc-sessions -a        # include child/subagent sessions
oc-sessions -f tsv 5  # pipe-friendly output formats: pretty|tsv|json|cmd
oc-sessions -h        # show help
```

Options can be combined, e.g. `oc-sessions -a 5`.

`pretty` output (default) shows the session details plus its launch command:

```bash
$ oc-sessions 2
2026-11-04 11:24:56 Debugging missing notifications              (/home/you/Code/MyProjectA)
        cd /home/you/Code/MyProjectA && opencode --session=ses_e5a1c8fe2ffeRJwc6mdf3EbX1Z

2026-11-03 13:45:28 Continuing previous work                     (/home/you/Code/MyProjectB)
        cd /home/you/Code/MyProjectB && opencode --session=ses_e6df02b25ffeU7Q6AYCr6g3mve
```

`cmd` output is just the launch lines, one per session:

```bash
$ oc-sessions -f cmd 2
cd /home/you/Code/MyProjectA && opencode --session=ses_e5a1c8fe2ffeRJwc6mdf3EbX1Z
cd /home/you/Code/MyProjectB && opencode --session=ses_e6df02b25ffeU7Q6AYCr6g3mve
```

### Output formats (`-f`)

| format | description |
|---|---|
| `pretty` | human-readable listing with launch commands (default) |
| `tsv` | one row per session, `\|`-separated: `updated\|title\|id\|directory` |
| `json` | JSON array of `{updated, title, id, directory}` objects |
| `cmd` | only the `cd <dir> && opencode --session=<id>` launch lines |

## How it works

- Reads `~/.local/share/opencode/opencode.db` (SQLite) directly.
- Rows are sorted by `time_updated` (most recently active first).
- Archived sessions are always hidden.
- Override the database path with `OPENCODE_DB=/path/to/opencode.db`.

## Install

Requirements: `bash`, `sqlite3`, and opencode.

1. Clone this repo and symlink the script into a directory on your `PATH`:

```bash
git clone <this-repo> ~/Code/OpenCodeRecentSessions
mkdir -p ~/.local/bin
ln -s ~/Code/OpenCodeRecentSessions/oc-sessions ~/.local/bin/oc-sessions
```

`~/.local/bin` must be on your `PATH` (add `export PATH="$HOME/.local/bin:$PATH"` to your shell profile if needed).

2. Install the man page:

```bash
mkdir -p ~/.local/share/man/man1
ln -sf <this-repo>/oc-sessions.1 ~/.local/share/man/man1/oc-sessions.1
```

Then `man oc-sessions` works with no MANPATH configuration (man-db 2.13 searches `~/.local/share/man` by default).

The man page is generated from `oc-sessions.1.md` with pandoc:

```bash
pandoc --standalone --to man -o oc-sessions.1 oc-sessions.1.md
```
