# Architectural Decision Records (ADR)

# Case Management Registry (CMR)

Status: APPROVED BASELINE v1.1

---

# Purpose

This document records significant architectural decisions made during the design and evolution of the Case Management Registry (CMR) platform.

Each ADR captures:

- Decision
- Context
- Rationale
- Consequences

All future architectural changes should either:

- Follow an existing ADR
- Amend an existing ADR
- Create a new ADR

---

# ADR-001

## Title

Case Management Registry as System of Record

### Decision

The Case Management Registry (CMR) shall be the authoritative System of Record for all business case information.

### Context

Workflow engines are optimized for workflow orchestration and execution, not long-term business data ownership.

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
- Easier workflow engine replacement

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

The platform must remain independent from workflow engine vendors.

### Initial Implementation

- CIBSeven Adapter

### Future Implementations

- Camunda Adapter
- Flowable Adapter
- jBPM Adapter
- Custom Workflow Adapters

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

### Backend

- Python
- FastAPI

### Frontend

- Jinja2
- HTMX

### Database

- PostgreSQL

### Forms

- JSON Forms

### Context

The platform requires:

- Metadata-driven UI generation
- Server-side rendering
- Long-term maintainability
- Rapid application development

### Consequences

Benefits:

- Reduced complexity
- Faster development
- Simpler deployment model
- Strong metadata-driven architecture support

---

# ADR-004

## Title

Metadata-Driven Architecture

### Decision

The platform shall be metadata-driven.

### Context

Business Analysts require the ability to introduce new case types and behaviors with minimal software development.

### Rationale

Metadata shall define:

- Forms
- Searches
- Detail Views
- Lifecycles
- Workflow Mappings
- Validation Rules
- UI Behavior Rules

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

A low-code platform requires controlled extensibility.

### Rationale

Most requirements should be solved through metadata.

Advanced requirements may use Python extensions.

### Consequences

Benefits:

- Lower maintenance cost
- Reduced custom development
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

The platform shall support metadata-driven UI behavior.

### Context

Different users require different levels of interaction with fields without introducing field-level security.

### Rationale

Controls may be:

- Visible
- Hidden
- Editable
- Read Only
- Required
- Optional
- Disabled

Based on:

- Platform Roles
- Case Roles
- Activities
- Lifecycle Stages
- REST Evaluation
- Python Extension Points

### Architectural Rule

UI behavior is not a security mechanism.

Security remains enforced at the Case level.

### Consequences

Benefits:

- Rich user experiences
- Simpler authorization model
- Highly configurable forms

---

# ADR-009

## Title

Search Returns Cases Only

### Decision

Search results shall return Cases only.

### Context

Returning activities, comments, attachments, and workflow artifacts increases complexity and weakens the business-focused experience.

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

Different Case Types require different search experiences.

### Rationale

Business Analysts should configure:

- Search Fields
- Filters
- Result Columns
- Sorting
- Saved Searches

without software changes.

### Consequences

Benefits:

- Rapid onboarding of new case types
- Low-code search customization

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

Different lifecycle stages and activities require different user experiences.

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
- Reduced abandonment rates
- Easier information organization

---

# ADR-014

## Title

Hybrid Localization Strategy

### Decision

The platform shall use a hybrid localization model.

### Application Translations

Stored in translation files under source control.

### Metadata Translations

Stored within metadata definitions.

### Context

Business terminology must be editable without software deployment.

### Consequences

Benefits:

- Low-code localization
- Easier administration
- Better separation of concerns

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

- Better internationalization
- Better accessibility
- Consistent user experience

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
- Change control

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
- Safe platform evolution

---

# ADR-018

## Title

Dynamic Business Rules and Extension Points

### Decision

The platform shall support dynamic business rules through REST integrations and optional Python extension points.

### Supported Scenarios

- Validation
- Visibility Rules
- Read-Only Rules
- Lookup Values
- Eligibility Checks
- Business Calculations
- External Integrations
- Data Enrichment

### Context

Business behavior often depends on external systems and advanced logic.

### Consequences

Benefits:

- Flexible integration
- Reduced custom development
- Strong extensibility model

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
- Reduced code dependencies

---

# ADR-020

## Title

Authentication, Authorization, and Auditing Separation

### Decision

Authentication, Authorization, and Auditing shall remain independent concerns.

### Definitions

Authentication

- Who the user is

Authorization

- What the user may access

Auditing

- What the user did

### Consequences

Benefits:

- Cleaner architecture
- Easier compliance
- Better governance

---

# ADR-021

## Title

Business Terminology Model

### Decision

Business users shall interact with business terminology rather than workflow terminology.

### Examples

Business Terms:

- Case
- Activity
- Participant
- Stage
- History

Workflow Terms:

- Process Instance
- Task
- Assignee
- Execution
- Runtime Variable

### Context

Workflow implementation details should remain hidden from business users.

### Consequences

Benefits:

- Improved usability
- Reduced BPM coupling
- Better business alignment

---

# ADR-022

## Title

Workflow Engine Independence

### Decision

The platform shall remain independent from workflow engine implementations.

Business services shall never directly invoke workflow engines.

All workflow interactions shall occur through Workflow Adapters.

### Context

Long-term platform sustainability requires workflow vendor independence.

### Consequences

Benefits:

- Architectural flexibility
- Easier migration
- Reduced vendor lock-in

---

# ADR-023

## Title

Metadata Localization Storage Strategy

### Decision

Metadata translations shall be stored as multilingual metadata values.

Application translations shall remain in translation files.

### Example

```json
{
  "label": {
    "en": "Request Title",
    "ar": "عنوان الطلب"
  }
}
```

### Context

Business terminology should be manageable by Business Analysts without deployment.

### Consequences

Benefits:

- No schema changes for additional languages
- Low-code localization
- Metadata ownership by business users

---

# ADR-024

## Title

Metadata-Defined Business Objects

### Decision

The platform shall support metadata-defined business objects.

### Examples

- Complaint
- Appeal
- Change Request
- NGO Request
- Service Request

### Context

New business capabilities should be introduced through metadata whenever possible.

### Consequences

Benefits:

- Faster onboarding of business services
- Reduced development effort
- Greater platform flexibility

---

# ADR Status

Status: APPROVED BASELINE v1.1

This document shall be updated whenever a significant architectural decision is made.
