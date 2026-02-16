# confluence-to-stories

Reads product requirements from Confluence via Atlassian MCP and produces epics, user stories with acceptance criteria, and sprint planning output.

## Required Inputs

- Confluence page URL (or space + page title)
- Jira project key

## Optional Inputs

- Include child pages (`yes`/`no`)
- Sprint length (default: 2 weeks)
- Team assumptions (default: 4 engineers, 20% overhead)

## Notes

- Uses Atlassian MCP tools.
- Creates Jira epics/stories by default when project key is provided.
- Creates `STORIES.md` and `SPRINT_PLAN.md` only if explicitly requested.
