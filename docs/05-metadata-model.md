# Metadata Model

# Case Management Registry (CMR)

Status: APPROVED FOUNDATION v1.0

---

# Document Purpose

The Metadata Model is the foundation of the Case Management Registry (CMR) platform.

The platform is metadata-driven and low-code by design.

Business functionality should be delivered through metadata whenever possible instead of custom development.

The metadata model drives:

- Case Types
- Forms
- Multi-Step Forms
- Search Screens
- Detail Screens
- Validation Rules
- Lifecycle Definitions
- Localization
- Workflow Mappings
- Workflow Events
- Access Rules
- Dashboard Widgets

---

# Design Principles

## Single Source of Truth

A business object definition shall drive:

- Data Model
- User Interface
- Search Experience
- Workflow Integration
- Lifecycle Behavior
- Reporting Experience

---

## Configuration Over Coding

New business capabilities should be introduced through metadata whenever possible.

Custom development should only be required when metadata cannot reasonably satisfy business requirements.

---

## Engine Independence

Metadata shall remain independent from any specific workflow engine.

Workflow integration shall occur through the Workflow Adapter Layer.

Current Implementation:

- CIBSeven Adapter

Future Implementations:

- Camunda Adapter
- Flowable Adapter
- jBPM Adapter
- Custom Workflow Adapters

---

## Metadata First

Configuration should solve the majority of business requirements.

Extension points should only be used when configuration alone is insufficient.

---

## Versioned Configuration

Metadata definitions must support versioning.

Changes to metadata shall not impact previously created cases.

Existing cases shall remain linked to the metadata version used at creation time.

---

## Localization Ready

All user-facing metadata shall support multilingual values.

Initial Supported Languages:

- Arabic
- English

The platform shall automatically support:

- RTL languages
- LTR languages

based on the active language.

---

# Core Metadata Components

## Entity Definition

Defines the underlying business object.

Example:

```json
{
  "id": "change-request",
  "name": "Change Request",
  "version": 1,
  "category": "Administrative"
}
```

Properties:

- id
- name
- description
- category
- version
- active
- tags

---

## Case Type Definition

Defines a business category of cases.

Examples:

- Complaint
- Appeal
- Change Request
- Service Request
- NGO Request

Responsibilities:

- Business Name
- Business Description
- Form Definitions
- Search Definitions
- Lifecycle Definition
- Workflow Mapping
- Detail View Definition

---

## Field Definition

Defines business attributes.

Example:

```json
{
  "id": "title",
  "type": "string",
  "required": true,
  "searchable": true
}
```

Supported Field Types:

- string
- text
- integer
- decimal
- boolean
- currency
- date
- datetime
- select
- multiselect
- user
- department
- email
- phone
- url
- richtext
- attachment

Additional field types may be added in future releases.

---

## Form Definition

Defines data-entry screens.

Example:

```json
{
  "entity": "change-request",
  "layout": "two-column"
}
```

Capabilities:

- Sections
- Tabs
- Conditional Visibility
- Conditional Validation
- Read-Only Rules
- Dynamic Hyperlinks
- Attachment Uploads

---

## Multi-Step Form Definition

The platform shall support wizard-style forms driven entirely by metadata.

Example:

```json
{
  "steps": [
    {
      "id": "request-info",
      "title": "Request Information"
    },
    {
      "id": "business-details",
      "title": "Business Details"
    },
    {
      "id": "attachments",
      "title": "Attachments"
    }
  ]
}
```

Capabilities:

- Next / Previous Navigation
- Save Draft
- Resume Later
- Conditional Step Visibility
- Step Validation
- Review Screen
- Completion Indicators

---

## Form Variants

A Case Type may define multiple forms.

Examples:

```text
Create Form
Edit Form
View Form
Closure Form
Activity Form
```

This allows different user experiences across the case lifecycle.

---

## Search Definition

Defines search behavior.

Example:

```json
{
  "entity": "change-request",
  "columns": [
    "caseNumber",
    "title",
    "status"
  ]
}
```

