# Where sessions live

Check each path and use what exists. Formats change between versions, so read the first line of a file before parsing it.

| Harness | Prompts | Full sessions |
| --- | --- | --- |
| Claude Code | `~/.claude/history.jsonl`, one JSON object per line with `display`, `timestamp` in milliseconds, `project`, `sessionId` and `pastedContents` | `~/.claude/projects/<project path with slashes as dashes>/*.jsonl` |
| Codex | `~/.codex/history.jsonl`, with `text`, `ts` in seconds and `session_id` | `~/.codex/sessions/<year>/<month>/<day>/` and `~/.codex/archived_sessions/`. The first line of each session holds the working directory at `payload.cwd` |
| Cursor | none in one file | `~/.cursor/projects/<project path with slashes as dashes>/agent-transcripts/<id>/<id>.jsonl`. Entries carry no date field, so use the file's modified time |
| opencode | none in one file | `~/.local/share/opencode/` |

A harness missing from this table still keeps its history somewhere under the home directory. Look for it before reporting the source as missing.

## Matching prompts to the project

Match on the project's folder name as a whole path segment, not on one exact path. A worktree, a renamed folder and a tool that manages its own checkouts each log under a different path. List the matched paths in the notes so the person can spot one that doesn't belong.

Where the prompt file carries no path, as with Codex, read each session's working directory and keep the sessions that ran inside the project.

Prompts about this project typed inside another project's session are left out. Say so in the report when a search shows there are some.

## Pastes and images

Claude Code stores a prompt's `display` with placeholders: `[Image #1]` for a screenshot and `[Pasted text #1 +37 lines]` for a paste. A short paste keeps its text in `pastedContents`. A long one is stored elsewhere by hash. Count the ones you could not recover.

## What the prompt file leaves out

The prompt file holds what the person typed. The full sessions also hold what the agent did, which answers two questions the prompts cannot. One is which commands and skills the agent ran most. The other is what it was doing just before a correction.

A script may tally across every session. Open a session to read it only for a pattern you intend to act on.
