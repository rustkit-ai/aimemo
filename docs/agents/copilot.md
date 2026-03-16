# memo × GitHub Copilot

GitHub Copilot reads `.github/copilot-instructions.md` automatically in VS Code and JetBrains IDEs. `memo setup` writes instructions into that file telling Copilot to run `memo inject --copilot` at the start of each session, keeping the context block up to date.

---

## Setup

Run once in your project root:

```sh
memo setup
```

This writes `.github/copilot-instructions.md` (or appends to it if one already exists).

---

## What gets written

**`.github/copilot-instructions.md`** (appended section):

```markdown
## memo — persistent agent memory
- Run `memo inject --copilot` at the start of every session to recall context
- Run `memo log "<what you did>"` after each significant task
- Run `memo log "todo: <next step>"` before ending the session

<!-- memo:start -->
## memo context
last: (no entries yet)
<!-- memo:end -->
```

Copilot loads `copilot-instructions.md` automatically in every chat session — no configuration needed.

> If a `copilot-instructions.md` already exists, `memo setup` appends the block instead of overwriting the file.

---

## The session loop

```
Open VS Code / JetBrains with Copilot
      │
      ▼
Copilot reads .github/copilot-instructions.md
      │
      ▼
Agent runs: memo inject --copilot
      │
      ▼
.github/copilot-instructions.md updated with latest context
      │
      ▼
Agent reads the context — knows where it left off
      │
      ▼
You work — agent runs:
  memo log "extracted shared Button component, replaced 12 inline usages"
  memo log "todo: update Storybook stories for Button"
      │
      ▼
Next session starts the same way
```

---

## Example session

```
You: [opens Copilot Chat]

Copilot: According to memo — last session you extracted a shared Button
         component and replaced 12 inline usages. Still pending: update
         the Storybook stories for Button. Should I help with that?
```

---

## Manual inject

Update the context block at any time:

```sh
memo inject --copilot
```

---

## Verify setup

```sh
cat .github/copilot-instructions.md
```

You should see the instructions section and the `<!-- memo:start -->` block.

---

## Enabling Copilot instructions in VS Code

Make sure the setting is enabled in VS Code:

```json
{
  "github.copilot.chat.codeGeneration.useInstructionFiles": true
}
```

Or via the UI: **Settings → GitHub Copilot → Chat: Use Instruction Files**.
