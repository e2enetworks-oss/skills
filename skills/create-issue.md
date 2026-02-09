# Create Issue

Create a Zoho Sprint Item or Task and set up the working branch.

## Prerequisites

- Config: `~/.config/e2e/agents/zoho.json` + `~/.config/e2e/agents/.env`
- CLI: `~/.config/e2e/agents/zoho-sprint.mjs`
- If not configured, tell the user to run `/zoho-setup` first

## Instructions

When invoked, follow this exact workflow:

### 1. Gather Input

Ask the user:
- **Type**: Item or Task?
  - If Task: which Item does it belong to? (ask for item ID or list items with the CLI)
- **Title**: Short, descriptive title
- **Description**: Brief description (1-2 sentences)
- **Item type** (for Items only): Story (1), Bug (2), or Task (3). Default: Story.

If $ARGUMENTS are provided, parse them as the title. Use Story as default type.

### 2. Create in Zoho Sprint

The CLI auto-detects the active sprint. No sprint ID needed.

For an **Item**:
```bash
node ~/.config/e2e/agents/zoho-sprint.mjs create-item "<title>" --description "<description>" --type <1|2|3>
```

For a **Task** under an existing Item:
```bash
node ~/.config/e2e/agents/zoho-sprint.mjs create-task <itemId> "<title>" --description "<description>"
```

Parse the JSON output. Extract: `prefix` (e.g., `I42` or `T7`), `url`, `itemId`.

### 3. Create Branch

```bash
git checkout -b feat/<prefix>-<slug>
```

Rules:
- `<prefix>` = `I<itemNo>` for items, `T<taskNo>` for tasks
- `<slug>` = title lowercased, spaces to hyphens, strip special chars, max 40 chars
- Example: `feat/I42-add-user-authentication`

### 4. Write `.zoho-issue`

```bash
echo '<prefix>' > .zoho-issue
```

If `.zoho-issue` is not in `.gitignore`, add it.

### 5. Output

Print exactly:
```
Zoho <prefix> created: <title>
<url>
Branch: feat/<prefix>-<slug>
```

## Helper: List Items in Active Sprint

If user needs to pick a parent item for a task:
```bash
node ~/.config/e2e/agents/zoho-sprint.mjs items
```

## Error Handling

- Missing config → tell user to run `/zoho-setup`
- No active sprint → show sprints list, ask user to pick with `--sprint <id>`
- API error → show the error from CLI output
