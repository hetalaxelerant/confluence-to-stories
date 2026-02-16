---
name: confluence-to-stories
description: Read product requirements from Confluence via Atlassian MCP, then generate epics, user stories with acceptance criteria, and a sprint plan.
---

## Inputs
- A Confluence page URL (or space + page title).
- Jira project key (required before creating epics/stories).
- Optional: include child pages (yes/no).
- Sprint length (default 2 weeks).
- Team size + capacity assumptions (default 4 engineers, 20% overhead).

## Workflow
1) Ask the user for the Jira project key where epics/stories should be created.
2) Use Atlassian MCP to open the Confluence page.
3) If include child pages = yes, list and fetch child pages (depth 1).
4) Build a "Source Map": page title -> key takeaways (bullets).
5) Extract:
   - product goal
   - users/personas
   - business metrics / success metrics (KPIs)
   - in-scope requirements
   - out-of-scope
   - non-functional requirements
   - dependencies
   - assumptions and constraints
   - risks
   - open questions / ambiguities
6) Generate:
   - 3-8 epics
   - user stories under each epic (INVEST)
   - acceptance criteria for each story (Gherkin where useful)
   - rough estimates (T-shirt or story points)
7) Identify story-to-story relationships before issue creation:
   - mark "relates to" for cross-cutting dependencies, shared APIs/data contracts, common test requirements, and parallelizable stories that must stay aligned.
   - apply these heuristics:
     - UI/frontend story <-> API/backend story for the same user flow.
     - Data model/migration story <-> reporting/QA validation story that verifies that data.
     - Search/filter/sort stories in the same experience cluster.
     - Integration contract story <-> integration implementation story.
     - Accessibility/compliance story <-> template/page stories it governs.
     - Release/UAT story <-> critical P1 journey stories included in sign-off scope.
   - avoid linking stories that are only in the same epic but have no concrete shared behavior, contract, or risk.
8) Create epics and stories in Jira under the provided project key by default.
9) After stories are created, add Jira issue links of type "Relates" between relevant stories.
10) Propose a sprint plan:
   - sprint goal
   - prioritized stories for Sprint 1
   - capacity assumptions + rationale
   - dependencies, risks, and what needs clarification

## Output Rules
- Default output: Jira epics and stories created in the provided project.
- Default linking behavior: add Jira "relates to" links between relevant stories.
- If Jira project key is missing: ask for it before proceeding.
- Only create `STORIES.md` and `SPRINT_PLAN.md` when the user explicitly asks for file output.
