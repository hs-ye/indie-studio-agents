# Instructions

This project is designed to be used by a set of LLM powered agents that will act as a software engineering team on GitHub, following the conventions listed in this file.

Give this to Qwen/Claude/Gemini and ask it to configure the `Qwen/Claude/Gemini.md` file with the following information, fetching the correct information from github to populate a `Github Conventions and Configuration` section. 

If you are a coding agent, read below to understand how to configure your user's Qwen/Claude/Gemini.md files. If there is an existing file, add to it rather than replace it.

# GitHub Configuration for Qwen/Claude/Gemini Agents

This file documents the GitHub-specific configurations that need to be customized and added to for agent configuration for your project (i.e. the `Qwen/Claude/Gemini.md` file)

The user needs to provide the Project_ID as well as the github owner ID (can be logged in or an org) and the agent should fetch the rest of hte info from github

## Github Config

### Fixed Issue Labels (Hardcoded)
Instructions to Agents: Add these to Qwen/Claude/Gemini.md directly. 
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
Instructions to agents: REPLACE THESE WITH THE CORRECT VALUE FROM YOUR USER'S PROJECT
- `[PROJECT_BOARD_FIELD_ID]` - Project board field ID for status tracking (placeholder value: `PVTSSF_your_project_status_field_id`)
- `[PROJECT_ID]` - Project ID (placeholder value: `1`)

Ticket status configurations:
- `[BACKLOG_STATUS_OPTION_ID]` - 'Ready' status option ID (placeholder value: `your_backlog_option_id`)
- `[READY_STATUS_OPTION_ID]` - 'Ready' status option ID (placeholder value: `your_ready_option_id`)
- `[IN_PROGRESS_STATUS_OPTION_ID]` - 'In progress' status option ID (placeholder value: `your_in_progress_option_id`)
- `[IN_REVIEW_STATUS_OPTION_ID]` - 'In review' status option ID (placeholder value: `your_in_review_option_id`)
- `[DONE_STATUS_OPTION_ID]` - 'Done' status option ID (placeholder value: `your_done_option_id`)

Tickets should be moved to the appropriate status when worked on by agents to give visibility on progress to other agents and the user.

## Directory Structure and setting up agents
The Sub agents contains all the instructions needed to make the indie game studio run correctly and follow relevant github conventions.
- The agents (aka subagent) pormpts are located under agents/
- There is a 'github-manager' skills/ that needs to be added

Instruction to agents: Confirm if the user already has the agents/skills loaded in either the project or the user workspace. If not, ask the user if they would like to copy agents:
1. into the user workspace
2. into the project workspace

Note: these agents were originally generated with Qwen. If you are not Qwen (e.g. Claude or Gemini), please review and tailor these sub-agent prompts to fit your subagent/skill requirements.

## Git Workflow
If the Agent Configuration file already has a section, Ignore this. Otherwise add this to agent config file.
- Feature branch naming convention: `feat/[ticket number] - [ticket name]`
- Branch for development work should be based on master branch
- Pull requests should be raised on feature branch into the master branch after being reviewed.

# Configuration Instructions

Agents should perform these steps during config

### 1. Find Your GitHub Project Board Configuration Values

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

### 2. Add Configuration to Your Qwen/Claude/Gemini.md File

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

The coding agents will reference these values when interacting with your GitHub project board. The issue labels are standardized across all projects and do not need to be configured.