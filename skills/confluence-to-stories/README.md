# confluence-to-stories

Reads product requirements from Confluence via Atlassian MCP and produces epics, user stories with acceptance criteria, and sprint planning output.

## Required Inputs

- Confluence page URL (or space + page title)
- Jira project key

## Optional Inputs

- Include child pages (`yes`/`no`)
- Sprint length (default: 2 weeks)
- Team assumptions (default: 4 engineers, 20% overhead)

## Extraction Coverage

The skill extracts and structures key planning inputs from Confluence, including:

- Product goal and problem statement
- Users/personas and primary journeys
- Business metrics / success metrics (KPIs) and expected outcomes
- In-scope vs out-of-scope requirements
- Non-functional requirements (performance, security, accessibility, compliance)
- Dependencies, assumptions, and constraints
- Risks, open questions, and ambiguities

## Notes

- Uses Atlassian MCP tools.
- Creates Jira epics/stories by default when project key is provided.
- Links relevant stories to each other using Jira issue link type `Relates`.
- Uses consistent linking heuristics (for example: UI<->API for same flow, migration<->QA validation, integration contract<->implementation, accessibility<->templates, UAT<->P1 journeys).
- Creates `STORIES.md` and `SPRINT_PLAN.md` only if explicitly requested.
