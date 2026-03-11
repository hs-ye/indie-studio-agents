# GitHub Manager Tutorial

## Introduction
This tutorial will walk you through using the GitHub Manager skill to interact with GitHub repositories, manage issues, and coordinate development workflows.

## Prerequisites
- GitHub CLI installed and authenticated
- Access to the target repository
- Understanding of the project board structure

## Basic Issue Management

### Creating an Issue
The most common task is creating issues to track work:

```bash
gh issue create \
  --title "[Feature] Add user profile page" \
  --body "## Description
Create a user profile page that displays user information.

## Acceptance Criteria
- [ ] Shows user's name and email
- [ ] Displays account creation date
- [ ] Allows editing of profile information

## Notes
Consider privacy settings for profile visibility." \
  --label "frontend" \
  --assignee "@me"
```

### Updating an Issue
Once work is complete, update the issue:

```bash
gh issue comment 123 --body "Feature implemented and tested. Ready for QA review."
gh issue edit 123 --add-label "ready-for-qa" --remove-label "in-progress"
```

## Project Board Workflow

### Moving Issues Between States
Issues on the project board transition between states as work progresses:

1. **Backlog** → Ready for work to begin
2. **Ready** → Assigned to developer
3. **In Progress** → Work actively happening
4. **In Review** → Work complete, awaiting review
5. **Done** → Work completed and accepted

To move an issue from "In Progress" to "In Review":

```bash
# First get the project item ID
ITEM_ID=$(gh issue view 123 --json projectItems --jq '.projectItems[0].id')

# Then update the status to "In Review" (assuming df73e18b is the ID for "In Review")
gh project item-edit --id $ITEM_ID --field-id PVTSSF_lAHOAHpRbM4BPxw-zg-FswM --project-id 1 --single-select-option-id df73e18b
```

## Coordinating with Team Members

### Using Labels Effectively
Labels help categorize and prioritize work:

- **Functional area**: `backend`, `frontend`, `ui`, `api`
- **Work type**: `bug`, `feature`, `refactor`, `documentation`
- **Priority**: `P0`, `P1`, `P2`
- **Status**: `ready-for-qa`, `needs-review`, `blocked`

### Commenting with Context
When commenting on issues, provide context for other team members:

```bash
gh issue comment 123 --body "Tech-Lead: Reviewed this implementation and found the following issues:

1. Security vulnerability in user input validation
2. Performance issue with database queries

Requesting changes before merging."
```

## Advanced Operations

### Bulk Issue Management
For managing multiple issues at once:

```bash
# List all open issues with a specific label
gh issue list --label "bug" --state open

# Update multiple issues (manually one by one or in a script)
for issue in $(gh issue list --label "needs-triage" --json number --jq '.[].number'); do
  gh issue edit $issue --add-label "triaged" --remove-label "needs-triage"
done
```

### Linking Related Work
When issues are related, reference them in descriptions or comments:

```bash
gh issue comment 123 --body "This issue is related to #456 and may need to be implemented together."
```

## Best Practices

1. **Be descriptive**: Clear titles and bodies help others understand the work
2. **Use consistent labels**: Maintain labeling conventions across the team
3. **Update status regularly**: Keep the project board accurate
4. **Comment with context**: Explain decisions and reasoning
5. **Close issues when done**: Clean up completed work