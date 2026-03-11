---
name: github-manager
description: Manages GitHub interactions including issues, projects, pull requests, and repository operations. Provides structured commands for creating, updating, and tracking work through GitHub.
tools:
  - run_shell_command
  - read_file
  - write_file
  - edit
---

# GitHub Manager Skill

This skill enables agents to interact with GitHub repositories, manage issues, track work through projects, and coordinate development activities using GitHub CLI.

## Core Capabilities

### Project Management
- View and update GitHub project board statuses
- Manage issue lifecycles through project workflows
- Track work items across different project states

### Issue Management
- Create, update, and close GitHub issues
- Assign labels, milestones, and assignees
- Link related issues and track dependencies
- Comment on issues with structured updates

### Pull Request Management
- Create, review, and merge pull requests
- Track PR status and CI/CD status
- Link PRs to related issues

## GitHub Project Board Integration

### Status Mapping
- `"Backlog"` (ID: f75ad846) - New work items not yet scheduled
- `"Ready"` (ID: 61e4505c) - Work items ready for implementation
- `"In progress"` (ID: 47fc9ee4) - Active work in progress
- `"In review"` (ID: df73e18b) - Work completed, awaiting review
- `"Done"` (ID: 98236657) - Completed work items

### Project Commands
```bash
# List project fields to get status option IDs
gh project field-list <project-number> --owner <owner> --format json

# Get project ID from project number (needed for item-edit command)
gh project list --owner <owner> --format json

# List project items to get individual item IDs
gh project item-list <project-number> --owner <owner> --format json

# Update an item's status
gh project item-edit --id <project-item-id> --field-id <status-field-id> --project-id <project-id> --single-select-option-id <status-id>

# Example workflow:
# 1. Get the project ID (returns something like PVT_kwHOAHpRbM4BQqNA)
PROJECT_ID=$(gh project list --owner <owner> --format json | jq -r '.projects[] | select(.number == <project-number>) | .id')

# 2. Get the project item ID for a specific issue (returns something like PVTI_lAHOAHpRbM4BQqNAzgmoBX0)
PROJECT_ITEM_ID=$(gh project item-list <project-number> --owner <owner> --format json | jq -r '.items[] | select(.content.number == <issue-number>) | .id')

# 3. Update the status (requires the actual project ID, project item ID, status field ID, and status option ID)
gh project item-edit --id $PROJECT_ITEM_ID --field-id <status-field-id> --project-id $PROJECT_ID --single-select-option-id <status-id>

# Note: The --project-id parameter requires the full project ID (PVT_* format), not just the project number
# The --id parameter requires the project item ID (PVTI_* format), not the issue number
# The --field-id parameter requires the status field ID (PVTSSF_* format)
```

## Issue Management Commands

### Creating Issues
```bash
gh issue create \
  --title "[Type] Short descriptive title" \
  --body "## Description
Full description of the issue

## Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2

## Additional Context
[Any additional information]" \
  --label "backend" \
  --assignee "@me"
```

### Updating Issues
```bash
# Add labels
gh issue edit <issue-number> --add-label "ready-for-qa"

# Remove labels
gh issue edit <issue-number> --remove-label "needs-revision"

# Add comments
gh issue comment <issue-number> --body "Comment content"
```

### Listing and Filtering Issues
```bash
# List issues by label
gh issue list --label "backend" --state open

# List issues by assignee
gh issue list --assignee "@me" --state open

# List issues by milestone
gh issue list --milestone "v1.0"

# Search issues
gh issue list --search "is:open label:bug"
```

## Pull Request Management

### Creating Pull Requests
```bash
gh pr create \
  --title "Feature: Brief description" \
  --body "## Summary
What this PR does

## Changes
- Change 1
- Change 2

## Testing
How to test these changes" \
  --label "enhancement" \
  --reviewer "reviewer-username"
```

### Managing Pull Requests
```bash
# List PRs
gh pr list --state open

# Check PR status
gh pr status

# Merge PR
gh pr merge <pr-number> --merge

# Close PR
gh pr close <pr-number>
```

## Best Practices for Agents

### Issue Titles
- Use prefixes like `[Feature]`, `[Bug]`, `[Tech Debt]`, `[Task]`
- Keep titles concise but descriptive
- Include context when possible

### Issue Bodies
- Use structured format with sections
- Include acceptance criteria for feature work
- Provide reproduction steps for bugs
- Link to related issues or designs

### Comments
- Prefix comments with role identifier (e.g., "QA-Tester:", "Tech-Lead:")
- Provide context for decisions
- Summarize work completed
- Mention next steps or blockers

### Labels
- Use consistent labeling scheme
- Include role-specific labels (backend, frontend, ui, etc.)
- Use status labels (ready-for-qa, needs-review, etc.)
- Include priority labels when appropriate

## Common Workflows

### Feature Development
1. Create feature issue with requirements
2. Break into subtasks if needed
3. Move to "Ready" status on project board
4. Assign to developer
5. Developer moves to "In progress"
6. Developer completes work, moves to "In review"
7. Reviewer approves, moves to "Done"

### Bug Fixing
1. Create bug report with reproduction steps
2. Triage and assign priority
3. Assign to developer
4. Developer fixes, adds "ready-for-test"
5. QA tests, closes if fixed or reopens if not

### Project Tracking
1. Create epic for major features
2. Break epic into granular tasks
3. Track progress through project board
4. Update stakeholders on status
5. Close when all tasks complete