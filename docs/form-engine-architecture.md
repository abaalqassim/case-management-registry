# Form Engine Architecture

# Case Management Registry (CMR)

Status: APPROVED ARCHITECTURE BASELINE v1.0

---

# Document Purpose

This document defines the architecture of the Form Engine used by the Case Management Registry (CMR).

The Form Engine is responsible for transforming metadata definitions into runtime user experiences.

The Form Engine serves as the foundation for:

- Create Forms
- Edit Forms
- View Forms
- Activity Forms
- Closure Forms
- Multi-Step Forms

The engine is metadata-driven and generates user interfaces without requiring custom application development.

---

# Architectural Principles

## Metadata Driven

The Form Engine shall generate forms from metadata.

Application code shall not be required when creating new form definitions.

The Form Engine shall interpret metadata as the authoritative source of form behavior.

---

## Separation of Concerns

The Form Engine is responsible for:

- Rendering
- Validation
- UI Behavior
- Data Collection

The Workflow Adapter Layer is responsible for:

- Workflow Communication
- Activity Completion
- Workflow Events

The Security Model is responsible for:

- Authentication
- Authorization
- Case Visibility

---

## Configuration Before Customization

Metadata shall be the preferred mechanism for defining behavior.

Python extensions shall only be introduced when metadata cannot satisfy a requirement.

---

## Version Preservation

Forms shall be versioned.

Existing cases shall continue to use the form version active at creation time.

New cases shall use the currently published version.

---

# Form Engine Responsibilities

The Form Engine shall support:

- Dynamic Form Rendering
- Dynamic Validation
- Localization
- Layout Management
- Multi-Step Navigation
- Draft Management
- Activity Forms
- UI Behavior Rules
- Workflow Event Handling
- Attachment Management

---

# Technology Architecture

## Rendering Layer

The rendering layer shall use:

- Jinja2
- HTMX
- JSON Forms Metadata

---

## Form Metadata Provider

The Form Engine obtains metadata from:

- Metadata Repository
- Metadata API
- Metadata Cache

The metadata version must be resolved before rendering.

---

## Runtime Architecture

```text
+----------------------------+
|      Metadata Store        |
+-------------+--------------+
              |
              v
+----------------------------+
|    Form Metadata Loader    |
+-------------+--------------+
              |
              v
+----------------------------+
|      Form Engine           |
+-------------+--------------+
              |
     +--------+--------+
     |                 |
     v                 v
Validation      UI Behavior
     |                 |
     +--------+--------+
              |
              v
+----------------------------+
|      Jinja2 + HTMX         |
+----------------------------+
```

---

# Form Types

## Create Form

Used to create new cases.

Capabilities:

- Draft Support
- Validation
- Submission
- Attachment Upload

---

## Edit Form

Used to modify case data.

Capabilities:

- Validation
- Attachment Management
- Audit Tracking

---

## View Form

Read-only representation of case information.

Capabilities:

- Data Display
- Timeline Access
- Attachment Access

---

## Activity Form

Used during workflow execution.

Examples:

- Technical Review
- Manager Approval
- Investigation
- Verification

Activity forms may differ from the original create form.

---

## Closure Form

Used when finalizing a case.

Capabilities:

- Resolution Information
- Closure Comments
- Final Attachments

---

# Form Definition Structure

A form definition contains:

- Metadata Identity
- Version
- Layout
- Sections
- Fields
- Validation Rules
- Behavior Rules
- Events

Example:

```json
{
  "id": "complaint-create-form",
  "version": 3
}
```

---

# Metadata References

All metadata references shall use:

```json
{
  "id": "complaint-create-form",
  "version": 3
}
```

Metadata references shall not embed versions inside identifiers.

Example:

✅ Correct

```json
{
  "id": "complaint-create-form",
  "version": 3
}
```

❌ Incorrect

```json
{
  "form": "complaint-create-form-v3"
}
```

---

# Layout Architecture

Supported layouts include:

- Single Column
- Two Column
- Three Column
- Tabbed Layout
- Wizard Layout

Layout behavior shall be metadata-driven.

---

# Sections

Sections group related fields.

Examples:

- Applicant Information
- Request Information
- Organization Information
- Attachments

Sections support:

- Order
- Visibility Rules
- Localization
- Dynamic Behavior

---

# Field Rendering

The engine shall select a renderer based on field type.

Example:

```text
string      → Text Input
date        → Date Picker
boolean     → Checkbox
attachment  → File Upload
```

---

# Supported Field Types

- string
- text
- integer
- decimal
- currency
- boolean
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

---

# Form Lifecycle

## Load

Metadata is loaded.

Localization is applied.

Behavior rules are evaluated.

---

## Render

Controls are generated.

UI behavior is applied.

Visibility rules are evaluated.

---

## Validate

Client-side validation executes.

Server-side validation executes.

Server-side validation remains authoritative.

---

## Save

Data is persisted.

Drafts may be created.

Events may execute.

---

## Submit

Form data is finalized.

Workflow integration may occur.

Events may execute.

---

# Multi-Step Forms

