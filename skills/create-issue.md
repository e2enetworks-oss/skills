# Create Issue

Create an issue in Linear, Jira, or Zoho Sprint and set up the working branch.

## Prerequisites

Choose your issue tracker:

**Linear**:
- Linear CLI installed: `npm install -g @linear/cli`
- Authenticated: `linear login`
- Active team selected

**Jira**:
- Jira CLI installed: `npm install -g jira-cli`
- Authenticated with your Jira instance
- Active project selected

**Zoho Sprint**:
- Zoho Sprint CLI script installed: `~/.config/e2e/agents/zoho-sprint.mjs`
- Credentials configured: Run `/zoho-setup` first
- Config file: `~/.config/e2e/agents/zoho.json`

## Instructions

When invoked, follow this exact workflow:

### 1. Ask for Issue Tracker

Ask the user: "Which issue tracker? (linear/jira/zoho)"

Default to the tracker they've used most recently, or detect from available CLIs.

### 2. Gather Input

Ask the user:
- **Title**: Short, descriptive title
- **Description**: Brief description (1-2 sentences)
- **Issue type** (optional):
  - Linear: Bug, Feature, Task, etc.
  - Jira: Bug, Story, Task, etc.
  - Zoho: Story (1), Bug (2), Task (3)
  - Default: Feature/Story/Story (depending on tracker)

If $ARGUMENTS are provided, parse them as the title. Use default type.

### 3. Create Issue in Tracker

Use the appropriate CLI command for the selected tracker:

**Linear**:
```bash
linear issue create --title "<title>" --description "<description>"
```

**Jira**:
```bash
jira issue create --summary "<title>" --description "<description>"
```

**Zoho Sprint**:
```bash
node ~/.config/e2e/agents/zoho-sprint.mjs create-item "<title>" --description "<description>" --type <1|2|3>
```

Parse the output. Extract: `issueId` (e.g., `ENG-142`, `ABC-123`, or `I42`), `url`, `title`.

### 4. Create Branch

```bash
git checkout -b feat/<issueId>-<slug>
```

Rules:
- `<issueId>` = The issue identifier from tracker
- `<slug>` = title lowercased, spaces to hyphens, strip special chars, max 40 chars
- Examples:
  - `feat/ENG-142-add-user-authentication`
  - `feat/I42-add-user-authentication`

### 5. Output

Print exactly:
```
Issue <issueId> created: <title>
<url>
Branch: feat/<issueId>-<slug>
```

## Helper: List Issues

If user needs to reference existing issues:

**Linear**:
```bash
linear issue list
```

**Jira**:
```bash
jira issue list
```

**Zoho Sprint**:
```bash
node ~/.config/e2e/agents/zoho-sprint.mjs items
```

## Error Handling

**Missing CLI**:
- Linear: `npm install -g @linear/cli`
- Jira: `npm install -g jira-cli`
- Zoho: Run `/zoho-setup` to install the script

**Not authenticated**:
- Linear: Run `linear login`
- Jira: Run `jira login`
- Zoho: Run `/zoho-setup` to configure credentials

**No active team/project**:
- Show available teams/projects
- Ask user to select one

**API error**: Show the error from CLI output
