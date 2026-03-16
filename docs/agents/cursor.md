# memo × Cursor

Cursor reads rules from `.cursor/rules/` automatically. `memo setup` writes a rule file with `alwaysApply: true` that instructs Cursor to run `memo inject --cursor` at the start of every session, keeping the context block up to date.

---

## Setup

Run once in your project root:

```sh
memo setup
```

This writes `.cursor/rules/memo.mdc` with `alwaysApply: true`.

---

## What gets written

**`.cursor/rules/memo.mdc`**:

```markdown
---
description: memo persistent memory instructions
alwaysApply: true
---

## memo — persistent agent memory
- Run `memo inject --cursor` at the start of every session to recall context
- Run `memo log "<what you did>"` after each significant task
- Run `memo log "todo: <next step>"` before ending the session

<!-- memo:start -->
## memo context
last: (no entries yet)
<!-- memo:end -->
```

Because `alwaysApply: true` is set, Cursor loads this rule file in every chat and Composer session without you having to select it manually.

---

## The session loop

```
Open Cursor
      │
      ▼
Cursor loads .cursor/rules/memo.mdc  (alwaysApply: true)
      │
      ▼
Agent runs: memo inject --cursor
      │
      ▼
.cursor/rules/memo.mdc updated with latest context
      │
      ▼
Agent reads the context — knows where it left off
      │
      ▼
You work — agent runs:
  memo log "refactored payment service to use idempotency keys"
  memo log "todo: write integration test for duplicate charge case"
      │
      ▼
Next session starts the same way
```

---

## Example session

```
You: [opens Cursor, starts a new chat]

Cursor: I can see from memo that last session you refactored the payment
        service to use idempotency keys. There's a pending todo: write an
        integration test for the duplicate charge case. Should I start there?
```

---

## Manual inject

Update the context block at any time:

```sh
memo inject --cursor
```

---

## Verify setup

```sh
cat .cursor/rules/memo.mdc
```

You should see the `alwaysApply: true` frontmatter and the `<!-- memo:start -->` block.
