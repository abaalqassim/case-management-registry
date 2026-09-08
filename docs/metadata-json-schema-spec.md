# Metadata JSON Schema Specification

# Case Management Registry (CMR)

Status: APPROVED BASELINE v1.1

---

# Purpose

This document defines the canonical JSON metadata structures used by the Case Management Registry (CMR).

The JSON structures described in this document are the contract between:

- Metadata Designer
- Form Engine
- Search Engine
- Workflow Adapter Framework
- Validation Engine
- Localization Engine
- UI Renderer

The objective is to maintain a single metadata model throughout the platform.

---

# Design Principles

## Metadata Is the Source of Truth

Runtime behavior shall be generated from metadata.

Metadata drives:

- Forms
- Searches
- Detail Views
- Lifecycles
- Workflow Bindings
- Localization
- Validation
- UI Behavior

---

## JSON as Canonical Format

Metadata shall be represented as JSON.

Benefits:

- Human Readable
- API Friendly
- Versionable
- PostgreSQL JSONB Compatible
- Easy Validation

---

## Versioned Metadata

Every metadata definition shall contain version information.

Example:

```json
{
  "id": "complaint",
  "version": 2
}
```

---

## Reference by ID and Version

Metadata objects shall reference each other using a standard Metadata Reference Object.

References shall contain:

- id
- version

Versions shall never be embedded within identifiers.

---

# Metadata Reference Schema

All metadata references shall use the following structure:

```json
{
  "id": "complaint-create-form",
  "version": 3
}
```

---

## Reference Rules

References between metadata objects:

- Must contain both id and version.
- Must reference published metadata.
- Must not encode version information inside IDs.
- Must remain immutable once published.

---

## Valid Example

```json
{
  "form": {
    "id": "complaint-create-form",
    "version": 3
  }
}
```

---

## Invalid Example

```json
{
  "form": "complaint-create-form-v3"
}
```

---

# Common Metadata Structure

All metadata objects shall contain:

```json
{
  "id": "complaint",
  "code": "complaint",
  "version": 1,
  "status": "published",
  "createdAt": "2026-09-01T00:00:00Z",
  "updatedAt": "2026-09-03T00:00:00Z"
}
```

---

# Metadata Status Values

Supported values:

```text
draft
review
approved
published
retired
```

---

# Localization Object

All user-facing content shall use localized values.

Example:

```json
{
  "label": {
    "en": "Request Title",
    "ar": "عنوان الطلب"
  }
}
```

---

# Entity Definition Schema

Represents a metadata-defined business object.

```json
{
  "id": "complaint",

  "code": "complaint",

  "version": 1,

  "name": {
    "en": "Complaint",
    "ar": "شكوى"
  },

  "description": {
    "en": "Citizen Complaint",
    "ar": "شكوى مواطن"
  },

  "category": "individual"
}
```

---

# Case Type Schema

```json
{
  "id": "complaint",

  "version": 2,

  "entity": {
    "id": "complaint",
    "version": 1
  },

  "forms": {

    "create": {
      "id": "complaint-create-form",
      "version": 3
    },

    "edit": {
      "id": "complaint-edit-form",
      "version": 2
    },

    "view": {
      "id": "complaint-view-form",
      "version": 2
    },

    "closure": {
      "id": "complaint-closure-form",
      "version": 1
    }
  },

  "search": {
    "id": "complaint-search",
    "version": 2
  },

  "detailView": {
    "id": "complaint-detail-view",
    "version": 1
  },

  "lifecycle": {
    "id": "complaint-lifecycle",
    "version": 1
  },

  "workflow": {
    "id": "complaint-workflow",
    "version": 1
  }
}
```

---

# Field Definition Schema

```json
{
  "id": "requestTitle",

  "version": 1,

  "type": "string",

  "label": {
    "en": "Request Title",
    "ar": "عنوان الطلب"
  },

  "description": {
    "en": "Enter the title",
    "ar": "أدخل العنوان"
  },

  "required": true,

  "searchable": true,

  "defaultValue": null
}
```

---

# Supported Field Types

```text
string
text
integer
decimal
currency
boolean
date
datetime
select
multiselect
email
phone
url
user
department
attachment
richtext
```

---

# Field Behavior Schema

```json
{
  "field": "budgetAmount",

  "behavior": {

    "caseRoles": {
      "requester": "readonly",
      "manager": "editable"
    }
  }
}
```

---

# Supported Behaviors

```text
hidden
visible
readonly
editable
required
optional
disabled
```

---

# Form Definition Schema

```json
{
  "id": "complaint-create-form",

  "version": 3,

  "layout": "two-column",

  "sections": [
    {
      "id": "request-information",
      "version": 1
    }
  ]
}
```

---

# Form Layout Types

```text
single-column
two-column
three-column
tabbed
wizard
```

---

# Section Schema

