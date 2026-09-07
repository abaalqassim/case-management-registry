# Search Requirements

# Case Management Registry (CMR)

## Document Purpose

This document defines the search capabilities, search experiences, search rules, and search requirements of the Case Management Registry (CMR).

This document focuses on business requirements and intentionally avoids implementation details such as database structures, indexes, search engines, and query optimization strategies.

---

# Search Vision

The platform shall provide a unified search experience across all Case Types regardless of:

- Business Domain
- Submission Channel
- Workflow Implementation
- Originating System

Users shall be able to locate relevant cases quickly using business-oriented search criteria.

---

# Search Principles

## Principle 1: Search Cases, Not Workflows

Users search for Cases.

Users do not search for:

- Process Instances
- Workflow Variables
- BPM Runtime Data

Search shall use business terminology.

---

## Principle 2: Security First

Search results shall be filtered according to the user's visibility permissions.

Unauthorized cases shall never appear in search results.

---

## Principle 3: Metadata-Driven Search

Search behavior should be configurable through metadata whenever possible.

New Case Types should be able to define their own search criteria.

---

## Principle 4: Unified Search Experience

Users should not need to know which workflow or system created a case.

A single search interface should support searching across multiple Case Types.

---

## Principle 5: Progressive Search

Users should be able to start with a simple search and progressively narrow the results through filtering.

---

# Search Types

## Global Search

Allows searching across all visible Case Types.

Examples:

- Case Number
- Citizen Name
- CPR
- Organization Name

Results may include cases from multiple business domains.

---

## Advanced Search

Allows searching using multiple criteria.

Examples:

- Case Type
- Status
- Date Range
- Organization Name
- Employee
- Department

---

## Case-Type Specific Search

Allows specialized search experiences designed for particular Case Types.

Examples:

### Complaint Search

- Complaint Number
- CPR
- Citizen Name

### Change Request Search

- Employee
- Department
- Change Category

### NGO Search

- NGO Identifier
- Organization Name

---

# Standard Search Criteria

All Case Types shall support the following criteria where applicable:

- Case Number
- Case Type
- Case Status
- Current Stage
- Submission Date
- Last Updated Date
- Channel

---

# Individual Search Requirements

Users shall be able to search for individual-related cases using:

- CPR
- Personal Identifier
- Full Name
- Mobile Number
- Email Address

Additional criteria may be configured through metadata.

---

# Organization Search Requirements

Users shall be able to search for organization-related cases using:

- Organization Name
- Commercial Registration Number
- NGO Identifier
- Organization Identifier

Additional criteria may be configured through metadata.

---

# Administrative Search Requirements

Users shall be able to search for administrative cases using:

- Employee Identifier
- Employee Name
- Department
- Directorate

Additional criteria may be configured through metadata.

---

# Metadata Search Requirements

The platform shall support searching using metadata-defined fields.

Examples:

- Service Type
- Complaint Category
- Priority
- Region
- Subject

Metadata-defined fields shall be configurable by authorized Business Analysts and Administrators.

---

# Date-Based Searches

Users shall be able to search using date ranges.

Examples:

- Submitted Date
- Completed Date
- Last Updated Date

Date range search shall support:

- From Date
- To Date

---

# Status-Based Searches

Users shall be able to filter by:

- Case Status
- Current Stage

Status and stage are separate concepts.

---

## Case Status

Represents the overall business state of a case.

Examples:

- Draft
- Submitted
- In Progress
- On Hold
- Rejected
- Completed
- Archived

---

## Current Stage

Represents the current operational stage within a case lifecycle.

Examples:

- Technical Review
- Investigation
- Approval
- Verification

Stage definitions may vary by Case Type.

---

# Search Result Requirements

Search results shall support displaying configurable columns.

Common columns include:

- Case Number
- Case Type
- Status
- Current Stage
- Submission Date
- Last Updated Date

Case-specific columns may be configured through metadata.

---

# Search Result Actions

Users may perform actions based on their permissions.

Examples:

- Open Case
- View History
- View Attachments

Available actions shall respect authorization rules.

---

# Search Filters

Users shall be able to refine results using filters.

Examples:

- Case Type
- Status
- Current Stage
- Date Range
- Channel
- Department

The available filters may vary by Case Type.

---

# Saved Searches

The platform may support saving search definitions.

Examples:

- My Open Cases
- Recently Submitted Cases
- Pending Approvals

Saved searches are user-specific.

---

# Search Security

All search operations shall enforce case visibility rules.

Requirements:

- Unauthorized cases shall not appear in results.
- Unauthorized case counts shall not be disclosed.
- Unauthorized metadata shall not be exposed.
- Search suggestions shall respect visibility rules.

Search security shall be enforced before result generation.

---

# Search Across Channels

Users shall be able to search cases regardless of originating channel.

Examples:

- Case Management Registry
- BPM Forms
- E-Service Portals
- Service Counters
- Contact Centers
- Email

Channel information may be used as a search criterion.

---

# Search Configuration

The platform shall support metadata-driven search configuration.

Configuration may define:

- Search Fields
- Search Filters
- Default Sorting
- Search Result Columns
- Search Result Actions

This configuration shall be manageable by authorized Business Analysts and Administrators.

---

# Future Search Enhancements

The following capabilities are outside the initial platform scope:

- Full Text Search
- Fuzzy Matching
- Phonetic Search
- AI-Assisted Search
- Natural Language Search
- Elasticsearch Integration

These capabilities may be introduced in future phases.

---

# Search Requirements Status

Status: Draft v1.0