Capabilities:

- Searchable Fields
- Search Filters
- Sortable Fields
- Result Columns
- Export Fields
- Saved Searches
- Default Sorting

Must align with the Search Requirements document.

---

## Detail View Definition

Defines read-only business views.

Capabilities:

- Summary Section
- Related Records
- Attachments
- Timeline
- Approval History
- Participant History
- Current Stage
- Current Status

The Detail View is the primary destination after a search result is opened.

---

## Lifecycle Definition

Defines business lifecycle behavior.

Examples:

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

Capabilities:

- Lifecycle States
- Stage Names
- Transition Rules
- Default State
- Completion States

Lifecycle definitions are metadata-driven.

Lifecycle behavior shall not be hardcoded.

---

## Workflow Mapping Definition

Connects business objects to workflow engines.

Example:

```json
{
  "entity": "change-request",
  "workflowAdapter": "cibseven",
  "workflowKey": "change-management"
}
```

Capabilities:

- Start Workflow
- Variable Mapping
- Activity Mapping
- Decision Mapping
- Status Synchronization
- Event Handling

---

## Workflow Extension Points

For advanced integrations metadata may reference optional Python handlers.

Examples:

- External Service Integration
- Complex Mapping Logic
- Data Enrichment
- Eligibility Validation
- Advanced Business Rules

Principle:

Metadata First + Optional Extension Points

---

## Dynamic Business Rules

Business rules may be executed through REST integrations.

Supported use cases:

- Validation
- Visibility Rules
- Eligibility Checks
- Business Calculations
- Read-Only Rules

Example:

```json
{
  "validation": {
    "type": "rest",
    "url": "/api/validation/request"
  }
}
```

---

## Dynamic Dropdowns

Dropdown values may be loaded from REST services.

Example:

```json
{
  "field": "department",
  "datasource": {
    "type": "rest",
    "endpoint": "/api/departments"
  }
}
```

---

## Dynamic Visibility

Visibility behavior may be driven by:

- Field Values
- Metadata Rules
- REST Integrations

Example:

```json
{
  "visibility": {
    "type": "rest",
    "endpoint": "/api/rules/show-budget-field"
  }
}
```

---

## Workflow Events

Metadata may define event handlers.

Supported Events:

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

Events may trigger:

- REST Integrations
- Notifications
- Validations
- Synchronization Logic

---

## Localization Model

All labels shall support localization.

Example:

```json
{
  "label": {
    "en": "Request Title",
    "ar": "عنوان الطلب"
  }
}
```

Supported Localization Targets:

- Labels
- Field Descriptions
- Help Text
- Validation Messages
- Status Names
- Search Labels
- Notification Templates

---

## Security Metadata

Metadata may define:

- Case Visibility Rules
- Access Assignment Rules
- Access Revocation Rules
- Role Mappings
- Activity Visibility Rules

Field-Level Security is not part of Phase 1 and shall not be modelled in metadata.

---

# Metadata Governance

Metadata definitions shall support lifecycle management.

Suggested Lifecycle:

```text
Draft
→ Review
→ Approved
→ Published
→ Retired
```

---

# Metadata Audit Requirements

All metadata changes shall be auditable.

Examples:

- Create Definition
- Modify Definition
- Publish Definition
- Retire Definition

Audit records shall include:

- User
- Timestamp
- Action
- Metadata Object
- Metadata Version

---

# Target Outcome

Using metadata alone, authorized Business Analysts and Administrators should be able to define:

1. New Case Type
2. New Business Object
3. Form Layout
4. Multi-Step Form
5. Search Screen
6. Detail Screen
7. Lifecycle Definition
8. Workflow Mapping
9. Localization
10. Validation Rules
11. REST Integrations
12. Access Rules

without creating new application code.

---

# Metadata Model Status

Status: APPROVED FOUNDATION v1.0

This document serves as the foundation for:

- Form Engine Architecture
- Logical Domain Model
- Database Design
- Workflow Adapter Contracts
- Metadata Designer
- Search Framework
