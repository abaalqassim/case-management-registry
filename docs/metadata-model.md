# Metadata Model

# Case Management Registry (CMR)

Status: APPROVED FOUNDATION v1.2

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
- Detail Pages
- Activity Screens
- Validation Rules
- Lifecycle Definitions
- Internationalization
- Workflow Bindings
- Workflow Events
- Access Rules
- Dashboard Widgets

---

# Design Principles

## Single Source of Truth

A business object definition must drive:

- Data Model
- User Interface
- Search Behavior
- Workflow Integration
- Lifecycle Behavior
- Reporting Experience

---

## Configuration Over Coding

New case types should be created using metadata whenever possible.

Custom development should only be required when metadata cannot reasonably satisfy business requirements.

---

## Engine Independence

Metadata shall not depend on any specific BPM vendor.

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

Extension points should only be used when metadata alone is insufficient.

---

## Versioned Configuration

Metadata definitions shall support versioning.

Changes to metadata shall not impact previously created cases.

Historical cases must continue to operate using the metadata version active at creation time.

---

## Hybrid Localization Strategy

The platform shall implement a hybrid localization model.

### Application Localization

Application translations shall be stored in translation files maintained under source control.

Examples:

- Save
- Cancel
- Search
- Login
- Dashboard
- Administration

### Metadata Localization

Metadata-driven translations shall be stored directly within metadata definitions.

Examples:

- Case Types
- Form Labels
- Search Labels
- Lifecycle States
- Status Names
- Notification Templates
- Activity Names

This enables:

- Business Analysts to manage business terminology
- Developers to manage platform terminology
- Independent deployment of platform and business configuration changes

---

# Core Metadata Components

## Entity Definition

Defines the business object.

Example:

```json
{
  "id": "change-request",
  "name": "Change Request",
  "category": "Administrative",
  "version": 1
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
- richtext
- attachment
- url

Additional field types may be introduced in future releases.

---

## Form Definition

Defines data entry screens.

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
- Read Only Rules
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
      "id": "request-information",
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

- Previous / Next Navigation
- Save Draft
- Resume Later
- Conditional Step Visibility
- Step Validation
- Step Completion Indicators
- Review Screen

---

## Form Variants

A Case Type may define multiple forms.

Supported Form Types:

- Create Form
- Edit Form
- View Form
- Activity Form
- Closure Form

Examples:

Complaint

```text
Create Form
View Form
Activity Form
Closure Form
```

Change Request

```text
Create Form
Edit Form
Technical Review Activity Form
CAB Activity Form
Closure Form
```

This enables different user experiences throughout the lifecycle of a case.

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
- Sortable Fields
- Export Fields
- Default Filters
- Saved Searches
- Result Columns
- Default Sorting

Search definitions must align with the Search Requirements document.

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

The Detail View is the primary destination after opening a search result.

---

## Lifecycle Definition

Lifecycle Definitions are metadata-driven.

Different Case Types may have different lifecycle models.

Example:

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

- States
- Stages
- Transitions
- Display Labels
- Default States
- Completion States

Lifecycle behavior shall not be hardcoded into the platform.

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
- Workflow Adapter Selection

---

## Workflow Adapter Architecture

Metadata shall remain independent from workflow engine implementations.

Workflow integration shall occur through the Workflow Adapter Layer.

Current Implementation:

- CIBSeven Adapter

Future Implementations:

- Camunda Adapter
- Flowable Adapter
- jBPM Adapter
- Custom Workflow Adapters

The metadata model should avoid workflow-engine-specific constructs wherever possible.

---

## Workflow Extension Points

For advanced requirements metadata may reference optional Python handlers.

Examples:

- External Service Integration
- Complex Mapping Logic
- Data Enrichment
- Eligibility Validation
- Advanced Business Rules

Example:

```json
{
  "handler": "ComplaintWorkflowHandler"
}
```

Principle:

```text
Metadata First
+
Optional Extension Points
```

---

## Dynamic Business Rules via REST APIs

Business rules shall be executable through REST integrations.

Supported use cases:

- Validation
- Dynamic Visibility
- Dynamic Read-Only Rules
- Dynamic Dropdown Values
- Dynamic Hyperlinks
- Business Calculations
- Eligibility Checks

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

## Dynamic Field Validation

Supported validation types:

- Required
- Pattern
- Length
- Range
- Cross-Field Validation
- Remote Validation

Example:

```json
{
  "field": "employeeNumber",
  "validation": {
    "type": "rest",
    "endpoint": "/api/employees/validate"
  }
}
```

---

## Dynamic Dropdowns

Dropdown values may be loaded from REST APIs.

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

## Conditional Visibility

Visibility rules may invoke backend services.

Example:

```json
{
  "field": "budgetAmount",
  "visibility": {
    "type": "rest",
    "endpoint": "/api/rules/visibility"
  }
}
```

---

## Dynamic Hyperlinks

Hyperlinks may be generated from REST responses.

Example:

```json
{
  "field": "requestLink",
  "type": "url",
  "source": "/api/request/link"
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

The platform shall support multilingual operation.

Initial supported languages:

- Arabic
- English

Additional languages shall be supported without schema changes.

The platform shall automatically adapt to:

- RTL Languages
- LTR Languages

based on the active language.

### Application Localization

Application translations shall be maintained in translation files.

### Metadata Localization

Metadata-driven values shall be stored directly in metadata.

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
- Descriptions
- Help Text
- Validation Messages
- Status Names
- Lifecycle States
- Search Labels
- Notification Templates
- Workflow Activity Names

---

## Localization Definition

A metadata object may define multilingual values directly.

Example:

```json
{
  "name": {
    "en": "Change Request",
    "ar": "طلب تغيير"
  }
}
```

The platform shall resolve the appropriate value using the active user language.

Optional fallback rules may be configured when translations are unavailable.

---

## Security Metadata

Security is metadata driven.

Capabilities:

- Case Visibility Rules
- Access Assignment Rules
- Access Revocation Rules
- Role Mappings
- Activity Visibility Rules

Field-Level Security is not part of Phase 1.

Security enforcement occurs at the Case level.

Future compliance requirements may introduce:

- Field-Level Security
- Data Masking
- ABAC
- GDPR Controls

---

# Metadata Governance

Metadata definitions shall support lifecycle management.

Lifecycle:

```text
Draft
→ Review
→ Approved
→ Published
→ Retired
```

Only Published metadata may be used by production cases.

---

# Metadata Versioning

Versioning applies to:

- Entity Definitions
- Field Definitions
- Form Definitions
- Detail View Definitions
- Search Definitions
- Workflow Mappings
- Lifecycle Definitions

Business Rules:

- Existing cases remain linked to their original metadata version.
- New cases use the currently active metadata version.
- Multiple versions may coexist.
- Historical metadata must remain available for audit purposes.

Example:

```text
Complaint Form v1
→ Case #100

Complaint Form v2
→ Case #101

Case #100 continues using v1.
Case #101 uses v2.
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

Status: APPROVED FOUNDATION v1.2

This document serves as the foundation for:

- Form Engine Architecture
- Logical Domain Model
- Database Design
- Workflow Adapter Contracts
- Metadata Designer
- Search Framework