The platform shall support wizard-style forms.

Capabilities:

- Previous
- Next
- Save Draft
- Resume Later
- Validation Per Step
- Review Step
- Conditional Steps

Example:

```text
Step 1: Applicant
Step 2: Details
Step 3: Attachments
Step 4: Review
```

---

# Draft Management

Users may save drafts.

Drafts shall preserve:

- Form Data
- Current Step
- Metadata Version

Drafts shall be recoverable.

---

# Validation Architecture

## Static Validation

Examples:

- Required
- Length
- Range
- Pattern

---

## Cross-Field Validation

Examples:

```text
Start Date < End Date
```

---

## Dynamic Validation

Validation may execute through REST integrations.

Example:

```json
{
  "type": "rest",
  "endpoint": "/api/validation/request"
}
```

---

# Permission-Based UI Behavior

The Form Engine shall support metadata-driven UI behavior.

UI behavior controls how controls are presented and interacted with.

UI behavior does not provide security.

Security remains enforced at the Case level.

---

## Supported Behavior Modes

- Visible
- Hidden
- Editable
- Read Only
- Required
- Optional
- Disabled

---

## Behavior Sources

Behavior rules may be evaluated from:

- Platform Roles
- Case Roles
- Workflow Activities
- Lifecycle Stages
- Metadata Rules
- REST Services
- Python Extensions

---

## Role-Based Example

```json
{
  "field": "budgetAmount",
  "behavior": {
    "caseRoles": {
      "manager": "editable",
      "requester": "readonly"
    }
  }
}
```

---

## Activity-Based Example

```json
{
  "field": "approvalRemarks",
  "behavior": {
    "activities": {
      "technical-review": "readonly",
      "manager-approval": "editable"
    }
  }
}
```

---

## Lifecycle Example

```json
{
  "field": "requestTitle",
  "behavior": {
    "stages": {
      "draft": "editable",
      "submitted": "readonly",
      "completed": "readonly"
    }
  }
}
```

---

## Dynamic Behavior Example

```json
{
  "field": "budgetAmount",
  "behavior": {
    "type": "rest",
    "endpoint": "/api/rules/field-behavior"
  }
}
```

---

## Rule Evaluation Order

The Form Engine shall evaluate behavior rules in the following order:

1. Static Metadata Rules
2. Lifecycle Rules
3. Activity Rules
4. Role Rules
5. REST Rules
6. Python Extensions

The most restrictive rule shall take precedence.

---

# Dynamic Dropdowns

Dropdown values may originate from:

- Static Metadata
- Reference Data
- REST Services

Example:

```json
{
  "datasource": {
    "type": "rest",
    "endpoint": "/api/departments"
  }
}
```

---

# Dynamic Visibility

Visibility may be controlled by:

- Metadata Rules
- Field Values
- REST Rules
- Python Extensions

---

# File Upload Architecture

The Form Engine shall support:

- Single File Upload
- Multiple File Upload
- Download
- Preview
- Validation
- Auditing

Supported validations:

- File Size
- File Type
- Virus Scanning Integration

---

# Workflow Integration

The Form Engine shall integrate through Workflow Adapters.

The Form Engine shall never directly invoke workflow engine APIs.

Supported functions:

- Start Workflow
- Complete Activity
- Update Variables
- Synchronize Status

---

# Workflow Events

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

# Event Execution

Events may execute:

- REST Integrations
- Notifications
- Validation Logic
- Synchronization Logic
- Python Extensions

---

# Localization Architecture

The Form Engine shall support multilingual rendering.

Initial languages:

- Arabic
- English

The Form Engine shall automatically adapt to:

- RTL Languages
- LTR Languages

Metadata labels shall be resolved using the active language.

---

# Metadata Version Resolution

Before rendering a form, the engine shall resolve:

- Form Version
- Field Versions
- Validation Rules
- Lifecycle Version

The version used shall be recorded for auditing purposes.

---

# Auditing Requirements

The Form Engine shall audit:

- Form Open
- Save Draft
- Submit
- Activity Completion
- Validation Failures
- Attachment Uploads

Audit records shall include:

- User
- Timestamp
- Case
- Form Version
- Action

---

# Error Handling

The Form Engine shall provide graceful handling for:

- Missing Metadata
- Invalid Metadata
- Validation Errors
- Integration Failures
- Workflow Failures

Metadata errors shall not expose internal implementation details.

---

# Extensibility

The Form Engine shall support extension through:

- Metadata
- REST Integrations
- Python Extension Points
- Additional Control Types
- Additional Layout Types

---

# Architectural Constraints

- Form behavior must be metadata-driven.
- Form rendering must remain workflow-engine independent.
- Security remains Case-based.
- UI behavior is not security.
- Metadata versions are immutable after publication.
- Workflow interactions must occur through adapters.

---

# Form Engine Status

Status: APPROVED ARCHITECTURE BASELINE v1.0

This document serves as the foundation for:

- Metadata Designer
- Runtime Renderer
- Workflow Activity Screens
- Dynamic Validation Framework
- Logical Domain Model
- PostgreSQL Schema Design
