## GitHub Configuration for Qwen/Claude/Gemini Agents

This file documents the GitHub-specific configurations that need to be customized for your project. This project is designed to be used by a set of LLM powered agents that will act as a software engineering team on GitHub, following the conventions listed in this file. 

### Instructions
Give this to Qwen/Claude/Gemini and ask it to configure the Qwen/Claude/Gemini.md file with the following information, fetching the correct information from github to populate the Project Board Configuration section.

You need to provide the Project_ID of the logged in user / owner and it should fetch the rest from github

### Fixed Issue Labels (Hardcoded)
These labels are fixed across all projects and should not be changed:
- `master-backlog` - Used by project-manager to identify the master backlog issue
- `project-manager-review` - Issues requiring project-manager attention
- `user-input-required` - Issues that require user decisions
- `blocking` - Used for blocking issues that prevent progress
- `backend` - For backend tasks (including API tasks), assigned to software-engineer
- `frontend` - For frontend tasks, assigned to software-engineer
- `ui` - For UI tasks, assigned to design-specialist
- `dependencies` - For dependency tasks that should be completed first to unblock others, the ticket itself should specify which other ticket is a dependency
- `epic` - For epic-level issues
- `tech-lead-review` - For technical lead review
- `qa` - For tickets needing QA-tester attention. Do not use for unit/local tests
- `bug` - For bug reports, these can be raised by anyone and should be reviewed by the tech-lead

### Project Board Configuration (Variable per project)
These configurations may vary between different projects:
- `[PROJECT_BOARD_FIELD_ID]` - Project board field ID for status tracking (placeholder value: `PVTSSF_your_project_status_field_id`)
- `[PROJECT_ID]` - Project ID (placeholder value: `1`)

Ticket status configurations:
- `[READY_STATUS_OPTION_ID]` - 'Ready' status option ID (placeholder value: `your_ready_option_id`)
- `[IN_PROGRESS_STATUS_OPTION_ID]` - 'In progress' status option ID (placeholder value: `your_in_progress_option_id`)
- `[DONE_STATUS_OPTION_ID]` - 'Done' status option ID (placeholder value: `your_done_option_id`)

Tickets should be moved to the appropriate status when worked on by agents to give visibility on progress to other agents and the user.

### Directory Structure
- ADRs (Architectural Decision Records) are stored in: `/design/adr/`

### Git Workflow
- Feature branch naming convention: `feat/[ticket number] - [ticket name]`
- Branch for development work should be based on master branch
- Pull requests should be raised on feature branch into the master branch after being reviewed.

### Configuration Instructions

#### 1. Find Your GitHub Project Board Configuration Values

Before running these commands, ensure your GitHub CLI is properly authenticated with the required scopes:

1. **Authenticate with required scopes**:
   ```bash
   gh auth login
   ```
   During the login process, make sure to grant the `read:project` scope which is required for accessing project boards.

   Alternatively, if you already have a token but need to add the scope:
   ```bash
   gh auth refresh -s read:project
   ```

2. **Get your project ID**:
   ```bash
   gh project list --format json
   ```
   Or visit your project board in the browser and note the number in the URL.

3. **Get your project board field ID for status tracking**:
   ```bash
   gh project field-list --format json --project-id [YOUR_PROJECT_ID]
   ```
   Look for the field named "Status" or similar.

4. **Get the status option IDs**:
   ```bash
   gh project field-value-list --format json --project-id [YOUR_PROJECT_ID] --field-status
   ```
   This will show you the IDs for "Ready", "In progress", "Done", etc. status options.

#### 2. Add Configuration to Your Qwen/Claude/Gemini.md File

Add this section to your project's Qwen/Claude/Gemini.md file:

```markdown
## GitHub Project Board Configuration

### Project Board Configuration
- `PROJECT_BOARD_FIELD_ID`: [Your status field ID from step 2]
- `PROJECT_ID`: [Your project ID from step 1]
- `READY_STATUS_OPTION_ID`: [ID for 'Ready' status from step 3]
- `IN_PROGRESS_STATUS_OPTION_ID`: [ID for 'In progress' status from step 3]
- `DONE_STATUS_OPTION_ID`: [ID for 'Done' status from step 3]
```


The Qwen/Claude/Gemini agents will reference these values when interacting with your GitHub project board. The issue labels are standardized across all projects and do not need to be configured.