# Logical Domain Model

# Case Management Registry (CMR)

Status: DRAFT v1.0

---

# Document Purpose

This document defines the logical business domain model for the Case Management Registry (CMR).

The logical domain model identifies the core business entities, relationships, responsibilities, and ownership boundaries that make up the platform.

This document intentionally avoids database implementation details such as:

- Tables
- Indexes
- Foreign Keys
- Storage Strategies

These concerns belong to the physical database design.

---

# Modeling Principles

## Business First

The logical domain model represents business concepts rather than technical implementation details.

Examples:

✅ Case

✅ Participant

✅ Attachment

✅ Lifecycle

❌ Table

❌ Process Instance

❌ Database Record

---

## Workflow Independence

Business entities shall remain independent from workflow engine implementations.

Workflow execution is external to the platform.

CMR owns business data.

Workflow engines own workflow execution.

---

## Metadata Driven

Business behavior is controlled through metadata.

The domain model includes metadata entities as first-class citizens.

---

## Explicit Access

Case visibility is controlled through platform-managed access assignments.

Access control is independent of workflow participation.

---

# Domain Overview

The platform consists of the following logical domains:

```text
Case Domain
Metadata Domain
Workflow Integration Domain
Security Domain
Audit Domain
```

---

# Case Domain

The Case Domain represents the core business capability of the platform.

---

## Case

Represents a managed business case.

Examples:

- Complaint
- Appeal
- Change Request
- Service Request
- NGO Request

Responsibilities:

- Business Data Ownership
- Lifecycle Tracking
- Status Tracking
- Case Identification
- Metadata Association

Relationships:

```text
Case
 ├─ Case Type
 ├─ Participants
 ├─ Attachments
 ├─ Access Assignments
 ├─ Audit Events
 ├─ Workflow Context
 └─ Case History
```

---

## Case Type

Represents a business classification of cases.

Examples:

- Complaint
- Appeal
- Change Request

Responsibilities:

- Form Association
- Lifecycle Association
- Workflow Association
- Search Association

---

## Case Participant

Represents a person or entity associated with a case.

Examples:

- Requester
- Reviewer
- Investigator
- Approver

Responsibilities:

- Business Association
- Participation Tracking

---

## Case Attachment

Represents a file associated with a case.

Examples:

- Documents
- Images
- Reports
- Supporting Evidence

Responsibilities:

- Attachment Management
- Version Awareness
- Audit Traceability

---

## Case History

Represents significant events that occurred during the lifecycle of a case.

Examples:

- Case Created
- Case Updated
- Status Changed
- Participant Added
- Attachment Added

Responsibilities:

- Timeline Generation
- Historical Traceability

---

## Case Channel

Represents the origin channel of a case.

Examples:

- Registry Portal
- BPM Forms
- E-Service Portal
- Contact Center
- Service Counter
- Email
- Mobile Application

Responsibilities:

- Source Attribution
- Reporting

---

# Security Domain

The Security Domain governs visibility and authorization.

---

## User

Represents an authenticated platform user.

Responsibilities:

- Authentication Identity
- Role Membership
- Access Evaluation

---

## Platform Role

Represents platform-level permissions.

Examples:

- Administrator
- Business Analyst
- Auditor
- Standard User

Responsibilities:

- Administrative Authorization

---

## Case Role

Represents authorization within a case context.

Examples:

- Requester
- Participant
- Approver
- Auditor

Responsibilities:

- Case-Level Permissions

---

## Case Access Assignment

Represents visibility rights granted to a user.

Responsibilities:

- Access Grant Tracking
- Access Revocation Tracking
- Visibility Evaluation

Relationships:

```text
Case
 └─ Case Access Assignment
       └─ User
```

---

# Audit Domain

The Audit Domain records platform activity.

---

## Audit Event

Represents an auditable action performed within the platform.

Examples:

- Case Created
- Metadata Published
- Access Granted
- Access Revoked

Responsibilities:

- Compliance Support
- Traceability
- Security Auditing

---

## Audit Actor

Represents the initiator of an audited action.

Examples:

- User
- Administrator
- System Process

---

# Metadata Domain

The Metadata Domain provides low-code configurability.

---

## Business Entity Definition

Represents a metadata-defined business object.

Examples:

- Complaint
- Change Request
- NGO Request

Responsibilities:

- Business Object Definition
- Metadata Ownership

