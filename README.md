# claude-search-sessions

Search past Claude Code conversations by keyword. Zero-dependency plugin that uses built-in tools only.

## Install

```
/plugin marketplace add kanekin/claude-search-sessions
/plugin install claude-search-sessions@kanekin
```

## Usage

```
/search-sessions <keyword>
```

Returns matching sessions grouped by ID with timestamps, ready to resume with `claude --resume "SESSION_ID"`.

## Example

```
/search-sessions auth migration
```

```
Found 3 matches in 2 sessions

  2026-03-10  a1b2c3d4-...
              We need to migrate the auth middleware...

  2026-03-12  e5f6g7h8-...
              Auth migration is done, let's test...

Resume with: claude --resume "SESSION_ID"
```
