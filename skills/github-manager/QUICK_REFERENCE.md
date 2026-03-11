# GitHub Manager Quick Reference

## Project Board Commands
- View project fields: `gh project field-list 1 --owner <owner> --format json`
- Update item status: `ITEM_ID=$(gh issue view <num> --json projectItems --jq '.projectItems[0].id') && gh project item-edit --id $ITEM_ID --field-id <field-id> --project-id 1 --single-select-option-id <status-id>`

## Issue Commands
- Create issue: `gh issue create --title "..." --body "..." --label "..."`  
- List issues: `gh issue list --label "..." --state open`
- Update issue: `gh issue edit <num> --add-label "..." --remove-label "..."`
- Comment: `gh issue comment <num> --body "..."`

## PR Commands
- Create PR: `gh pr create --title "..." --body "..." --label "..."`  
- List PRs: `gh pr list --state open`
- Merge PR: `gh pr merge <num> --merge`

## Common Labels
- `ready-for-qa`, `tech-lead-review`, `bug`, `enhancement`
- `backend`, `frontend`, `ui`, `api`, `dependencies`
- `epic`, `blocking`, `user-input-required`