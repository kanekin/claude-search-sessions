---
name: search-sessions
description: Use when the user wants to find a past Claude Code conversation by keyword, topic, or content - searches message history across all sessions
---

# Search Sessions

Search `~/.claude/history.jsonl` for past conversations by keyword.

## When to Use
- User asks "which session did we discuss X?"
- User wants to find a past conversation
- User wants to resume a session but doesn't remember the ID

## How to Search

1. Use the **Grep** tool to search `~/.claude/history.jsonl` for the keyword:
   - pattern: the user's keyword
   - path: `~/.claude/history.jsonl`
   - output_mode: `content`
   - `-i`: true (case-insensitive)
   - head_limit: 50 (avoid flooding context on large histories)

2. Each matching line is a JSON object with fields: `sessionId`, `timestamp`, `display`.

3. Parse the matching lines and present results grouped by `sessionId`:
   - Convert `timestamp` (ms since epoch) to human-readable date
   - Show first ~100 chars of `display` for each match (up to 3 per session)
   - Number each session (1, 2, 3...) for easy selection
   - End with: "Which session would you like to resume? (number or session ID)"

## Resuming a Session

When the user picks a session (by number or ID), open it in a new terminal tab.

**CRITICAL:** Each history line has a `project` field containing the directory the session ran in. You MUST `cd` to that directory before running `claude --resume`, otherwise the session will fail to load or load in the wrong project context.

### Steps

1. Extract the `project` field from the matched history line for the selected session.
2. Build the full command: `cd PROJECT_DIR && claude --resume SESSION_ID`
3. Write it to a temp file (avoids shell/AppleScript escaping issues):
   ```bash
   printf '%s' 'cd /path/to/project && claude --resume SESSION_ID' > /tmp/iterm_cmd.txt
   ```
4. Detect the terminal via `$TERM_PROGRAM` and open a new tab.

### iTerm2

```bash
osascript \
  -e 'tell application "iTerm2" to tell current window to create tab with default profile' \
  -e "tell application \"iTerm2\" to tell current session of current tab of current window to write text \"$(cat /tmp/iterm_cmd.txt)\""
```

### Terminal.app

```bash
osascript -e "tell application \"Terminal\" to do script \"$(cat /tmp/iterm_cmd.txt)\""
```

### Fallback

Print the command for the user to copy.

## Notes
- Searches the `display` field (user messages only)
- Case-insensitive
- No external dependencies — uses only Claude Code built-in tools