---

## Case Type Definition

Represents metadata describing a Case Type.

Responsibilities:

- Workflow Association
- Lifecycle Association
- Search Association
- Form Association

---

## Field Definition

Defines business attributes.

Responsibilities:

- Field Metadata
- Validation Metadata
- Search Behavior

---

## Field Behavior Definition

Defines UI behavior rules.

Supported Behaviors:

- Visible
- Hidden
- Editable
- Read Only
- Disabled
- Required
- Optional

Responsibilities:

- Runtime UI Behavior

Field Behavior is not a security mechanism.

---

## Form Definition

Represents a metadata-driven form.

Responsibilities:

- Data Collection
- Data Presentation

---

## Form Variant

Represents a form purpose.

Examples:

- Create Form
- Edit Form
- View Form
- Activity Form
- Closure Form

---

## Form Section

Represents a logical grouping of fields.

Examples:

- Request Information
- Organization Details
- Attachments

---

## Form Step

Represents a wizard step in a multi-step form.

Responsibilities:

- Navigation
- Validation Scope

---

## Search Definition

Represents metadata controlling search experiences.

Responsibilities:

- Search Fields
- Filters
- Columns
- Sorting

---

## Detail View Definition

Represents metadata describing case detail screens.

Responsibilities:

- Summary Layout
- Timeline Layout
- Attachment Layout

---

## Lifecycle Definition

Represents metadata describing lifecycle behavior.

Responsibilities:

- States
- Transitions
- Completion Rules

---

## Lifecycle State

Represents a lifecycle state.

Examples:

- Draft
- Submitted
- Investigation
- Closed

---

## Workflow Mapping Definition

Represents metadata mapping business cases to workflow engines.

Responsibilities:

- Workflow Selection
- Variable Mapping
- Synchronization Rules

---

## Workflow Event Definition

Represents metadata-driven event behavior.

Examples:

- Before Save
- After Save
- Before Submit
- After Submit

Responsibilities:

- Trigger Definitions
- Integration Definitions

---

## Validation Definition

Represents metadata defining business validations.

Examples:

- Required
- Length
- Pattern
- REST Validation

---

## Localization Definition

Represents multilingual metadata values.

Responsibilities:

- Labels
- Descriptions
- Messages

---

# Workflow Integration Domain

The Workflow Integration Domain manages communication with workflow engines.

---

## Workflow Adapter

Represents an implementation capable of communicating with a workflow engine.

Examples:

- CIBSeven Adapter
- Camunda Adapter
- Flowable Adapter

Responsibilities:

- Workflow Translation
- API Integration

---

## Workflow Context

Represents the relationship between a business case and an external workflow instance.

Responsibilities:

- Workflow Correlation
- Status Synchronization

Relationships:

```text
Case
 └─ Workflow Context
       └─ Workflow Adapter
```

---

## Workflow Variable Mapping

Represents metadata controlling workflow variable exchanges.

Responsibilities:

- Business-to-Workflow Mapping
- Workflow-to-Business Mapping

---

# Domain Relationships

```text
Case
 ├─ Case Type
 ├─ Case Participants
 ├─ Case Attachments
 ├─ Case History
 ├─ Case Access Assignments
 ├─ Audit Events
 └─ Workflow Context

Case Type
 ├─ Form Definitions
 ├─ Search Definition
 ├─ Detail View Definition
 ├─ Lifecycle Definition
 └─ Workflow Mapping Definition

Form Definition
 ├─ Form Sections
 ├─ Form Steps
 ├─ Field Definitions
 └─ Validation Definitions

Workflow Mapping Definition
 ├─ Workflow Adapter
 └─ Workflow Variable Mappings
```

---

# Ownership Boundaries

## Owned by Case Domain

- Cases
- Participants
- Attachments
- History

---

## Owned by Metadata Domain

- Forms
- Fields
- Searches
- Lifecycles
- Workflow Mappings
- Localization

---

## Owned by Security Domain

- Users
- Roles
- Access Assignments

---

## Owned by Audit Domain

- Audit Events

---

## Owned by Workflow Domain

- Workflow Context
- Workflow Adapters

---

# Logical Domain Model Status

Status: DRAFT v1.0

This document serves as the foundation for:

- PostgreSQL Schema Design
- Entity Relationship Modeling
- Repository Design
- Service Layer Design
- API Design
