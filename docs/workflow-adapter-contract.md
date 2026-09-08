# Workflow Adapter Contract

# Case Management Registry (CMR)

Status: DRAFT v1.0

---

# Document Purpose

This document defines the Workflow Adapter Contract used by the Case Management Registry (CMR).

The Workflow Adapter Contract provides a standard interface between the CMR platform and workflow engines.

The objective is to ensure that:

- Workflow engines remain replaceable.
- Business services remain workflow-independent.
- Metadata remains engine-independent.
- Workflow integrations remain standardized.

This document defines contracts and responsibilities, not implementation details.

---

# Architectural Principles

## Workflow Independence

The Case Management Registry shall not directly depend on workflow engine APIs.

All workflow interactions must occur through Workflow Adapters.

---

## Adapter Replaceability

Workflow adapters must be replaceable without requiring changes to:

- Case Management
- Search
- Access Management
- Metadata
- Audit Management

---

## Business Terminology

The Workflow Adapter Contract shall expose business-oriented terminology.

Business services should work with:

- Case
- Activity
- Participant
- Stage
- Status

instead of workflow-engine-specific concepts.

---

## Workflow Ownership

CMR owns:

- Business Data
- Attachments
- Metadata
- Search
- Access Control
- Audit History

Workflow Engines own:

- Workflow Execution
- Routing
- User Assignments
- Timers
- Escalations
- Process State

---

# Adapter Responsibilities

The Workflow Adapter is responsible for:

- Starting Workflows
- Retrieving Workflow State
- Retrieving Activities
- Completing Activities
- Cancelling Workflows
- Suspending Workflows
- Resuming Workflows
- Synchronizing Workflow Status
- Mapping Engine Concepts to Platform Concepts

---

# Supported Workflow Engines

## Initial Implementation

- CIBSeven Adapter

---

## Future Implementations

- Camunda Adapter
- Flowable Adapter
- jBPM Adapter
- Custom Workflow Adapters

---

# Common Types

## Workflow Reference

```json
{
  "workflowId": "wf-123",
  "workflowKey": "complaint-process"
}
```

---

## Activity Reference

```json
{
  "activityId": "act-001",
  "activityKey": "technical-review"
}
```

---

## User Reference

```json
{
  "userId": "ealshaker",
  "displayName": "Ebrahim Alshaker"
}
```

---

# Workflow Adapter Interface

Every workflow adapter shall implement the following capabilities.

---

# Start Workflow

## Purpose

Starts a workflow instance.

---

## Request

```json
{
  "workflowKey": "complaint-process",

  "businessKey": "CASE-1001",

  "variables": {
    "cpr": "123456789"
  }
}
```

---

## Response

```json
{
  "workflowInstanceId": "wf-12345",

  "status": "started"
}
```

---

# Get Workflow

## Purpose

Retrieves workflow information.

---

## Request

```json
{
  "workflowInstanceId": "wf-12345"
}
```

---

## Response

```json
{
  "workflowInstanceId": "wf-12345",

  "workflowKey": "complaint-process",

  "status": "active"
}
```

---

# Get Workflow Status

## Purpose

Retrieves the current workflow state.

---

## Response

```json
{
  "workflowInstanceId": "wf-12345",

  "status": "active"
}
```

---

# Get Active Activities

## Purpose

Returns currently active activities.

---

## Response

```json
[
  {
    "activityId": "task-01",

    "activityKey": "technical-review",

    "name": "Technical Review"
  }
]
```

---

# Claim Activity

## Purpose

Assigns an activity to a user.

---

## Request

```json
{
  "activityId": "task-01",

  "userId": "ealshaker"
}
```

---

## Response

```json
{
  "success": true
}
```

---

# Release Activity

## Purpose

Releases an assigned activity.

---

## Request

```json
{
  "activityId": "task-01"
}
```

---

# Complete Activity

## Purpose

Completes an active activity.

---

## Request

```json
{
  "activityId": "task-01",

  "variables": {
    "approved": true
  }
}
```

---

## Response

```json
{
  "success": true
}
```

---

# Get Activity Details

## Purpose

Returns detailed information about an activity.

