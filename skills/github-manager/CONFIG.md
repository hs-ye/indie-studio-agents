# GitHub Manager Configuration

## Project Board Settings
- Default project ID: 1
- Status field ID: PVTSSF_lAHOAHpRbM4BPxw-zg-FswM
- Status mappings:
  - Backlog: f75ad846
  - Ready: 61e4505c
  - In progress: 47fc9ee4
  - In review: df73e18b
  - Done: 98236657

## Common Labels
- Status: ready-for-qa, tech-lead-review, blocked, needs-review
- Type: bug, feature, task, epic
- Area: backend, frontend, ui, api, documentation
- Priority: P0, P1, P2

## Workflow Labels
- ready-for-qa: Indicates work is ready for quality assurance
- tech-lead-review: Requires technical lead approval
- user-input-required: Awaiting user decision
- blocking: Blocking other work, project manager to review and assign agent for unblocking

## Default Issue Templates
- Standard feature issue format
- Bug report format
- Task format

## Authentication
- Requires gh CLI authentication
- Token scopes: repo, project