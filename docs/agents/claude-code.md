# memo × Claude Code

Claude Code reads `CLAUDE.md` automatically at the start of every session. `memo` writes a context block into that file and installs a Stop hook so the block updates automatically when you close a session.

**This is the most seamless integration** — no manual steps, ever.

---

## Setup

Run once in your project root:

```sh
memo setup
```

That's all. Two things happen:

1. **`CLAUDE.md`** gets a `memo` instructions block and an empty context block
2. **`.claude/settings.json`** gets a Stop hook that runs `memo inject --claude` each time you close Claude Code

---

## What gets written

**`CLAUDE.md`** (excerpt):

```markdown
<!-- memo:instructions:start -->
## memo — persistent agent memory
- Run `memo inject --claude` at the start of every session to recall context
- Run `memo log "<what you did>"` after each significant task
- Run `memo log "todo: <next step>"` before ending the session
<!-- memo:instructions:end -->

<!-- memo:start -->
## memo context
last: (no entries yet)
<!-- memo:end -->
```

**`.claude/settings.json`** (excerpt):

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          { "type": "command", "command": "memo inject --claude" }
        ]
      }
    ]
  }
}
```

---

## The session loop

```
Open Claude Code
      │
      ▼
Claude reads CLAUDE.md  ←── context from last session
      │
      ▼
You work — Claude runs:
  memo log "added OAuth2 flow via google provider"
  memo log "todo: handle token expiry edge case"
      │
      ▼
You close Claude Code
      │
      ▼
Stop hook fires automatically
      │
      ▼
memo inject --claude  ←── CLAUDE.md updated silently
      │
      ▼
Next session starts with fresh context
```

---

## Example session

```
You: what did we work on last time?

Claude: Based on memo context — you added an OAuth2 flow via Google provider.
        There's a pending todo: handle token expiry edge case.
        Recent tags: auth · oauth · todo.
        Want me to pick up from there?
```

---

## Manual inject

If you ever want to update the context block manually (e.g. mid-session):

```sh
memo inject --claude
```

---

## Verify the hook is installed

```sh
cat .claude/settings.json
```

You should see `memo inject --claude` in the `Stop` hooks array.

If the hook is missing, run `memo setup` again — it is idempotent and safe to re-run.
