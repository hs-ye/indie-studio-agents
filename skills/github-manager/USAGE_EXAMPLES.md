# GitHub Manager Usage Examples

## Moving an Issue to "Ready" Status
```bash
# After creating a new task that's ready for work
ITEM_ID=$(gh issue view 123 --json projectItems --jq '.projectItems[0].id')
gh project item-edit --id $ITEM_ID --field-id PVTSSF_lAHOAHpRbM4BPxw-zg-FswM --project-id 1 --single-select-option-id 61e4505c
```

## Creating a Feature Issue
```bash
gh issue create \
  --title "[Feature] Implement user authentication" \
  --body "## Description
Implement user login and registration functionality.

## Acceptance Criteria
- [ ] Users can register with email/password
- [ ] Users can login with credentials
- [ ] Session management implemented

## Technical Notes
Consider using JWT tokens for session management." \
  --label "backend" \
  --label "security"
```

## Updating Issue Status After Completion
```bash
# After completing work, comment and update status
gh issue comment 123 --body "Implementation complete. Ready for review."
ITEM_ID=$(gh issue view 123 --json projectItems --jq '.projectItems[0].id')
gh project item-edit --id $ITEM_ID --field-id PVTSSF_lAHOAHpRbM4BPxw-zg-FswM --project-id 1 --single-select-option-id df73e18b
```

## Creating a Bug Report
```bash
gh issue create \
  --title "[Bug] Login fails with valid credentials" \
  --body "## Bug Description
Login returns error even with correct username/password.

## Steps to Reproduce
1. Navigate to login page
2. Enter valid credentials
3. Click login button

## Expected Behavior
User should be logged in successfully

## Actual Behavior
Error message displayed: 'Invalid credentials'

## Environment
- Browser: Chrome 98
- OS: Windows 10" \
  --label "bug" \
  --label "backend" \
  --assignee "developer-username"
```

## Creating a blocking ticket
```bash
  gh issue create \
    --title "[BLOCKING] User Decision Required: [Issue Description]" \
    --body "## Blocking Issue
  [Description of what decision is needed]

  ## Context
  [Background and why this decision is needed]

  ## Options
  - Option A: [Description]
  - Option B: [Description]

  ## Impact
  [What happens if decision is not made]

  **Blocks**: #[epic-number], #[related-tickets]" \
    --label "blocking" \
    --label "user-input-required"
  ```

## Creating a Pull Request
```bash
gh pr create \
  --title "feat(auth): Add user registration functionality" \
  --body "## Summary
Implements user registration with email verification.

## Changes
- Added registration form
- Created user creation API endpoint
- Implemented email verification flow

## Testing
- Verified form validation
- Tested API endpoint with Postman
- Confirmed email sending" \
  --label "enhancement" \
  --label "backend" \
  --reviewer "tech-lead-username"
```