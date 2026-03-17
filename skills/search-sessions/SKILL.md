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
   - End with: `Resume with: claude --resume "SESSION_ID"`

## Notes
- Searches the `display` field (user messages only)
- Case-insensitive
- No external dependencies — uses only Claude Code built-in tools
