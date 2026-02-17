# Sprint Plan - ESC Functional Split (2 Weeks)

## Sprint Goal

Release a usable Phase 1 learner/company portal slice in Drupal by delivering dashboard, profile, certifications, catalog discovery, and stable handoffs to iMIS for checkout and legacy-critical functions.

## Capacity Assumptions

- Sprint length: 2 weeks (10 working days).
- Team size: 4 engineers.
- Overhead: 20% (ceremonies, support, reviews, coordination).
- Gross engineering hours: `4 x 10 x 8 = 320`.
- Net engineering hours: `320 x 0.8 = 256`.
- Planning conversion: `1 SP ~= 4 hours`.
- Effective capacity: `~64 SP`.
- Recommended commitment: `42 SP` (reserve `~22 SP` for integration uncertainty and hardening).

## Prioritized Backlog (from STORIES.md)

1. ESC-001 Role-based dashboard (8 SP)
2. ESC-003 Unified editable profile (8 SP)
3. ESC-004 Certifications/badges/preferences (8 SP)
4. ESC-005 Course/event search and detail (8 SP)
5. ESC-006 Registration redirect to iMIS cart (5 SP)
6. ESC-008 Open invoices view + pay redirect (3 SP)
7. ESC-009 Data Gateway handoff to iMIS (2 SP)

Total planned for Sprint 1: `42 SP`

## Stretch (if ahead)

1. ESC-007 Access code filter/export (5 SP)
2. ESC-011 Integration contract and fallback testing (3 SP)
3. ESC-002 Dashboard role-rule configurability (5 SP)

## Suggested Sprint 1 Sequence

1. Build core shell and role-aware dashboard first (ESC-001).
2. Deliver profile and certifications flows in parallel (ESC-003, ESC-004).
3. Complete catalog discovery and registration handoff (ESC-005, ESC-006).
4. Add financial and legacy continuity redirects (ESC-008, ESC-009).
5. Run integration smoke validation for all handoffs before demo.

## Dependencies

- iMIS Party/UserSecurity, IQA, and REST endpoint availability in test environment.
- Confirmed role matrix for dashboard widget composition.
- API credentials and redirect/session behavior validated across Drupal and iMIS.
- Invoice and cart deep-link contract confirmed.

## Risks

- Hybrid flows depend on multiple upstream systems with variable latency.
- Role-rule ambiguity can create late rework in dashboard and permissions.
- Certification preference edge cases can create data mismatches.
- Redirect/session handling failures can impact registration/payment conversion.

## Clarifications Needed

1. Confirm exact persona-role mapping for each dashboard widget.
2. Confirm minimum analytics events required for UAT and launch go/no-go.
3. Confirm access-code export format requirements (CSV, XLSX, both).
4. Confirm acceptable timeout/fallback thresholds for each integration path.

## Definition of Done (Sprint 1)

- Planned stories meet acceptance criteria in `/Users/hetalmistry/Documents/New project 3/STORIES.md`.
- UAT users can complete dashboard -> profile/certifications -> catalog -> register journey.
- Invoice and Data Gateway redirects function consistently.
- No open Sev-1 defects in committed scope.
- KPI instrumentation plan is documented for Sprint 2 execution.
