# OpencodeRecentSessions

A small CLI tool that lists your recent [opencode](https://opencode.ai) sessions with ready-to-run launch commands. Sessions are persisted by opencode in an SQLite database, so this works even after a reboot or a lost terminal.

## Example output

```
2026-11-04 11:24:56 Debugging missing gus notifications              (/home/you/Code/project)
        cd /home/you/Code/project && opencode --session=ses_e5a1c8fe2ffeRJwc6mdf3EbX1Z

2026-11-03 13:45:28 Continuing previous work                         (/home/you/Code/other)
        cd /home/you/Code/other && opencode --session=ses_e6df02b25ffeU7Q6AYCr6g3mve
```

Note: you must `cd` into the session's directory *before* launching, because opencode resolves sessions per project directory.

## Install

Requirements: `bash`, `sqlite3`, and opencode.

1. Clone this repo and symlink the script into a directory on your `PATH`:

```bash
git clone <this-repo> ~/Code/YunohostGits/OpencodeRecentSessions
mkdir -p ~/.local/bin
ln -s ~/Code/YunohostGits/OpencodeRecentSessions/oc-sessions ~/.local/bin/oc-sessions
```

`~/.local/bin` must be on your `PATH` (add `export PATH="$HOME/.local/bin:$PATH"` to your shell profile if needed).

## Usage

```bash
oc-sessions           # list 10 most recent sessions, child/subagent sessions excluded
oc-sessions 20        # list last 20 sessions
oc-sessions -a        # include child/subagent sessions
oc-sessions -h        # show help
```

Options can be combined, e.g. `oc-sessions -a 5`.

## How it works

- Reads `~/.local/share/opencode/opencode.db` (SQLite) directly.
- Rows are sorted by `time_updated` (most recently active first).
- Archived sessions are always hidden.
- Override the database path with `OPENCODE_DB=/path/to/opencode.db`.
