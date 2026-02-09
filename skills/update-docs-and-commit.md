# Update Docs and Commit

Update CHANGELOG.md based on code changes, then commit.

## Instructions

Be conservative — only update for **user-visible changes**.

## Process

1. **Analyze git changes**
   - Run `git status` and `git diff`
   - Identify user-facing changes vs internal changes

2. **Update CHANGELOG.md if needed**

   **Update for:**
   - ✓ New features, bug fixes, breaking changes
   - ✓ Deprecated APIs, removed features
   - ✓ Performance improvements users will notice

   **Skip for:**
   - ✗ Internal refactoring
   - ✗ Documentation updates
   - ✗ Code cleanup, formatting
   - ✗ Test changes

3. **CHANGELOG format**

   Add entry under `## [Unreleased]` section:
   ```markdown
   ## [Unreleased]

   ### Added
   - New feature description

   ### Changed
   - Changed behavior description

   ### Fixed
   - Bug fix description
   ```

4. **Version bumping**

   **DO NOT** auto-bump version numbers. That's a manual decision.
   - User decides when to release (MAJOR.MINOR.PATCH)
   - Version bump happens separately from CHANGELOG update

5. **Stage and commit**
   - Stage only CHANGELOG.md
   - Commit: `docs: update CHANGELOG for [brief description]`

## Output

1. Analysis of changes (user-visible or internal?)
2. CHANGELOG entry added (or "No user-visible changes")
3. Commit created (or "No update needed")
