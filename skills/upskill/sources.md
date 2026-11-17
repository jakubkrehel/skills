# Where sessions live

Check each path and use what exists. Formats change between versions, so read the first line of a file before parsing it.

| Harness | Prompts | Full sessions |
| --- | --- | --- |
| Claude Code | `~/.claude/history.jsonl`, one JSON object per line with `display`, `timestamp` in milliseconds and `project` | `~/.claude/projects/<project path with slashes as dashes>/*.jsonl` |
| Codex | `~/.codex/history.jsonl`, with `text`, `ts` in seconds and `session_id` | `~/.codex/sessions/<year>/<month>/<day>/`, where each session records its working directory |
| opencode | none in one file | `~/.local/share/opencode/` |

A harness missing from this table still keeps its history somewhere under the home directory. Look for it before reporting the source as missing.

## Matching prompts to the project

Match on the project's folder name anywhere in the path, not on one exact path. A worktree, a renamed folder and a tool that manages its own checkouts each log under a different path. List the matched paths in the notes so the person can spot one that doesn't belong.

Where the prompt file carries no path, as with Codex, read each session's working directory and keep the sessions that ran inside the project.

## What the prompt file leaves out

The prompt file holds what the person typed. The full sessions also hold what the agent did, which answers two questions the prompts cannot. One is which commands the agent ran most. The other is what it was doing just before a correction. Read them for the patterns you intend to act on, never in bulk.
