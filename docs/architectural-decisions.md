# Architectural Decision Records (ADR)

# Case Management Registry (CMR)

Status: APPROVED BASELINE v1.0

---

# Purpose

This document records significant architectural decisions made during the design and evolution of the Case Management Registry (CMR) platform.

Each ADR captures:

- Decision
- Context
- Rationale
- Consequences

All future architectural changes should either:

- Follow an existing ADR, or
- Create a new ADR.

---

# ADR-001

## Title

Case Management Registry as System of Record

### Decision

The Case Management Registry (CMR) shall be the authoritative System of Record for all business case information.

### Context

Workflow engines are optimized for workflow execution and not long-term business data management.

### Rationale

CMR must own:

- Case Data
- Attachments
- Access Control
- Audit History
- Search Metadata
- Business Metadata

### Consequences

Benefits:

- Workflow independence
- Better search capabilities
- Easier reporting
- Easier migration between workflow engines

---

# ADR-002

## Title

Workflow Adapter Architecture

### Decision

All workflow interactions shall occur through a Workflow Adapter Layer.

### Context

The platform should not become dependent on a single workflow engine implementation.

### Rationale

Workflow engines may change over time.

The platform must remain independent.

### Initial Implementation

- CIBSeven Adapter

### Future Implementations

- Camunda Adapter
- Flowable Adapter
- jBPM Adapter
- Custom Adapters

### Consequences

Benefits:

- Vendor independence
- Easier migrations
- Better testability

---

# ADR-003

## Title

Technology Stack Selection

### Decision

The platform shall use:

Backend:

- Python
- FastAPI

Frontend:

- Jinja2
- HTMX

Database:

- PostgreSQL

### Context

The platform requires rapid development, metadata-driven UI generation, and long-term maintainability.

### Rationale

FastAPI provides:

- Excellent performance
- Modern architecture
- OpenAPI integration

Jinja2 + HTMX provides:

- Server-side rendering
- Low complexity
- Excellent metadata-driven rendering support

### Consequences

Benefits:

- Reduced complexity
- Faster development
- Simpler deployment model

---

# ADR-004

## Title

Metadata-Driven Architecture

### Decision

The platform shall be metadata-driven.

### Context

Business Analysts need the ability to introduce new case types with minimal software development.

### Rationale

Metadata shall define:

- Forms
- Searches
- Detail Views
- Lifecycles
- Workflow Mappings
- Validation Rules

### Consequences

Benefits:

- Faster delivery
- Reduced coding effort
- Increased flexibility

---

# ADR-005

## Title

Configuration First, Extension Second

### Decision

Configuration shall be preferred over custom development.

Python extension points shall only be used when metadata cannot satisfy requirements.

### Context

A low-code platform still requires a controlled method for implementing advanced behavior.

### Rationale

Most requirements should be solved through metadata.

Complex requirements may use Python extensions.

### Consequences

Benefits:

- Lower maintenance cost
- Reduced custom coding
- Flexible extensibility

---

# ADR-006

## Title

Explicit Case Access Model

### Decision

Case visibility shall be managed through explicit access assignments.

### Context

Workflow history does not reliably determine who should continue seeing a case.

### Rationale

Access control must remain independent from workflow execution.

### Consequences

Benefits:

- Predictable authorization
- Easier search security
- Better auditing

---

# ADR-007

## Title

No Field-Level Security in Phase 1

### Decision

Security shall be enforced at the Case level.

Field-level security shall not be implemented in Phase 1.

### Context

Field-level security significantly increases complexity.

### Rationale

The initial platform should focus on reliable case-level authorization.

Future compliance requirements may introduce:

- Field-Level Security
- Data Masking
- ABAC
- GDPR Controls

### Consequences

Benefits:

- Simpler architecture
- Simpler metadata model
- Simpler search implementation

---

# ADR-008

## Title

Permission-Based UI Behavior

### Decision

The platform shall support metadata-driven UI behavior control.

### Context

Businesses require different levels of interaction for fields without introducing field-level security.

### Rationale

Controls may be:

- Visible
- Hidden
- Editable
- Read Only
- Required
- Optional
- Disabled

based on:

- Platform Roles
- Case Roles
- Activities
- Lifecycle Stages

### Architectural Rule

UI behavior is not a security mechanism.

### Consequences

Benefits:

- Rich user experiences
- Simplified authorization model

---

# ADR-009

## Title

Search Returns Cases Only

### Decision

Search results shall return Cases only.

### Context