```json
{
  "id": "complaint-details",

  "version": 1,

  "title": {
    "en": "Complaint Details",
    "ar": "بيانات الشكوى"
  },

  "fields": [
    {
      "id": "requestTitle",
      "version": 1
    },
    {
      "id": "description",
      "version": 1
    }
  ]
}
```

---

# Multi-Step Form Schema

```json
{
  "id": "complaint-wizard",

  "version": 1,

  "steps": [

    {
      "id": "request-information",
      "version": 1
    },

    {
      "id": "attachments",
      "version": 1
    }

  ]
}
```

---

# Step Definition Schema

```json
{
  "id": "request-information",

  "version": 1,

  "title": {
    "en": "Request Information",
    "ar": "بيانات الطلب"
  },

  "sections": [
    {
      "id": "applicant-details",
      "version": 1
    }
  ]
}
```

---

# Validation Schema

```json
{
  "type": "required",

  "message": {
    "en": "Value is required",
    "ar": "الحقل مطلوب"
  }
}
```

---

# REST Validation Schema

```json
{
  "type": "rest",

  "endpoint": "/api/validation/request",

  "method": "POST"
}
```

---

# Search Definition Schema

```json
{
  "id": "complaint-search",

  "version": 2,

  "fields": [

    {
      "id": "cpr",
      "version": 1
    },

    {
      "id": "citizenName",
      "version": 1
    }

  ],

  "filters": [
    "status",
    "submissionDate"
  ],

  "columns": [
    "caseNumber",
    "citizenName",
    "status"
  ]
}
```

---

# Detail View Schema

```json
{
  "id": "complaint-detail-view",

  "version": 1,

  "sections": [

    "summary",
    "timeline",
    "attachments",
    "participants"

  ]
}
```

---

# Lifecycle Definition Schema

```json
{
  "id": "complaint-lifecycle",

  "version": 1,

  "states": [

    {
      "code": "draft"
    },

    {
      "code": "submitted"
    },

    {
      "code": "investigation"
    },

    {
      "code": "resolved"
    },

    {
      "code": "closed"
    }

  ]
}
```

---

# Lifecycle State Schema

```json
{
  "code": "submitted",

  "label": {
    "en": "Submitted",
    "ar": "تم التقديم"
  },

  "terminal": false
}
```

---

# Workflow Mapping Schema

```json
{
  "id": "complaint-workflow",

  "version": 1,

  "adapter": "cibseven",

  "workflowKey": "complaint-process"
}
```

---

# Variable Mapping Schema

```json
{
  "field": {
    "id": "cpr",
    "version": 1
  },

  "variable": "citizenCPR"
}
```

---

# REST Rule Schema

```json
{
  "type": "rest",

  "endpoint": "/api/rules/eligibility",

  "method": "POST"
}
```

---

# Dynamic Dropdown Schema

```json
{
  "field": {
    "id": "department",
    "version": 1
  },

  "datasource": {

    "type": "rest",

    "endpoint": "/api/departments"

  }
}
```

---

# Dynamic Visibility Schema

```json
{
  "field": {
    "id": "budgetAmount",
    "version": 1
  },

  "visibility": {

    "type": "rest",

    "endpoint": "/api/rules/visibility"

  }
}
```

---

# Workflow Event Schema

```json
{
  "event": "before_submit",

  "actions": [

    {
      "type": "rest",

      "endpoint": "/api/eligibility/check"
    }

  ]
}
```

---

# Python Extension Schema

```json
{
  "handler": "ComplaintWorkflowHandler",

  "events": [
    "before_submit",
    "after_submit"
  ]
}
```

---

# Metadata Governance States

```text
draft
review
approved
published
retired
```

---

# Metadata Versioning Rules

- Existing cases continue using their original metadata version.
- New cases use the active metadata version.
- Historical versions remain available for audit purposes.
- Published versions cannot be modified.
- Published versions may be superseded but not deleted.

---

# Publication Rules

Metadata may only be referenced when:

```text
status = published
```

Draft metadata must not be used by runtime services.

---

# Validation Rules

Schema validation shall occur:

1. During Metadata Publication
2. During Runtime Rendering

Invalid metadata shall not be publishable.

---

# Security Rules

Metadata may define:

- Visibility Rules
- Access Rules
- UI Behavior Rules

The metadata model shall not implement Field-Level Security in Phase 1.

Security enforcement remains at the Case level.

---

# Compatibility Rules

Breaking changes require a new version.

Examples:

- Field Type Changes
- Required Field Changes
- Layout Changes
- Lifecycle Changes

Non-breaking changes may use metadata revisions within the same version lifecycle.

---

# Status

Status: APPROVED BASELINE v1.1

This specification is the canonical metadata contract for:

- Metadata Designer
- Form Engine
- Search Engine
- Workflow Adapter Framework
- Validation Engine
- Localization Engine
- Runtime UI Renderer
