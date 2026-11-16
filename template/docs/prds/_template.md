# PRD: [Feature or Product Name]

## Meta

- **Status:** [DRAFT | IN_REVIEW | APPROVED | SHIPPED]
- **Owner:** [Name]
- **Stakeholders:** [Product, Engineering, Design, QA, Other]
- **Target Release:** [Date, milestone, or sprint]
- **Related Docs:** [Links to ADRs, architecture docs, meeting notes, tickets, designs]
- **Last Updated:** [YYYY-MM-DD]

## 1. Summary

[Briefly describe the feature or product change in 2–4 sentences. Explain what will change for users, why it matters, and how it fits the product.]

## 2. Problem Statement

[What specific user or business problem are we solving? Why is this worth solving now?]

Consider:

- Who experiences the problem today?
- What workflow is blocked, slow, risky, or unclear?
- What happens if we do nothing?

## 3. Target Audience

- **Primary persona:** [Who uses this directly? What are they trying to accomplish?]
- **Secondary persona:** [Who is affected indirectly?]
- **Assumptions:** [Important assumptions about user skill level, environment, or workflow]

## 4. Goals and Success Metrics

### Goals

- [Goal 1: user or business outcome]
- [Goal 2: user or business outcome]

### Success Metrics

| Metric | Target | Measurement Method |
|--------|--------|--------------------|
| [Example: task completion rate] | [Example: ≥ 95%] | [Example: manual QA, automated test] |

## 5. Scope

### In Scope

- [Must-have capability 1]
- [Must-have capability 2]

### Out of Scope

- [Explicitly excluded capability]
- [Anything deferred to a later release]

### Non-Goals

- [What this PRD is not trying to optimise, support, or decide]

## 6. User Journeys

### Primary Flow

1. [User starts from…]
2. [User performs…]
3. [System responds by…]
4. [User completes the task when…]

### Edge or Error Flows

- [Invalid input, missing data, unavailable service, etc.]
- [Expected user-facing error message or recovery path]

## 7. Requirements and User Stories

| ID | User Story | Priority | Acceptance Criteria |
|----|-----------|----------|---------------------|
| REQ-001 | As a [persona], I want to [action], so that [benefit]. | Must / Should / Could | 1. [Criterion A]<br>2. [Criterion B] |
| REQ-002 | As a [persona], I want to [action], so that [benefit]. | Must / Should / Could | 1. [Criterion A]<br>2. [Criterion B] |

Acceptance criteria should be testable, unambiguous, and observable from the user's point of view.

## 8. UX and Design

- **Entry points:** [Where does the user access this feature?]
- **Core screens or components:** [List the screens, forms, dialogs, or states needed]
- **Design references:** [Links to sketches, screenshots, Figma, or component library docs]

## 9. API and Integration Expectations

Keep this product-level. Detailed endpoint contracts belong in architecture documentation.

- [Which backend capabilities are needed?]
- [Which frontend interactions depend on these capabilities?]
- [Expected request/response behaviour, validation errors, and status states]

## 10. Testing and Quality Expectations

- **Unit / integration tests:** [What logic must be covered?]
- **Manual QA:** [Critical paths to verify before release]
- **Documentation:** [README, API docs, setup instructions, or examples to update]

## 11. Dependencies and Assumptions

- [Dependency: library, service, design input, API, etc.]
- [Assumption: something that must be true for this PRD to remain valid]
- [Decision needed: anything that may require a new ADR]

## 12. Release Plan

- **Milestone 1:** [Planning, design, or spike]
- **Milestone 2:** [Core implementation]
- **Milestone 3:** [Testing and documentation]
- **Milestone 4:** [Polish and release]

## 13. Risks and Mitigations

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| [Risk 1] | High / Medium / Low | High / Medium / Low | [Mitigation] |

## 14. Open Questions

| Question | Owner | Due Date | Resolution |
|----------|-------|----------|------------|
| [Question 1] | [Name] | [YYYY-MM-DD] | Pending |

## 15. Readiness Checklist

- [ ] Problem, target audience, and success metrics are clear.
- [ ] In-scope and out-of-scope boundaries are explicit.
- [ ] User stories have testable acceptance criteria.
- [ ] UX flow is defined where relevant.
- [ ] Backend and integration expectations are clear enough for implementation.
- [ ] Required tests and documentation updates are identified.
- [ ] Open questions have owners.

## Appendix

[Supporting notes, research, links, glossary terms, or examples.]
