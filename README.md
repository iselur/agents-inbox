# agents-inbox

An inbox for your Claude Code background agents.

If you run a lot of background jobs, the job list tells you what *finished* — but not
which results you've already read. This is a tiny script that remembers that for you,
like unread marks in a mail client.

```
$ agents
● 05eaabe5   1m ago  done     lets correct models json
    relay prod models.json updated on main (#235): bound_reviewer → …
· b9c0456e   2m ago  running  simplify the harness research artifact
✓ 7dfde0f6  27m ago  done     whats in the ledger

3 running · 2 unread — 'agents show ID' to read, 'agents read ID' to check off
```

`●` finished, you haven't read it (its one-line result shows underneath) ·
`✓` finished and read · `·` still running.

## Install

One Python file, no dependencies (Python 3.7+):

```sh
curl -fsSL https://raw.githubusercontent.com/iselur/agents-inbox/main/agents \
  -o ~/.local/bin/agents && chmod +x ~/.local/bin/agents
```

## Use

```
agents             list all jobs, newest first
agents unread      only the finished ones you haven't read yet
agents show ID     read one job's intent and full result
agents read ID..   check job(s) off ('agents read all' clears the backlog)
agents unread ID   flip one back to unread
```

IDs can be abbreviated to any unique prefix.

## How it works

Claude Code keeps each background job's state in `~/.claude/jobs/<id>/state.json`.
The script lists those, and marking a job read just touches a `.read` file inside
its job directory. If the job produces new output after you marked it read, it
flips back to unread automatically (marker older than the job's `updatedAt`).
Deleting a job deletes its marker; nothing is written anywhere else.

Respects `$CLAUDE_CONFIG_DIR` (default `~/.claude`) and `$NO_COLOR`.

## License

MIT
