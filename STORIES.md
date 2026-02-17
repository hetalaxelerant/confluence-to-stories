# ESC Portal Functional Split - Epics and User Stories

## Source Map

Page: All Roles - Functional Split (`https://axelerant.atlassian.net/wiki/spaces/ESC/pages/5538283549/All+Roles+-+Functional+Split`)
- Dashboard/Overview moves from iMIS to Drupal as a unified role-based experience.
- Profile becomes a hybrid flow: Drupal UI with iMIS Party/UserSecurity updates.
- Certifications and badges are hybrid: IQA read + REST preference updates.
- Access codes are hybrid with IQA fetch, table UX, filtering, and export.
- Courses and Events move to Drupal with REST-based catalog/search and iMIS cart redirect.
- Invoices stay in iMIS for payment; Drupal provides read-only visibility and redirect.
- Data Gateway remains in iMIS for Phase 1 due complex form logic; Phase 2 rebuild is possible.

Assumption
- Child pages were not included (depth 1 fetch skipped) because no child-page requirement was provided.

## Requirement Extraction

### Product Goal
- Deliver a role-aware ESC learner/company portal in Drupal that improves discoverability and self-service, while preserving iMIS reliability where needed.

### Users / Personas
- Learner (student/professional).
- Certification candidate.
- Company admin/training coordinator.
- Billing/finance contact.
- Internal operations/compliance user.

### In-Scope Requirements
- Build role-based Drupal dashboard.
- Build unified editable profile screen in Drupal with iMIS updates.
- Build certifications and badges view with preferences toggle behavior.
- Build access code table with search/filter/export.
- Build course/event search and details in Drupal with registration redirect.
- Show invoices in Drupal and redirect to iMIS payment.
- Maintain Data Gateway in iMIS with Drupal handoff.

### Out of Scope (Phase 1)
- Full Data Gateway rebuild in Drupal.
- Direct payment transaction processing in Drupal.
- Replacing complex iMIS financial/data workflows.

### Non-Functional Requirements
- Consistent role-based access control across new Drupal pages.
- Reliable integration contracts (IQA, REST, Party/UserSecurity).
- Read/write auditability for profile and preferences updates.
- Response-time targets for dashboard/catalog pages.

### Dependencies
- iMIS API/IQA endpoint readiness and access.
- Data mapping validation for Contact, Party, certifications, products, invoices.
- Redirect and SSO/session behavior between Drupal and iMIS.
- Security/privacy review for profile and invoice views.

### Open Questions / Ambiguities
- Exact role matrix for dashboard widgets by persona.
- SLA and timeout/fallback behavior for each integration endpoint.
- Export format requirements for access codes (CSV only or XLSX too).
- Final KPI baseline values and launch targets by module.

## Epics

1. Role-Based Experience Foundation in Drupal
2. Profile and Credential Self-Service
3. Catalog, Registration, and Access Code Operations
4. Financial and Legacy Continuity
5. Integration Quality, Analytics, and Rollout Readiness

## User Stories with Acceptance Criteria

### Story Format Standard
- Each story includes persona, business impact area, and measurable KPIs.
- Acceptance criteria are written in testable Given/When/Then form with explicit outcome expectations where possible.
- Each story includes assumptions, in scope, out of scope, non-functional requirements, dependencies, and open questions.

### Epic 1: Role-Based Experience Foundation in Drupal

Story ESC-001 (8 SP)
- Persona: Learner / Company Admin
- User Story: As a portal user, I want a role-based dashboard so that I can immediately see courses, certifications, badges, and reminders relevant to me.
- Business Goal/Impact: Improve first-session clarity and reduce time to meaningful action.
- Potential Business Impact Area: Engagement, retention, self-service adoption.
- Suggested Business Metrics: dashboard DAU/WAU, time-to-first-click, CTA completion rate.
- Outcome Hypothesis: Faster visibility of next actions will increase first-session task completion and reduce abandonment.
- Acceptance Criteria:
  - Given an authenticated learner, when the dashboard loads, then learner-specific widgets (courses, certifications, badges, reminders) are shown.
  - Given an authenticated company admin, when the dashboard loads, then company-relevant widgets are shown and learner-only widgets are hidden.
  - Given at least one upstream data source fails, when the dashboard loads, then affected widgets show fallback messaging and unaffected widgets remain usable.
  - Given dashboard load is complete, when measured in non-prod monitoring, then P95 page load is within agreed performance target.
- Assumptions:
  - Role mapping for Learner and Company Admin is finalized before build start.
  - Required widget data is available from iMIS-backed sources.