Returning activities, comments, attachments, and workflow artifacts increases complexity.

### Rationale

Users work with Cases as the primary business object.

### Consequences

Benefits:

- Simpler search experience
- Consistent terminology
- Better security enforcement

---

# ADR-010

## Title

Metadata-Driven Search

### Decision

Search behavior shall be defined through metadata.

### Context

New Case Types require different search experiences.

### Rationale

Business Analysts should configure:

- Search Fields
- Filters
- Result Columns
- Sorting

without code changes.

### Consequences

Benefits:

- Rapid onboarding of new case types

---

# ADR-011

## Title

Lifecycle Definitions Managed Through Metadata

### Decision

Lifecycle behavior shall be configured through metadata.

### Context

Different Case Types require different lifecycle models.

### Rationale

Lifecycle behavior should not be hardcoded.

### Examples

Complaint Lifecycle

```text
Draft
→ Submitted
→ Investigation
→ Resolved
→ Closed
```

Change Request Lifecycle

```text
Draft
→ Submitted
→ Technical Review
→ CAB Approval
→ Implementation
→ Closed
```

### Consequences

Benefits:

- Flexible lifecycle design
- Business Analyst ownership

---

# ADR-012

## Title

Multiple Form Variants Per Case Type

### Decision

A Case Type may define multiple form definitions.

### Supported Forms

- Create Form
- Edit Form
- View Form
- Activity Form
- Closure Form

### Context

Different lifecycle stages require different user experiences.

### Consequences

Benefits:

- Better usability
- Better workflow integration

---

# ADR-013

## Title

Multi-Step Forms

### Decision

The platform shall support metadata-driven multi-step forms.

### Context

Government and enterprise services frequently require staged data collection.

### Consequences

Benefits:

- Better user experience
- Reduced submission abandonment

---

# ADR-014

## Title

Hybrid Localization Strategy

### Decision

The platform shall use a hybrid localization model.

### Application Translations

Stored in translation files.

Examples:

- Save
- Cancel
- Search
- Dashboard

### Metadata Translations

Stored within metadata definitions.

Examples:

- Case Types
- Form Labels
- Search Labels
- Lifecycle Labels

### Context

Business terminology must be editable without software deployment.

### Consequences

Benefits:

- Low-code localization
- Easier administration

---

# ADR-015

## Title

RTL and LTR Support

### Decision

The platform shall dynamically adapt to the writing direction of the active language.

### Supported Directions

- RTL
- LTR

### Context

The platform must support multilingual operation.

### Consequences

Benefits:

- Better internationalization support
- Better accessibility

---

# ADR-016

## Title

Metadata Governance Lifecycle

### Decision

Metadata definitions shall follow a governance lifecycle.

### Lifecycle

```text
Draft
→ Review
→ Approved
→ Published
→ Retired
```

### Context

Uncontrolled metadata changes introduce operational risk.

### Consequences

Benefits:

- Governance
- Auditability
- Change Control

---

# ADR-017

## Title

Metadata Versioning

### Decision

All metadata definitions shall support versioning.

### Context

Existing cases must continue functioning after metadata changes.

### Consequences

Benefits:

- Backward compatibility
- Auditable history

---

# ADR-018

## Title

Dynamic Business Rules Through REST

### Decision

Metadata may invoke REST services for business rules.

### Supported Scenarios

- Validation
- Visibility
- Read Only Rules
- Lookup Values
- Eligibility Checks
- Business Calculations

### Context

Business logic often depends on external systems.

### Consequences

Benefits:

- Flexible integration
- Reduced custom development

---

# ADR-019

## Title

Workflow Events

### Decision

Metadata shall support workflow and form lifecycle events.

### Supported Events

- Form Load
- Step Enter
- Step Exit
- Before Save
- After Save
- Before Submit
- After Submit
- Before Start Workflow
- After Start Workflow
- Before Complete Activity
- After Complete Activity

### Consequences

Benefits:

- Configurable integrations
- Flexible automation

---

# ADR-020

## Title

Authentication, Authorization, and Auditing Separation

### Decision

Authentication, Authorization, and Auditing shall remain independent concerns.

### Context

These responsibilities are frequently coupled incorrectly.

### Definitions

Authentication:

- Who the user is

Authorization:

- What the user may access

Auditing:

- What the user did

### Consequences

Benefits:

- Cleaner architecture
- Easier compliance
- Better governance

---

# ADR Status

Status: APPROVED BASELINE v1.0

This document shall be updated whenever a significant architectural decision is made.