---

## Response

```json
{
  "activityId": "task-01",

  "activityKey": "technical-review",

  "status": "active",

  "assignee": "ealshaker"
}
```

---

# Get Workflow History

## Purpose

Retrieves completed workflow activities.

---

## Response

```json
[
  {
    "activityKey": "submission",

    "completedAt": "2026-09-01T10:00:00Z"
  }
]
```

---

# Cancel Workflow

## Purpose

Terminates a workflow.

---

## Request

```json
{
  "workflowInstanceId": "wf-12345",

  "reason": "Cancelled by business user"
}
```

---

## Response

```json
{
  "success": true
}
```

---

# Suspend Workflow

## Purpose

Suspends workflow execution.

---

## Response

```json
{
  "success": true
}
```

---

# Resume Workflow

## Purpose

Resumes workflow execution.

---

## Response

```json
{
  "success": true
}
```

---

# Variable Management

## Get Variables

```json
{
  "workflowInstanceId": "wf-12345"
}
```

---

## Set Variables

```json
{
  "workflowInstanceId": "wf-12345",

  "variables": {
    "priority": "high"
  }
}
```

---

# Business Key Requirements

Every workflow instance must be linked to a Case.

The Case Number shall be used as the workflow business key.

Example:

```text
CASE-1001
```

Benefits:

- Easier correlation
- Simpler troubleshooting
- Consistent audit trail

---

# Workflow Status Mapping

Workflow adapters shall normalize engine-specific states.

## Standard Workflow States

```text
draft
active
suspended
completed
cancelled
failed
```

Workflow engines may use different internal states.

Adapters are responsible for mapping them to standard platform states.

---

# Activity Status Mapping

## Standard Activity States

```text
created
ready
claimed
active
completed
cancelled
failed
```

Adapters shall normalize activity states.

---

# Variable Mapping

Workflow variables are defined through metadata.

Example:

```json
{
  "field": "cpr",
  "variable": "citizenCPR"
}
```

The adapter executes the mapping.

---

# Workflow Events

Adapters shall support the following events.

```text
before_start_workflow
after_start_workflow

before_complete_activity
after_complete_activity

before_cancel_workflow
after_cancel_workflow
```

Events may trigger:

- REST Integrations
- Notifications
- Synchronization Services
- Python Extension Points

---

# Error Handling

Adapters shall return standardized errors.

## Example

```json
{
  "code": "WORKFLOW_NOT_FOUND",

  "message": "Workflow instance not found"
}
```

---

## Standard Error Codes

```text
WORKFLOW_NOT_FOUND
ACTIVITY_NOT_FOUND
INVALID_VARIABLE
INVALID_STATE
UNAUTHORIZED
ENGINE_UNAVAILABLE
ADAPTER_ERROR
```

---

# Security Requirements

Workflow adapters shall not perform business authorization.

Authorization remains the responsibility of CMR.

Workflow adapters may validate:

- Authentication
- Engine Permissions
- Technical Access Rights

Business visibility must never be derived from workflow history.

---

# Audit Requirements

The following operations shall be audited:

- Workflow Started
- Workflow Cancelled
- Workflow Suspended
- Workflow Resumed
- Activity Claimed
- Activity Released
- Activity Completed

Audit ownership remains with CMR.

---

# Extension Points

Workflow adapters shall support optional extension hooks.

Examples:

- Before Start Workflow
- After Start Workflow
- Before Complete Activity
- After Complete Activity

Extension implementations may use:

- REST Services
- Python Handlers

Adapters remain responsible for workflow execution.

---

# Adapter Certification Requirements

A workflow adapter shall be considered compliant when it supports:

- Workflow Start
- Workflow Status Retrieval
- Activity Retrieval
- Activity Completion
- Workflow History
- Variable Mapping
- Status Mapping
- Error Mapping
- Audit Events

---

# Versioning

The Workflow Adapter Contract shall be versioned independently from workflow engine implementations.

Contract Version:

```text
1.0
```

Adapters may support multiple contract versions.

---

# Contract Status

Status: DRAFT v1.0

This document defines the canonical workflow integration contract for the Case Management Registry platform.