- In Scope:
  - Role-based widget composition for Learner and Company Admin.
  - Dashboard fallback messaging for partial data failures.
  - Dashboard performance monitoring at page level.
- Out of Scope:
  - Advanced personalization beyond role-based rules.
  - Net-new analytics dashboards outside agreed KPI tracking.
- Non-Functional Requirements:
  - Meet agreed dashboard P95 load target.
  - Preserve usability when one or more widget data sources fail.
- Dependencies:
  - Final role-to-widget matrix.
  - iMIS integration endpoints for course/certification/badge/reminder data.
- Open Questions:
  - Which widgets are mandatory vs optional for each role at launch?
  - What is the exact launch P95 page-load target?

Story ESC-002 (5 SP)
- Persona: Internal Operations User
- User Story: As an operations user, I want configurable dashboard widget rules by role so that the portal can evolve without code changes for every role adjustment.
- Business Goal/Impact: Reduce operational lead time for role-content changes.
- Potential Business Impact Area: Operational agility, maintenance efficiency.
- Suggested Business Metrics: role-config change lead time, number of role-change releases avoided.
- Outcome Hypothesis: Configuration-driven role management will reduce delivery cycle time for role changes and lower engineering dependency.
- Acceptance Criteria:
  - Given role-widget configuration exists, when role mapping is changed and published, then new mapping is reflected on next user session.
  - Given invalid configuration, when publish is attempted, then validation blocks publish and returns actionable error messages.
  - Given configuration changes are applied, when audited, then change history captures who changed what and when.
- Assumptions:
  - Authorized admin roles for configuration changes are defined.
  - Configuration storage model is available in Drupal.
- In Scope:
  - Role-widget configuration management.
  - Validation and publish guardrails.
  - Audit trail for configuration changes.
- Out of Scope:
  - Full no-code workflow engine for all portal behavior.
  - Bulk import tooling for role configuration.
- Non-Functional Requirements:
  - Configuration updates should apply without code deployment.
  - Audit records must be immutable and queryable.
- Dependencies:
  - RBAC policy for config administration.
  - Logging/auditing mechanism in target platform.
- Open Questions:
  - Is config activation immediate or release-window controlled?
  - What retention period is required for config audit history?

### Epic 2: Profile and Credential Self-Service

Story ESC-003 (8 SP)
- Persona: Learner
- User Story: As a learner, I want one editable profile screen for personal and professional information so that my account is accurate without navigating multiple pages.
- Business Goal/Impact: Improve profile data quality and lower support load.
- Potential Business Impact Area: Data quality, support efficiency.
- Suggested Business Metrics: profile completion rate, profile update success rate, profile-related ticket volume.
- Outcome Hypothesis: A single profile workflow will increase profile completeness and reduce support tickets caused by stale user data.
- Acceptance Criteria:
  - Given profile fields are displayed in Drupal, when a user edits and saves valid data, then updates persist through Party/UserSecurity endpoints.
  - Given invalid values, when user saves, then inline validation identifies specific fields and required corrections.
  - Given update success, when page reloads, then latest saved values are visible and match iMIS records.
  - Given endpoint failure, when save is attempted, then user sees non-destructive error handling and retry guidance.
- Assumptions:
  - Field-level mappings between Drupal profile UI and iMIS Party/UserSecurity are approved.
  - Existing identity linkage between Drupal and iMIS user records is stable.
- In Scope:
  - Unified personal/professional profile edit experience.
  - Validation and safe error handling for save failures.
  - Read-after-write consistency checks.
- Out of Scope:
  - Redesign of iMIS source data model.
  - Bulk profile administration features.
- Non-Functional Requirements:
  - PII handling must comply with security/privacy policy.
  - Save operation reliability should meet agreed success threshold.
- Dependencies:
  - Party/UserSecurity endpoint access and schema contracts.
  - SSO identity mapping and session continuity.
- Open Questions:
  - Which profile fields are editable vs read-only in Phase 1?
  - Are approval workflows required for any profile updates?

