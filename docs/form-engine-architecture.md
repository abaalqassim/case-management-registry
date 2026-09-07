# Form Engine Architecture (Frozen)

**Status:** APPROVED

**Depends On:**
- Metadata Model v2
- Logical Domain Model
- Physical Data Model
- AD-08 Metadata-Driven Platform
- AD-09 Event-Driven Runtime
- AD-10 Hybrid Storage Model

---

# 1. Purpose

The Form Engine is responsible for rendering, validating, persisting, and executing metadata-driven forms without requiring new application code.

The Form Engine shall support:

- Single-step forms
- Multi-step forms
- Task forms
- Request forms
- Read-only detail views
- Dynamic validations
- Dynamic business rules
- REST integrations
- Internationalization
- RTL/LTR rendering

---

# 2. Architectural Principles

## 2.1 Metadata Driven

No business form shall require a dedicated HTML template.

Forms are generated dynamically from metadata definitions.

## 2.2 Event Driven

Forms react to lifecycle events and trigger behaviors through configured metadata.

## 2.3 Workflow Independent

The Form Engine shall not contain BPM vendor-specific logic.

Integration must occur only through the Workflow Adapter Layer.

## 2.4 API First

Business rules and dynamic behavior shall be executed through REST APIs.

---

# 3. High-Level Architecture

```text
Browser
    |
HTMX + Bootstrap
    |
Form Runtime Controller
    |
Form Engine
    |
+-------------------+
| Metadata Service  |
| Rules Engine      |
| Validation Engine |
| Workflow Adapter  |
+-------------------+
    |
PostgreSQL
```

---

# 4. Core Components

- Form Definition Service
- Runtime Renderer
- Layout Engine
- Multi-Step Engine
- Validation Engine
- Rules Engine
- Draft Management
- Workflow Adapter

---

# 5. Multi-Step Form Engine

Capabilities:

- Previous Step
- Next Step
- Save Draft
- Resume Later
- Review Page
- Conditional Navigation
- Step Completion Status

---

# 6. Event Model

Supported Events:

- onLoad
- onFieldChange
- onFieldBlur
- onStepEnter
- onStepExit
- beforeSave
- afterSave
- beforeSubmit
- afterSubmit
- beforeTaskComplete
- afterTaskComplete

---

# 7. Validation Engine

Supported:

- Required
- Length
- Pattern
- Range
- Cross Field
- REST Validation

---

# 8. Rules Engine

Supported:

- Visibility
- Enablement
- Read Only
- Calculations
- Hyperlinks
- Lookups

---

# 9. Dynamic Data Sources

- Static Lists
- Database Queries
- REST APIs
- Workflow Variables

---

# 10. Draft Management

Storage Table:

```text
bo_draft
```

Features:

- Auto Save
- Manual Save
- Resume Later

---

# 11. Security Model

- Role Security
- Field Security
- Row-Level Security
- JSONB Security

---

# 12. Localization Model

Supported:

- Arabic (RTL)
- English (LTR)

---

# 13. Attachment Framework

- Upload
- Download
- Preview
- Versioning
- Virus Scanning

Storage:

```text
Object Storage
```

---

# 14. Workflow Integration

Workflow Adapter Operations:

- Start Workflow
- Load Task
- Complete Task
- Retrieve Workflow State

Targets:

- Camunda 7 Forked BPM Engines
- Future Workflow Engines

---

# 15. Audit Integration

Audited Events:

- Open Form
- Save Draft
- Submit
- Approve
- Reject
- Upload File

---

# 16. Future Low-Code Capabilities

- Form Designer
- Rule Designer
- Workflow Binding Designer
- Reusable Components
- Form Publishing Workflow

---

# 17. Target Outcome

Business Analysts shall be able to create forms, workflows, validations, dynamic behaviors, and localization resources through metadata without application development.
