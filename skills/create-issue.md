# Create Issue

Create a Linear/Jira issue and set up the working branch.

## Prerequisites

- Linear/Jira CLI installed and configured
- Issue tracker credentials set up
- Active project/team selected

## Instructions

When invoked, follow this exact workflow:

### 1. Gather Input

Ask the user:
- **Title**: Short, descriptive title
- **Description**: Brief description (1-2 sentences)
- **Issue type** (optional): Bug, Feature, Task, etc. Default: Feature.

If $ARGUMENTS are provided, parse them as the title. Use default type.

### 2. Create Issue in Tracker

Use the appropriate CLI command for your issue tracker:

**Linear**:
```bash
linear issue create --title "<title>" --description "<description>"
```

**Jira**:
```bash
jira issue create --summary "<title>" --description "<description>"
```

Parse the output. Extract: `issueId` (e.g., `ENG-142` or `ABC-123`), `url`, `title`.

### 3. Create Branch

```bash
git checkout -b feat/<issueId>-<slug>
```

Rules:
- `<issueId>` = The issue identifier from tracker
- `<slug>` = title lowercased, spaces to hyphens, strip special chars, max 40 chars
- Example: `feat/ENG-142-add-user-authentication`

### 4. Output

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

## Error Handling

- Missing CLI → tell user to install Linear or Jira CLI
- Not authenticated → tell user to run `linear login` or `jira login`
- No active team/project → show available teams/projects, ask user to select
- API error → show the error from CLI output