Story ESC-004 (8 SP)
- Persona: Certification Candidate
- User Story: As a certification candidate, I want a single certifications page with badges and preferences so that I can manage my credential status and visibility.
- Business Goal/Impact: Increase credential completion and badge activation.
- Potential Business Impact Area: Credential adoption, learner outcomes.
- Suggested Business Metrics: certification completion rate, badge claim rate, preference update rate.
- Outcome Hypothesis: Centralized credential management will improve credential adoption and increase badge-sharing behavior.
- Acceptance Criteria:
  - Given certification data is available in IQA, when page loads, then active certifications and badge eligibility display correctly for the logged-in user.
  - Given preference toggles are changed, when saved, then preferences persist through REST and remain after refresh.
  - Given badges or training records exist, when CTA is clicked, then add-to-wallet/record workflow opens with correct credential context.
  - Given no credentials are available, when page loads, then empty-state guidance is displayed with next-step actions.
- Assumptions:
  - IQA returns certification/badge data at required freshness.
  - Preference update endpoints support required fields and validation.
- In Scope:
  - Credential and badge visibility.
  - Preference toggle updates and persistence.
  - Empty states and CTA routing for credential actions.
- Out of Scope:
  - New badge issuance rules.
  - Third-party wallet integrations beyond approved CTAs.
- Non-Functional Requirements:
  - Data shown must reflect latest available IQA snapshot.
  - Preference changes must be auditable.
- Dependencies:
  - IQA query definitions for certification/badge entities.
  - REST endpoints for preference updates.
- Open Questions:
  - What is acceptable data freshness lag for credentials?
  - Which CTAs require external legal/compliance review?

### Epic 3: Catalog, Registration, and Access Code Operations

Story ESC-005 (8 SP)
- Persona: Prospective Learner
- User Story: As a prospective learner, I want to search and view courses/events in Drupal so that I can find relevant training quickly.
- Business Goal/Impact: Improve course discovery and progression to registration.
- Potential Business Impact Area: Conversion, catalog engagement, revenue enablement.
- Suggested Business Metrics: search-to-detail CTR, catalog bounce rate, detail-to-register click-through.
- Outcome Hypothesis: Improved discovery flow will increase qualified traffic to course details and boost registration intent.
- Acceptance Criteria:
  - Given course/event data is available via REST, when users apply supported filters, then matching results are returned consistently.
  - Given a course card is selected, when detail page opens, then required metadata (title, date, format, location/modality, CTA) is shown.
  - Given no results, when filters are applied, then a clear empty state is displayed with reset option.
  - Given search/filter interaction events, when analytics is reviewed, then search, filter, and detail-click events are captured.
- Assumptions:
  - Course/event catalog APIs expose required searchable fields.
  - Search/filter taxonomy is finalized before implementation.
- In Scope:
  - Search and filtering in Drupal.
  - Course/event detail navigation.
  - Analytics instrumentation for discovery funnel.
- Out of Scope:
  - AI recommendations or personalized ranking.
  - Multi-language search enhancements.
- Non-Functional Requirements:
  - Search responses should meet agreed latency targets.
  - Result ordering and filtering must be deterministic.
- Dependencies:
  - REST catalog endpoints and field-level contracts.
  - Analytics event schema and tagging strategy.
- Open Questions:
  - What default sort should apply per persona/use case?
  - Are there hard limits for filter combinations in Phase 1?

Story ESC-006 (5 SP)
- Persona: Learner
- User Story: As a learner, I want registration actions to redirect to the iMIS cart with correct context so that checkout can continue reliably.
- Business Goal/Impact: Preserve enrollment completion reliability during hybrid architecture.
- Potential Business Impact Area: Revenue continuity, conversion reliability.
- Suggested Business Metrics: register-click-to-cart success rate, registration drop-off rate at redirect.
- Outcome Hypothesis: Reliable cart handoff will protect conversion by minimizing drop-offs at the boundary between Drupal and iMIS.
- Acceptance Criteria:
  - Given a selected course/event, when user clicks register, then redirect opens iMIS cart for the intended offering with correct product/event identifier.
  - Given redirect failure, when it occurs, then user sees recovery guidance and retry option without losing page context.
  - Given redirect success/failure events, when tracked, then conversion funnel metrics are available by offering.
- Assumptions:
  - iMIS cart supports deep linking with product/event context.
  - Session handoff between Drupal and iMIS is supported for target user roles.
- In Scope:
  - Register CTA handoff with context preservation.
  - Failure recovery messaging and retry behavior.
  - Redirect funnel event tracking.
- Out of Scope:
  - Rebuild of checkout/payment flow inside Drupal.
  - Promotions/coupon logic changes in iMIS cart.
- Non-Functional Requirements:
  - Redirect success rate should meet agreed threshold.
  - Handoff should not expose sensitive data in URL/query params.
- Dependencies:
  - iMIS cart URL contract and accepted identifiers.
  - SSO/session and environment configuration.
