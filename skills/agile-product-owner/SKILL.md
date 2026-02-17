---
name: agile-product-owner
description: Agile product ownership toolkit for Senior Product Owner including INVEST-compliant user story generation, sprint planning, backlog management, and velocity tracking. Use for story writing, sprint planning, stakeholder communication, and agile ceremonies.
---

# Agile Product Owner

Complete toolkit for Product Owners to excel at backlog management and sprint execution.

## First-Prompt Setup Check (Mandatory)
1) On first user prompt in a session, ask: "Do you want to set up Jira/Confluence MCP connection now?"
2) If user says yes:
   - run Jira/Confluence MCP setup/auth steps
   - verify connection before continuing
3) If user says no, or setup fails:
   - continue in file-only mode
   - use local markdown outputs when requested
   - provide concise "set up later" guidance

## Mandatory Jira Behavior
- In connected Jira mode, before creating epics/stories/tasks, ask for Jira project key.
- Create backlog items in Jira by default once project key is provided.
- If not connected (or user opts out), continue without blocking in file-only mode.
- Only generate local `.md` story/sprint outputs when the user explicitly requests file output, or when Jira mode is unavailable.

## Core Capabilities
- INVEST-compliant user story generation
- Automatic acceptance criteria creation
- Sprint capacity planning
- Backlog prioritization
- Velocity tracking and metrics

## Key Scripts

### user_story_generator.py
Generates well-formed user stories with acceptance criteria from epics.

**Usage**:
- Generate stories: `python scripts/user_story_generator.py`
- Plan sprint: `python scripts/user_story_generator.py sprint [capacity]`

**Features**:
- Breaks epics into stories
- INVEST criteria validation
- Automatic point estimation
- Priority assignment
- Sprint planning with capacity
