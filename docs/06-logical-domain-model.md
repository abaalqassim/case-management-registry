# Logical Domain Model (Frozen)

Status: APPROVED
Depends On:
- Metadata Model v2
- Security & Access Model
- Approved Architectural Decisions

## Purpose
Define the core business domains of the platform independent of implementation details, database technology, or BPM engine vendor.

---

# Domain Overview

## 1. Identity & Access Domain

### Concepts
- User
- Role
- Group
- Permission
- Delegation
- Organization Unit

### Responsibilities
- Authentication
- Authorization
- Row Level Security
- User Context

---

## 2. Business Object Domain

Represents all request types.

### Core Entity
BusinessObject

Examples:
- Change Request
- Access Request
- Purchase Request
- Leave Request
- Incident

### Relationships
BusinessObject
├── Fields
├── Attachments
├── Comments
├── Tasks
├── Workflow Instance
└── Audit Events

---

## 3. Metadata Domain

### Core Entities
- Entity Definition
- Field Definition
- Form Definition
- Search Definition
- Detail View Definition
- Workflow Binding
- Localization Resource

### Purpose
Generate application behavior without coding.

---

## 4. Form Runtime Domain

### Entities
- Form Instance
- Form Step
- Form Submission
- Draft
- Validation Result

### Capabilities
- Multi-step forms
- Draft save
- Resume later
- Dynamic rendering
- Dynamic validation

---

## 5. Workflow Domain

### Entities
- Workflow Definition
- Workflow Instance
- Task
- Task Decision
- Assignment
- Escalation

### Adapter Principle
The portal communicates through a Workflow Adapter.

Supported Targets:
- Camunda 7 Forked BPM Engines
- Future workflow engines

---

## 6. Rules Domain

### Entities
- Validation Rule
- Visibility Rule
- Calculation Rule
- Lookup Rule
- REST Rule

### Invocation Modes
- Client initiated
- Server initiated
- Workflow initiated

---

## 7. Search Domain

### Entities
- Search Definition
- Search Filter
- Saved Search
- Search Result

### Capabilities
- Cross-object search
- Advanced filtering
- Export
- Saved queries

---

## 8. Document & Attachment Domain

### Entities
- Attachment
- File Version
- File Classification
- Virus Scan Result

### Capabilities
- Upload
- Download
- Preview
- Audit Tracking

---

## 9. Audit Domain

### Entities
- Audit Event
- Activity Timeline
- Change Record
- Access Record

Tracked Events:
- Create
- Update
- Delete
- Approval
- Rejection
- Login
- Search
- Download

---

## 10. Localization Domain

### Entities
- Language
- Translation Resource
- Label
- Message

Supported Modes:
- Arabic (RTL)
- English (LTR)

---

# Canonical Business Object Aggregate

BusinessObject
├── Metadata
├── Data
├── Workflow
├── Tasks
├── Attachments
├── Audit Trail
├── Security Context
└── Localization

---

# Target Platform Outcome

Administrators should be able to define:

1. Business Object
2. Multi-Step Form
3. Search View
4. Detail View
5. Workflow Mapping
6. Security Rules
7. REST-Based Business Rules
8. Localization Resources

entirely through metadata.
