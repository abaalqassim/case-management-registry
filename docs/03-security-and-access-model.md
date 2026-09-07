# Security and Access Model

# Case Management Registry (CMR)

## Document Purpose

This document defines the security model, access control principles, visibility rules, and authorization requirements for the Case Management Registry.

The objective is to ensure that business information is accessible only to authorized users while maintaining complete auditability and flexibility.

---

# Security Principles

## Principle 1: Least Privilege

Users shall only have access to information required to perform their responsibilities.

Access shall not be granted by default.

---

## Principle 2: Explicit Access

Case visibility shall be controlled through explicit access assignments managed by the platform.

Access visibility shall not be derived solely from workflow history.

---

## Principle 3: Auditability

All security-related activities shall be auditable.

Examples:

- Access Granted
- Access Revoked
- Permission Changes
- Administrative Overrides

---

## Principle 4: Separation of Responsibilities

Business permissions and workflow permissions are separate concerns.

A user may:

- Participate in a workflow
- View a case
- Approve an activity

independently, based on authorization rules.

---

## Principle 5: Business Ownership

Case access decisions shall be governed by business rules rather than technical workflow implementation details.

---

# Security Scope

The platform shall distinguish between:

- Authentication
- Authorization
- Auditing

Authentication determines who the user is.

Authorization determines what the user may access.

Auditing records what the user did.

These concerns shall remain independent.

---

# Access Model

Case access is the primary authorization mechanism of the platform.

Users may gain access through:

- Case Creation
- Business Assignment
- Participation
- Organizational Role
- Administrative Assignment
- Audit Assignment

---

# User Roles

The platform distinguishes between platform roles and case roles.

---

# Platform Roles

Platform roles govern system capabilities.

Examples:

- Administrator
- Business Analyst
- Auditor
- Manager
- Standard User

Platform roles do not automatically grant access to specific cases.

---

# Case Roles

Case roles govern visibility and interactions with individual cases.

---

## Requester

The user who initiated the case.

Capabilities:

- View Case
- View History
- View Attachments
- Track Progress

Restrictions:

- Cannot automatically view unrelated cases.

---

## Participant

A user who contributed to case processing.

Examples:

- Reviewer
- Processor
- Investigator
- Specialist

Capabilities:

- View Authorized Cases
- View Related History
- View Related Attachments

Access may be revoked.

---

## Approver

A user responsible for approving activities.

Capabilities:

- View Assigned Cases
- View Supporting Information
- View Audit History

Access may remain after approval based on configuration.

---

## Manager

A user responsible for overseeing cases.

Capabilities:

- View Cases within Authorized Scope
- View Reports
- View Audit Information

Manager access should be configurable.

---

## Auditor

A user assigned auditing responsibilities.

Capabilities:

- Read-Only Access
- Full Historical Visibility
- Audit Review

Auditors shall not modify business data.

---

## Administrator

A platform administrator.

Capabilities:

- Manage Access
- Manage Configuration
- Perform Administrative Actions

Administrative actions must be audited.

---

# Case Visibility Rules

## Rule 1

Users shall only see cases for which they have active authorization.

---

## Rule 2

Search results shall be filtered according to the user's case visibility permissions.

Visibility rules apply before search results are returned.

---

## Rule 3

Case details shall not be accessible simply because a case identifier is known.

Visibility checks shall always be enforced.

---

## Rule 4

Access revocation shall immediately remove visibility for affected users.

Historical workflow participation shall not preserve access.

---

## Rule 5

Managers and Auditors may receive broader visibility through business-defined authorization rules.

---

# Access Assignment Sources

Access may originate from multiple sources.

---

## Direct Assignment

Access explicitly granted to a named user.

Examples:

- Requester
- Participant
- Auditor

---

## Organizational Assignment

Access derived from organizational structures.

Examples:

- Department
- Directorate
- Division

---

## Role-Based Assignment

Access derived from business roles.

Examples:

- Case Manager
- Department Manager
- Quality Auditor

---

## Workflow Assignment

Access granted because a workflow activity was assigned.

Workflow-generated access may be temporary or permanent based on business configuration.

---

# Access Lifecycle

## Grant Access

Access may be granted:

- Automatically
- Through Metadata Rules
- Through Workflow Integration
- Through Administrative Action

---

## Review Access

Authorized users may review who currently has access to a case.

Visibility of this information is configurable.

---

## Revoke Access

Access may be revoked:

- Automatically
- Through Workflow Events
- Through Organizational Changes
- Through Administrative Action

Revocation shall be auditable.

---

# Metadata Security

Metadata visibility and modification shall be separately controlled.

---

## Business Analysts

May manage:

- Case Types
- Forms
- Search Definitions
- Workflow Mappings
- Lifecycle Definitions

based on assigned permissions.

---

## Administrators

May manage:

- Security Configuration
- Platform Configuration
- Metadata Permissions

---

# Audit Requirements

The platform shall maintain a complete security audit trail.

Examples:

- Access Granted
- Access Revoked
- Metadata Modified
- Permission Changed
- Administrative Override

Each audit record shall contain:

- User
- Timestamp
- Action
- Target Object
- Reason (where applicable)

---

# Administrative Override

The platform may support controlled administrative override capabilities.

Examples:

- Emergency Access
- Production Support
- Audit Investigations

Requirements:

- Explicit Authorization
- Mandatory Audit Record
- Full Traceability

---

# Field-Level Security

Field-level security is not part of the initial platform scope.

Authorization shall be enforced at the case level.

Users who have access to a case may access all information contained within that case.

Rationale:

- Simplifies implementation.
- Simplifies metadata management.
- Simplifies search architecture.
- Simplifies form rendering.
- Simplifies auditing.

Future requirements such as GDPR compliance, privacy regulations, data classification, and sensitive-data protection may introduce:

- Field-Level Security
- Record Masking
- Attribute-Based Access Control (ABAC)
- Data Classification Policies

These capabilities shall be evaluated in a future phase.

---

# Future Security Enhancements

Potential future enhancements include:

- Attribute-Based Access Control (ABAC)
- Row-Level Security
- Data Classification
- Field-Level Security
- Record Masking
- Delegated Administration

These capabilities are outside the initial platform scope.

---

# Security Architecture Status

Status: Approved Security Baseline v1.0

Frozen Decisions:

- Explicit Case Access model
- Platform Roles and Case Roles are separate
- Access visibility independent from workflow history
- Search results filtered by visibility rules
- Access can be granted and revoked
- Administrative actions are audited
- Authentication, Authorization, and Auditing are separate concerns
- No Field-Level Security in Phase 1
- GDPR-related capabilities deferred to future phases
