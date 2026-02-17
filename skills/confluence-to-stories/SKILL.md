---
name: confluence-to-stories
description: Read product requirements from Confluence via Atlassian MCP, then generate epics, user stories with acceptance criteria, and a sprint plan.
---

## Inputs
- A Confluence page URL (or space + page title).
- Jira project key (required before creating epics/stories in Jira).
- Optional: include child pages (yes/no).
- Sprint length (default 2 weeks).
- Team size + capacity assumptions (default 4 engineers, 20% overhead).

## First-Prompt Setup Check (Mandatory)
1) On first user prompt in a session, ask: "Do you want to set up Jira/Confluence MCP connection now?"
2) If user says yes:
   - run Jira/Confluence MCP setup/auth steps
   - verify connection before continuing
3) If user says no, or setup fails:
   - continue in file-only mode
   - use `STORIES.md` and `SPRINT_PLAN.md`
   - provide concise "set up later" guidance

## Workflow
1) Run first-prompt setup check.
2) Ask where output should go:
   - Jira mode: ask for Jira project key.
   - File-only mode: confirm local markdown output.
3) Use Atlassian MCP to open the Confluence page (if connected).
4) If include child pages = yes, list and fetch child pages (depth 1).
5) Build a "Source Map": page title -> key takeaways (bullets).
6) Extract:
   - product goal
   - users/personas
   - in-scope requirements
   - out-of-scope
   - non-functional requirements
   - dependencies
   - open questions / ambiguities
7) Generate:
   - 3-8 epics
   - user stories under each epic (INVEST)
   - acceptance criteria for each story (Gherkin where useful)
   - rough estimates (T-shirt or story points)
8) Deliver output by selected mode:
   - Jira mode: create epics and stories in Jira under the project key.
   - File-only mode: write/update `STORIES.md` and `SPRINT_PLAN.md`.
9) Propose sprint plan:
   - sprint goal
   - prioritized stories for Sprint 1
   - capacity assumptions + rationale
   - dependencies, risks, and what needs clarification

## Output Rules
- Default preference is Jira mode only when MCP is connected and project key is provided.
- If Jira project key is missing in Jira mode: ask before proceeding.
- If MCP is not connected: do not block the user; continue with file-only output.
