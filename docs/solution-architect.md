# Solution Architecture

# Case Management Registry (CMR)

## Document Purpose

This document defines the high-level solution architecture of the Case Management Registry (CMR) platform.

It establishes the architectural vision, system boundaries, core components, integration model, and guiding principles that govern future implementation decisions.

This document intentionally avoids implementation details such as database schemas, API specifications, and deployment topologies.

---

# Architectural Vision

Case Management Registry (CMR) is a metadata-driven platform that provides centralized management of business cases while integrating with workflow engines through a pluggable Workflow Adapter Framework.

The platform serves as the authoritative System of Record for business information and exposes a unified experience for creating, searching, auditing, and managing cases.

Workflow execution remains the responsibility of external workflow engines.

---

# Architectural Drivers

The architecture is primarily driven by the following requirements:

- Centralized Case Repository
- Metadata-Driven Configuration
- Advanced Search
- Auditing and Traceability
- Pluggable Workflow Integration
- Business Analyst Enablement
- Long-Term Maintainability
- Multilingual Support
- Future Extensibility

---

# Architectural Principles

## Principle 1: System of Record

CMR is the authoritative source for all business case information.

The platform owns:

- Case Data
- Case Metadata
- Case History
- Attachments
- Access Permissions
- Search Definitions

Business information shall not depend on workflow engine databases.

---

## Principle 2: Workflow Independence

CMR shall remain independent from workflow engine implementations.

All workflow interactions shall occur through the Workflow Adapter Layer.

This enables workflow engines to be replaced or extended without impacting core business functionality.

---

## Principle 3: Business Terminology

Business users interact with business concepts rather than workflow concepts.

Examples:

- Case
- Activity
- Participant
- Stage
- History

Technical workflow terminology shall remain hidden from business users whenever possible.

---

## Principle 4: Metadata First

Business functionality should be configurable through metadata before custom development is considered.

Examples include:

- Case Types
- Forms
- Fields
- Search Definitions
- Validation Rules
- Workflow Mappings
- Lifecycle Definitions

---

## Principle 5: Separation of Concerns

Each architectural component shall have a clearly defined responsibility.

Business logic, workflow orchestration, metadata management, and user presentation should remain independent.

---

## Principle 6: Security by Design

Visibility of business data shall be controlled by platform-defined access rules.

Access rights shall not be derived dynamically from workflow history.

---

## Principle 7: Channel Independence

Cases may originate from multiple channels.

The platform shall provide a unified repository regardless of case origin.

Examples:

- Case Registry
- BPM Forms
- E-Service Portals
- Contact Centers
- Service Counters
- Email
- Mobile Applications

---

# Platform Capabilities

The platform is composed of three primary capability domains.

---

# Case Management Domain

Responsible for managing business cases.

Capabilities include:

- Case Creation
- Case Maintenance
- Case Status Tracking
- Case History
- Attachments
- Participant Management
- Access Management

This domain represents the core business capability of the platform.

---

# Metadata Domain

Responsible for business configuration.

Capabilities include:

- Case Type Management
- Form Definitions
- Search Definitions
- Validation Rules
- Workflow Mappings
- Lifecycle Configuration

The Metadata Domain enables low-code extensibility and Business Analyst empowerment.

---

# Workflow Integration Domain

Responsible for communication with workflow engines.

Capabilities include:

- Workflow Initiation
- Workflow Tracking
- Activity Retrieval
- Activity Completion
- Status Synchronization

This domain does not execute workflows itself.

Workflow execution remains delegated to an external workflow engine.

---

# High-Level Architecture

```text
+------------------------------------------------------+
|                User Experience Layer                 |
|                  Jinja2 + HTMX                       |
+----------------------------+-------------------------+
                             |
                             v
+------------------------------------------------------+
|                Case Management Platform              |
|                        FastAPI                       |
+------------------------------------------------------+
|                                                      |
|  Case Domain     Metadata Domain     Workflow Domain |
|                                                      |
+----------------------------+-------------------------+
                             |
         +------------------+------------------+
         |                                     |
         v                                     v

+----------------------+      +---------------------------+
|      PostgreSQL      |      |  Workflow Adapter Layer  |
+----------------------+      +-------------+------------+
                                            |
                                            v

                                 +------------------------+
                                 |    CIBSeven Adapter    |
                                 +------------+-----------+
                                              |
                                              v

                                       +-------------+
                                       |  CIBSeven   |
                                       +-------------+
```

---

# Responsibility Ownership

## Case Management Registry Owns

- Business Data
- Attachments
- Access Control
- Search
- Audit Trail
- Metadata Configuration
- Case Lifecycle

---

## Workflow Engine Owns

- Workflow Execution
- Routing
- Activity Assignment
- Escalations
- Timers
- Process State

---

# Metadata Driven Architecture

The platform shall support the definition of business behavior through metadata.

Metadata may define:

- Case Types
- Forms
- Fields
- Localization
- Validation Rules
- Search Experiences
- Workflow Mappings

Changes to metadata should require minimal or no application code changes.

---

# Workflow Adapter Architecture

CMR shall communicate exclusively through workflow adapter contracts.

The platform shall not directly depend on workflow engine APIs.

Initial implementation:

- CIBSeven Adapter

Future adapters may include:

- Camunda Adapter
- Flowable Adapter
- jBPM Adapter
- Custom Workflow Adapters

---

# Localization Architecture

The platform shall support multilingual operation.

The user interface shall dynamically adapt to:

- RTL languages
- LTR languages

based on the active language.

Metadata shall support multilingual values.

Examples:

- Labels
- Descriptions
- Statuses
- Validation Messages
- Notifications

---

# Extensibility Strategy

The platform shall support extension through:

- Metadata Configuration
- Additional Workflow Adapters
- New Case Types
- Additional Channels
- Future Integration Components

The architecture should favor extension over modification whenever possible.

---

# Architectural Constraints

The following constraints shall be enforced:

- Business data must not be stored exclusively in workflow engines.
- Workflow adapters must remain replaceable.
- Metadata is the preferred extension mechanism.
- Business terminology must remain workflow-agnostic.
- Case visibility must be managed by the platform.

---

# Future Architectural Evolution

## Phase 1

Foundation Platform

- Case Registry
- Metadata Engine
- Search
- Audit
- Attachments
- CIBSeven Adapter

---

## Phase 2

Business Configuration Platform

- Form Designer
- Search Designer
- Workflow Mapping Designer
- Notification Templates

---

## Phase 3

Digital Workplace

- Unified Activity Inbox
- Activity Execution
- Dashboards
- Reporting
- Advanced Analytics
- 360-Degree Views

---

# Architecture Status

Status: Approved Architecture Baseline v1.0