- Open Questions:
  - Should failed handoffs auto-retry or remain user-initiated only?
  - How should abandoned-cart attribution be handled across systems?

Story ESC-007 (5 SP)
- Persona: Company Admin / Learner
- User Story: As a user, I want access code order details with filtering and export so that I can reconcile and share records efficiently.
- Business Goal/Impact: Reduce manual reporting effort and support tickets.
- Potential Business Impact Area: Admin productivity, support deflection.
- Suggested Business Metrics: lookup completion time, export adoption rate, access-code support tickets.
- Outcome Hypothesis: Better self-service reporting for access codes will reduce manual reconciliation effort and support contacts.
- Acceptance Criteria:
  - Given access-code order data exists, when filters are applied, then result table updates to reflect selected criteria.
  - Given filtered results, when export is requested, then exported file contains only current filtered rows and expected columns.
  - Given unauthorized user, when page is accessed, then access is denied and event is logged.
  - Given large result sets, when filters or pagination are used, then interaction remains within agreed response target.
- Assumptions:
  - Access code data includes stable identifiers and exportable fields.
  - User roles for company admin vs learner data visibility are defined.
- In Scope:
  - Filterable/paginated access code table.
  - Export aligned to currently filtered dataset.
  - Access denial and event logging for unauthorized attempts.
- Out of Scope:
  - Write/update actions on access code orders.
  - Scheduled/report subscription automation.
- Non-Functional Requirements:
  - Large result interactions should meet agreed responsiveness target.
  - Exports must be generated with consistent schema and encoding.
- Dependencies:
  - IQA queries for CsOrders/CsOrderLines/CsProduct.
  - RBAC enforcement and audit logging.
- Open Questions:
  - Is CSV sufficient at launch, or is XLSX mandatory?
  - What is the maximum expected row count per export?

### Epic 4: Financial and Legacy Continuity

Story ESC-008 (3 SP)
- Persona: Billing Contact
- User Story: As a billing contact, I want to see open invoices in Drupal and pay via iMIS so that I can act quickly while using trusted payment flows.
- Business Goal/Impact: Improve invoice visibility without disrupting current payment process.
- Potential Business Impact Area: Cash flow, payment completion.
- Suggested Business Metrics: invoice-view-to-pay click rate, payment completion rate, invoice aging.
- Outcome Hypothesis: Invoice visibility in Drupal will shorten payment initiation time and improve on-time collections.
- Acceptance Criteria:
  - Given open invoices exist, when invoice view loads, then invoice list, status, amount, and due date are visible in Drupal.
  - Given a selected invoice, when user clicks pay, then user is redirected to matching iMIS payment flow for that invoice.
  - Given no open invoices, when view loads, then empty state clearly communicates account status.
- Assumptions:
  - Invoice data feed includes amount, status, and due date.
  - iMIS payment links can be mapped one-to-one to invoice records.
- In Scope:
  - Read-only invoice listing in Drupal.
  - Redirect to iMIS payment flow for selected invoice.
  - Empty-state behavior for no dues.
- Out of Scope:
  - Payment processing or invoice adjustments in Drupal.
  - Dispute/credit workflows.
- Non-Functional Requirements:
  - Financial data displayed must match iMIS source of truth.
  - Payment redirect behavior must be reliable and trackable.
- Dependencies:
  - Invoice API/IQA data contract.
  - iMIS payment route and identifier mapping.
- Open Questions:
  - Are partial payments in scope at all for Phase 1?
  - Should past paid invoices be visible or only open invoices?

Story ESC-009 (2 SP)
- Persona: Operations / Compliance User
- User Story: As an operations user, I want Data Gateway to remain in iMIS during Phase 1 so that critical complex forms continue without migration risk.
- Business Goal/Impact: Protect service continuity while sequencing riskier rebuild work later.
- Potential Business Impact Area: Reliability, risk containment.
- Suggested Business Metrics: Data Gateway completion rate, incident count, form failure rate.
- Outcome Hypothesis: Keeping Data Gateway in iMIS during Phase 1 will avoid service disruption while core Drupal capabilities launch.
- Acceptance Criteria:
  - Given a user navigates to Data Gateway, when action is initiated in Drupal, then handoff to iMIS succeeds with preserved user context.
  - Given iMIS is unavailable, when handoff fails, then user receives clear error messaging and support path.
  - Given handoff attempts, when monitored, then success/failure rates are reported weekly.
- Assumptions:
  - Data Gateway remains fully supported in iMIS during Phase 1.
  - Link/handoff path from Drupal can preserve user context.
