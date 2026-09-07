# Approved Architectural Decisions

# Case Management Registry (CMR)

Status: APPROVED BASELINE v1.2

---

# Purpose

This document provides a concise summary of all approved architectural decisions governing the Case Management Registry (CMR) platform.

Detailed rationale, context, and consequences are maintained in `architectural-decisions.md`.

This document serves as the quick-reference architecture baseline for Product Owners, Business Analysts, Architects, and Developers.

---

## AD-001

### System of Record

Case Management Registry (CMR) is the authoritative System of Record for all business case information.

CMR owns:

- Case Data
- Attachments
- Access Control
- Audit History
- Business Metadata
- Search Metadata

---

## AD-002

### Workflow Adapter Architecture

All workflow operations must occur through a Workflow Adapter Layer.

Direct workflow engine integration is prohibited.

Current implementation:

- CIBSeven Adapter

Future implementations:

- Camunda Adapter
- Flowable Adapter
- jBPM Adapter
- Custom Adapters

---

## AD-003

### Workflow Engine Independence

The platform shall remain independent from workflow engine implementations.

Workflow engines may be replaced without impacting business functionality.

---

## AD-004

### Technology Stack

Backend:

- Python
- FastAPI

Frontend:

- Jinja2
- HTMX

Database:

- PostgreSQL

Forms:

- JSON Forms

---

## AD-005

### Metadata-Driven Architecture

The platform shall be metadata-driven.

Metadata defines:

- Business Objects
- Case Types
- Forms
- Searches
- Detail Views
- Lifecycles
- Workflow Mappings
- Validation Rules
- UI Behavior Rules

---

## AD-006

### Configuration First

Configuration is preferred over code.

Custom Python extensions shall only be used when metadata cannot reasonably satisfy a requirement.

---

## AD-007

### Metadata-Defined Business Objects

New business objects shall be defined through metadata.

Examples:

- Complaint
- Appeal
- Change Request
- NGO Request
- Service Request

---

## AD-008

### Multiple Form Variants

A Case Type may define multiple forms.

Supported forms:

- Create Form
- Edit Form
- View Form
- Activity Form
- Closure Form

---

## AD-009

### Multi-Step Forms

The platform shall support metadata-driven wizard-style forms.

Supported capabilities:

- Save Draft
- Resume Later
- Conditional Steps
- Step Validation
- Review Screen

---

## AD-010

### Metadata Versioning

All metadata definitions shall support versioning.

Existing cases remain linked to the metadata version used at creation time.

---

## AD-011

### Metadata Governance

Metadata definitions shall follow a controlled lifecycle:

```text
Draft
→ Review
→ Approved
→ Published
→ Retired
```

Only Published metadata may be used in production.

---

## AD-012

### Lifecycle Definitions

Lifecycle behavior shall be metadata-driven.

Different Case Types may have different lifecycles.

Lifecycle behavior shall not be hardcoded.

---

## AD-013

### Metadata-Driven Search

Search behavior shall be defined through metadata.

Business Analysts may configure:

- Search Fields
- Filters
- Result Columns
- Sorting
- Saved Searches

---

## AD-014

### Search Returns Cases Only

Search results shall return Cases only.

Search results shall not directly return:

- Activities
- Attachments
- Comments
- Workflow Artifacts

---

## AD-015

### Explicit Case Access Model

Case visibility shall be managed through explicit access assignments.

Visibility shall not depend on workflow history alone.

---

## AD-016

### No Field-Level Security (Phase 1)

Security shall be enforced at the Case level.

Field-level security is excluded from Phase 1.

Potential future enhancements:

- Data Masking
- ABAC
- GDPR Controls
- Field-Level Security

---

## AD-017

### Permission-Based UI Behavior

The platform shall support metadata-driven UI behavior.

Supported behaviors:

- Visible
- Hidden
- Editable
- Read Only
- Required
- Optional
- Disabled

Behavior may be based on:

- Platform Roles
- Case Roles
- Activities
- Lifecycle Stages
- REST Rules
- Python Extensions

UI behavior is not a security mechanism.

---

## AD-018

### Dynamic Business Rules

Metadata may invoke REST services for:

- Validation
- Visibility
- Read-Only Rules
- Dynamic Dropdowns
- Hyperlinks
- Eligibility Checks
- Business Calculations

---

## AD-019

### Python Extension Points

Metadata may reference optional Python handlers for advanced requirements.

Examples:

- Complex Business Rules
- External Integrations
- Data Enrichment
- Eligibility Validation

Principle:

```text
Metadata First
+
Optional Extension Points
```

---

## AD-020

### Workflow Events

Metadata shall support lifecycle events.

Supported events:

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

---

## AD-021

### Business Terminology Model

Business users interact with:

- Cases
- Activities
- Participants
- Stages
- History

Business users do not interact with workflow terminology.

Examples:

```text
Case
instead of
Process Instance
```

```text
Activity
instead of
Task
```

---

## AD-022

### Hybrid Localization Strategy

Application translations shall be stored in translation files.

Examples:

- Menus
- Buttons
- Navigation
- System Messages

Metadata translations shall be stored within metadata definitions.

Examples:

- Case Types
- Field Labels
- Search Labels
- Lifecycle Labels
- Activity Labels
- Notification Templates

---

## AD-023

### Metadata Localization Storage

Metadata translations shall be stored as multilingual metadata values.

Example:

```json
{
  "label": {
    "en": "Request Title",
    "ar": "عنوان الطلب"
  }
}
```

Additional languages shall not require schema changes.

---

## AD-024

### Bidirectional Language Support

The platform shall automatically adapt to:

- RTL languages
- LTR languages

based on the active user language.

Initial languages:

- Arabic
- English

---

## AD-025

### Authentication, Authorization and Auditing Separation

Authentication determines:

- Who the user is

Authorization determines:

- What the user can access

Auditing records:

- What the user did

These concerns shall remain independent throughout the architecture.

---

# Baseline Status

Status: APPROVED BASELINE v1.2

This document is derived from the following frozen documents:

- README.md
- product-requirements-document.md
- solution-architecture.md
- security-and-access-model.md
- search-requirements.md
- metadata-model.md v1.2
- architectural-decisions.md v1.1
