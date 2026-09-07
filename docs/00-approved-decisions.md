# Approved Architectural Decisions

Status: FROZEN
Version: 1.2

---

# AD-01 Portal Technology Stack

Approved Stack:

- FastAPI
- Jinja2
- HTMX
- Bootstrap 5
- PostgreSQL
- SQLAlchemy
- Alembic

Purpose:

- Lightweight architecture
- Server-side rendering
- Enterprise maintainability
- Rapid development

---

# AD-02 Internationalization First

The platform shall be designed as multilingual from inception.

Supported Languages:

- Arabic
- English

Requirements:

- Runtime language switching
- Runtime RTL/LTR switching
- Metadata-driven localization

---

# AD-03 Workflow Engine Independence

The platform shall not be coupled to a specific BPM vendor.

Reference Architecture:

Workflow Adapter Layer

Supported Targets:

- Camunda 7 Forked BPM Engines
- Future Workflow Engines

---

# AD-04 Business Data Ownership

Business data shall be owned by the platform.

The BPM engine shall not be the system of record.

Business data shall be persisted in the application database.

---

# AD-05 Portal Responsibilities

Responsibilities:

- Request Management
- Task Management
- Search
- Dashboards
- Reporting
- Attachments
- User Experience

---

# AD-06 Workflow Responsibilities

Responsibilities:

- Orchestration
- Routing
- Escalations
- Timers
- Workflow History
- Task Lifecycle

---

# AD-07 Low-Code Direction

The platform shall evolve toward a low-code architecture.

Goals:

- Minimize development effort
- Enable business configuration
- Accelerate delivery

---

# AD-08 Metadata-Driven Platform

Application behavior shall be generated through metadata.

Metadata shall govern:

- Business Objects
- Forms
- Multi-Step Forms
- Search Screens
- Detail Views
- Workflow Bindings
- Validation Rules
- Dynamic Rules
- Localization
- Security
- UI Behavior

---

## Permission-Based UI Behavior

Permission behavior is part of the metadata model.

Supported Controls:

### Visibility

- Visible
- Hidden

### Enablement

- Enabled
- Disabled

### Editability

- Editable
- Read Only

### Navigation Security

Controls:

- Menus
- Submenus
- Dashboards
- Reports
- Administration Screens

### Form Security

Controls:

- Sections
- Tabs
- Steps
- Fields
- Attachments
- Actions

### Detail View Security

Controls:

- Approve
- Reject
- Delegate
- Escalate
- Edit
- Delete

### Permission Sources

- Roles
- Policies
- Delegations
- Workflow Context
- Request State

Backend authorization remains mandatory.

---

# AD-09 Event-Driven Runtime

The platform shall support event-driven execution.

Supported Events:

- onLoad
- onChange
- onBlur
- onStepEnter
- onStepExit
- beforeSave
- afterSave
- beforeSubmit
- afterSubmit
- beforeTaskComplete
- afterTaskComplete

Events may invoke:

- Local Logic
- REST APIs
- Workflow Operations

---

# AD-10 Hybrid Storage Model

The platform shall adopt a hybrid persistence strategy.

Metadata:

- Relational Tables

Business Data:

- Relational Columns
- JSONB Payloads

Search:

- Projection Tables

Attachments:

- Object Storage

Benefits:

- Flexibility
- Performance
- Reporting
- Metadata extensibility

---

# Approved Outcome

Business Analysts should be able to configure:

- Business Objects
- Forms
- Multi-Step Forms
- Search Screens
- Detail Views
- Workflow Bindings
- Validation Rules
- Security Rules
- UI Behavior
- Localization

without requiring new application development.