- In Scope:
  - Drupal-to-iMIS Data Gateway handoff.
  - Failure messaging and support escalation path.
  - Weekly handoff reliability reporting.
- Out of Scope:
  - Reimplementation of Data Gateway forms in Drupal.
  - Workflow redesign for safety data processes.
- Non-Functional Requirements:
  - Handoff reliability should meet agreed uptime/success targets.
  - Failures should be observable via operational monitoring.
- Dependencies:
  - iMIS Data Gateway availability.
  - SSO/session continuity between systems.
- Open Questions:
  - What threshold triggers incident escalation for handoff failures?
  - Is there a fallback offline process for extended outages?

### Epic 5: Integration Quality, Analytics, and Rollout Readiness

Story ESC-010 (5 SP)
- Persona: Product Owner / Delivery Lead
- User Story: As a product owner, I want module-level analytics for each functional area so that we can measure business impact against rollout goals.
- Business Goal/Impact: Enable outcome-based prioritization after launch.
- Potential Business Impact Area: Product decision quality, ROI tracking.
- Suggested Business Metrics: module adoption rate, task success rate, persona-level funnel conversion, support contact rate.
- Outcome Hypothesis: Trusted analytics will enable outcome-driven prioritization and faster corrective decisions after launch.
- Acceptance Criteria:
  - Given key journeys are instrumented, when users perform actions, then events are captured with persona and module dimensions.
  - Given reporting cadence, when weekly report is generated, then each module KPI trend is available with prior-week comparison.
  - Given missing/invalid events, when QA runs analytics checks, then gaps are flagged and assigned before release.
- Assumptions:
  - Analytics tooling and data warehouse/reporting access are available.
  - Persona and module taxonomy for reporting is finalized.
- In Scope:
  - Event instrumentation for key journeys.
  - Weekly KPI reporting with trend comparison.
  - Analytics QA checks before release.
- Out of Scope:
  - Predictive analytics/ML scoring in Phase 1.
  - Enterprise BI model redesign.
- Non-Functional Requirements:
  - Event data quality must meet agreed completeness threshold.
  - Reporting outputs should be available within agreed refresh window.
- Dependencies:
  - Analytics SDK/tag manager implementation.
  - Reporting layer ownership and governance.
- Open Questions:
  - Which KPIs are hard launch gates vs monitor-only?
  - What attribution model will be used for cross-system journeys?

Story ESC-011 (3 SP)
- Persona: QA Engineer
- User Story: As a QA engineer, I want integration contract and fallback testing across IQA/REST/iMIS handoffs so that hybrid flows are resilient in production.
- Business Goal/Impact: Reduce production incidents caused by upstream dependency failures.
- Potential Business Impact Area: Quality, reliability, incident prevention.
- Suggested Business Metrics: integration test pass rate, incident rate by integration point, MTTR.
- Outcome Hypothesis: Strong pre-release integration validation will reduce production incidents and recovery time.
- Acceptance Criteria:
  - Given contract tests, when upstream schema/field mismatch occurs, then failure is detected before release.
  - Given timeout or API error, when triggered in test, then user-facing fallback behavior matches requirements.
  - Given critical integration test failures, when release readiness is reviewed, then go-live is blocked until defects are triaged and accepted.
- Assumptions:
  - Test environments can simulate upstream failures and schema mismatches.
  - Contract baselines are versioned and available to QA.
- In Scope:
  - Contract tests for IQA/REST/iMIS handoffs.
  - Failure-mode and fallback-behavior validation.
  - Release gate policy based on integration criticality.
- Out of Scope:
  - Full performance/load certification across all systems.
  - Third-party penetration/security testing not tied to story scope.
- Non-Functional Requirements:
  - Integration test suite should run reliably in CI.
  - Critical-path tests should complete within agreed pipeline time budget.
- Dependencies:
  - Stable non-prod integrations and seeded test data.
  - CI pipeline support for integration test execution and reporting.
- Open Questions:
  - Which failures are auto-blockers vs PO-approved exceptions?
  - What minimum pass rate is required for release approval?

## Backlog Notes
- Prioritize dashboard, profile, certifications, and catalog flows first due highest user-visible impact.
- Keep invoice payment and Data Gateway unchanged in iMIS for Phase 1 risk control.
- Track each story against at least one business KPI from day one of UAT.
- Split any story above 8 SP during sprint planning into independently testable slices.
- Apply Definition of Done: acceptance criteria met, analytics events verified, and cross-system handoff smoke-tested.
