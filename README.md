# Case Management Registry (CMR)

## Overview

Case Management Registry (CMR) is a metadata-driven case management platform that provides a centralized repository for managing, searching, auditing, and tracking business cases.

The platform serves as the authoritative System of Record for business case information while delegating workflow execution to external workflow engines through a Workflow Adapter Layer.

The initial workflow engine implementation is CIBSeven BPM.

---

## Vision

Build a metadata-driven platform that enables Business Analysts to configure case types, forms, search experiences, workflow mappings, and lifecycle behavior with minimal development effort.

The platform shall provide:

- Case Management
- Dynamic Forms
- Dynamic Search
- Audit Trails
- Access Control
- Attachment Management
- Workflow Integration
- Metadata-Driven Configuration

while remaining independent from any specific workflow engine implementation.

---

## Business Domains

### Internal Administrative Cases

Examples:

- Change Requests
- Development Requests
- Service Requests
- Administrative Requests

Primary Search:

- Employee
- Department
- Case Number

---

### Individual Cases

Examples:

- Complaints
- Appeals
- Service Requests
- Social Assistance Requests

Primary Search:

- CPR
- Personal Identifier
- Citizen Name
- Case Number

---

### Organization Cases

Examples:

- Commercial Organizations
- NGOs
- Institutional Requests

Primary Search:

- Organization Name
- Commercial Registration Number
- NGO Identifier
- Case Number

---

## Core Principles

### System of Record

The Case Management Registry is the authoritative source for:

- Case Data
- Case Metadata
- Attachments
- Audit History
- Access Control
- Search

---

### Workflow Independence

The platform shall remain independent from workflow engine implementations.

Workflow operations shall be executed through a Workflow Adapter Layer.

Current implementation:

- CIBSeven BPM Adapter

Future implementations may include:

- Camunda Adapter
- Flowable Adapter
- jBPM Adapter
- Custom Workflow Adapters

---

### Separation of Responsibilities

#### Case Management Registry Owns

- Business Data
- Case Lifecycle
- Case Search
- Attachments
- Audit History
- Access Control
- Metadata Configuration

#### Workflow Engine Owns

- Process Execution
- Activity Execution
- Routing
- Escalations
- Timers
- Workflow State Management

---

### Business Terminology

Users interact with:

- Cases
- Activities
- Participants
- Case History
- Case Status

Users do not interact directly with:

- Process Instances
- Process Variables
- Engine Runtime Tables
- Workflow Technical Details

---

### Metadata-Driven Design

Business functionality should be driven by metadata whenever possible.

Examples:

- Case Types
- Forms
- Sections
- Fields
- Search Definitions
- Workflow Mappings
- Validation Rules

Business Analysts should be able to introduce new case types without requiring major platform changes.

---

## Technology Stack

### Backend

- Python
- FastAPI
- SQLAlchemy
- Alembic

### Frontend

- Jinja2
- HTMX
- Bootstrap RTL

### Database

- PostgreSQL
- JSONB

### Forms

- JSON Forms
- Metadata-Driven Form Designer

### Workflow

- Workflow Adapter Layer
- CIBSeven Adapter (Initial Implementation)

---

## High-Level Capabilities

### Case Management

- Create Cases
- Update Cases
- View Cases
- Archive Cases
- Track Case Progress

---

### Search

- Global Search
- Advanced Search
- Cross-Case-Type Search
- Metadata-Driven Search Configuration

---

### Audit

- Activity Timeline
- Decision History
- Full Traceability
- Access History

---

### Attachments

- Upload Files
- Download Files
- Attachment Audit Tracking

---

### Access Control

Supported Roles:

- Requester
- Participant
- Approver
- Manager
- Auditor
- Administrator

Capabilities:

- Grant Access
- Revoke Access
- Audit Access Changes

---

### Workflow Adapter Framework

Capabilities:

- Start Workflow
- Track Workflow
- Synchronize Workflow State
- Retrieve Activities
- Complete Activities
- Support Multiple Workflow Engines

---

### Metadata Management

Capabilities:

- Manage Case Types
- Manage Form Definitions
- Manage Search Definitions
- Manage Workflow Mappings
- Manage Validation Rules

---

## Supported Channels

Cases may originate from:

- Case Management Registry
- BPM Forms
- E-Service Portals
- Service Counters
- Contact Centers
- Email
- Mobile Applications
- Future External Systems

---

## Case Lifecycle

```text
Draft
  ↓
Submitted
  ↓
In Progress
  ↓
Completed
  ↓
Archived
```

Alternative Paths:

```text
In Progress → On Hold

In Progress → Rejected
```

The case lifecycle is independent from workflow implementation details.

Workflow behavior remains the responsibility of BPM Analysts.

---

## High-Level Architecture

```text
+--------------------------------------------------+
|           Case Management Registry               |
|               Jinja2 + HTMX                      |
+--------------------------+-----------------------+
                           |
                           v
+--------------------------------------------------+
|                 FastAPI Platform                 |
+--------------------------+-----------------------+
                           |
          +----------------+----------------+
          |                                 |
          v                                 v

+------------------+          +--------------------------+
|    PostgreSQL    |          | Workflow Adapter Layer   |
+------------------+          +------------+-------------+
                                           |
                                           v

                                 +------------------+
                                 | CIBSeven Adapter |
                                 +--------+---------+
                                          |
                                          v

                                   +-------------+
                                   |  CIBSeven   |
                                   +-------------+
```

---

## Project Roadmap

### Phase 1 - Platform Foundation

- Authentication
- Authorization
- Case Registry
- Search
- Audit Trail
- Attachments
- Access Control
- Metadata Engine
- CIBSeven Workflow Adapter

---

### Phase 2 - Business Configuration

- Case Type Designer
- Form Designer
- Search Designer
- Workflow Mapping Designer
- Notification Templates

---

### Phase 3 - Digital Workplace

- Integrated Activity Inbox
- Activity Completion
- Dashboards
- Advanced Reporting
- Citizen 360 View
- Organization 360 View

---

## Documentation

Project documentation is maintained in the `/docs` directory.

Key documents include:

- Product Requirements Document
- Solution Architecture
- System Architecture
- Metadata Model
- Domain Model
- Case Lifecycle
- Integration Specification
- Security & Access Model
- Search Requirements
- Architectural Decision Records

---

## Current Status

```text
Architecture & Discovery Phase
```

Current Workflow Engine:

```text
CIBSeven BPM
```

Current Workflow Adapter:

```text
CIBSeven Adapter
```
