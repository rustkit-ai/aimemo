# memo × Windsurf

Windsurf reads `.windsurfrules` automatically in every session. `memo setup` writes instructions into that file telling Windsurf to run `memo inject --windsurf` at the start of each session, so the context block stays current.

---

## Setup

Run once in your project root:

```sh
memo setup
```

This writes `.windsurfrules`.

---

## What gets written

**`.windsurfrules`**:

```markdown
# memo — persistent agent memory
- Run `memo inject --windsurf` at the start of every session to recall context
- Run `memo log "<what you did>"` after each significant task
- Run `memo log "todo: <next step>"` before ending the session

<!-- memo:start -->
## memo context
last: (no entries yet)
<!-- memo:end -->
```

Windsurf loads `.windsurfrules` automatically — no configuration needed.

---

## The session loop

```
Open Windsurf
      │
      ▼
Windsurf reads .windsurfrules
      │
      ▼
Agent runs: memo inject --windsurf
      │
      ▼
.windsurfrules updated with latest context
      │
      ▼
Agent reads the context — knows where it left off
      │
      ▼
You work — agent runs:
  memo log "migrated database from PostgreSQL 14 to 16"
  memo log "todo: update connection pool config for pg16 defaults"
      │
      ▼
Next session starts the same way
```

---

## Example session

```
You: [opens Windsurf, starts a new session]

Windsurf: Based on memo — last session you migrated the database from
          PostgreSQL 14 to 16. Pending: update the connection pool config
          for pg16 defaults. Want to tackle that now?
```

---

## Manual inject

Update the context block at any time:

```sh
memo inject --windsurf
```

---

## Verify setup

```sh
cat .windsurfrules
```

You should see the instructions and the `<!-- memo:start -->` block.
