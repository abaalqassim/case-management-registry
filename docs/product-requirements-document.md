# Product Requirements Document (PRD)

# Case Management Registry (CMR)

Status: APPROVED BASELINE v1.1

## Document Purpose

This document defines the business requirements, objectives, scope, and success criteria for the Case Management Registry (CMR).

It serves as the primary reference for Product Owners, Business Analysts, Architects, Developers, and Stakeholders.

---

# Product Overview

Case Management Registry (CMR) is a metadata-driven case management platform that provides a centralized repository for managing, searching, auditing, and tracking business cases.

The platform serves as the authoritative System of Record for all business cases while integrating with workflow engines through a Workflow Adapter Layer.

The initial workflow implementation will use the CIBSeven Adapter.

---

# Problem Statement

Business requests currently exist across multiple systems and processes.

This creates challenges including:

- Information fragmentation
- Difficult search experiences
- Lack of centralized auditing
- Inconsistent access control
- Duplicate business data
- Limited cross-process visibility

A unified platform is required to centralize business case information regardless of source channel or workflow implementation.

---

# Product Vision

Build a metadata-driven platform that enables organizations to manage business cases through configurable forms, workflows, search experiences, and lifecycle definitions.

The platform should minimize custom development while maximizing configuration capabilities available to Business Analysts and System Administrators.

---

# Product Goals

## Goal 1

Establish a centralized repository for all business cases.

---

## Goal 2

Provide a unified search experience across all case types.

---

## Goal 3

Provide complete auditing and traceability.

---

## Goal 4

Provide centralized access management.

---

## Goal 5

Provide workflow integration through a pluggable Workflow Adapter Framework.

---

## Goal 6

Enable metadata-driven configuration and extension.

---

# Business Domains

## Internal Administrative Cases

Examples:

- Change Requests
- Development Requests
- Service Requests
- Administrative Requests

Primary Search Criteria:

- Employee
- Department
- Case Number

---

## Individual Cases

Examples:

- Complaints
- Appeals
- Service Requests
- Assistance Requests

Primary Search Criteria:

- CPR
- Personal Identifier
- Citizen Name
- Case Number

---

## Organization Cases

### Commercial Organizations

Primary Search Criteria:

- Commercial Registration Number
- Organization Name
- Case Number

### NGOs

Primary Search Criteria:

- NGO Identifier
- Organization Name
- Case Number

### Institutional Requests

Primary Search Criteria:

- Organization Name
- Organization Identifier
- Case Number

---

# Stakeholders

## Requesters

Submit and track cases.

---

## Participants

Contribute to case processing activities.

---

## Approvers

Review and approve activities.

---

## Managers

Monitor and oversee case execution.

---

## Auditors

Review complete case history and activity.

---

## Administrators

Configure and manage the platform.

---

## Business Analysts

Configure metadata, forms, workflows, search experiences, and lifecycle behavior.

---

# Functional Requirements

# Case Management

The platform shall allow users to:

- Create Cases
- View Cases
- Update Cases
- Archive Cases
- View Case Status
- View Current Stage
- Track Progress

---

# Search

The platform shall support:

- Global Search
- Advanced Search
- Cross-Case-Type Search
- Search by Metadata
- Search by Case Number
- Search by Date Range
- Search by Status

---

# Access Management

The platform shall allow authorized users to:

- Grant Case Access
- Revoke Case Access
- Review Access History

The access model shall be managed independently from workflow engine history.

---

# Audit Management

The platform shall record:

- User
- Activity
- Timestamp
- Comments
- Attachments
- Status Changes
- Access Changes

The complete case timeline shall be available for review.

---

# Attachment Management

The platform shall allow:

- File Upload
- File Download
- Attachment Tracking
- Attachment Auditing

---

# Workflow Integration

The platform shall support:

- Workflow Initiation
- Workflow Tracking
- Workflow Synchronization
- Multiple Workflow Adapter Implementations

Current Workflow Adapter:

- CIBSeven Adapter

---

# Metadata Management

The platform shall support configuration of:

- Case Types
- Form Definitions
- Field Definitions
- Search Definitions
- Workflow Mappings
- Validation Rules
- Lifecycle Definitions
- UI Behavior Rules

through metadata.

Metadata management shall be available to authorized Administrators and Business Analysts.

---

# Supported Channels

Cases may originate from:

- Case Management Registry
- BPM Forms
- E-Service Portals
- Service Counters
- Contact Centers
- Email
- Mobile Applications

Additional channels shall be supported in the future without major architectural changes.

---

# Non-Functional Requirements

## Performance

The platform shall support at least:

- 100,000 cases annually

without architectural changes.

---

## Scalability

The platform shall support horizontal and vertical scaling.

---

## Security

The platform shall support:

- Authentication
- Authorization
- Auditability
- Access Revocation

---

## Localization and Internationalization

The platform shall support internationalization (i18n) and localization (l10n).

Initial supported languages:

- Arabic
- English

The user interface shall dynamically adapt to the writing direction of the active language:

- RTL layouts for RTL languages
- LTR layouts for LTR languages

All user-facing content shall support localization, including:

- Form Labels
- Field Descriptions
- Validation Messages
- Status Labels
- Search Definitions
- Activity Names
- Notification Templates
- Metadata Configuration

Metadata shall support multilingual values.

---

## Extensibility

The platform shall support:

- New Case Types
- New Workflow Adapters
- New Search Definitions
- New Form Definitions

through configuration whenever possible.

---

# Out of Scope (Phase 1)

The following items are excluded from the initial phase:

- Built-in Workflow Engine
- Custom BPM Designer
- Citizen 360 View
- Organization 360 View
- Advanced Analytics
- AI Capabilities
- Field-Level Security

These may be considered in future phases.

---

# Success Criteria

The platform shall be considered successful when:

- Business cases are centrally managed.
- Cross-case search is operational.
- Audit history is available.
- Access can be granted and revoked.
- Workflow integration functions through adapters.
- New case types can be introduced using metadata.

---

# Assumptions

- PostgreSQL is the primary database platform.
- FastAPI is the application framework.
- Jinja2 and HTMX provide the user experience layer.
- JSON Forms are used for dynamic form rendering.
- CIBSeven is the initial workflow engine implementation.
- Future workflow engines may be integrated through adapters.
- Workflow interactions occur through the Workflow Adapter Layer.

---

# Product Status

Status: APPROVED BASELINE v1.1
