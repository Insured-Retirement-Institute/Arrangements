# IRI Index Performance Lock (IPL) API

## Overview

This repository consolidates **Index Performance Lock (IPL) arrangement transactions** for annuity products under the **IRI Digital First vision**.

- **Submit Index Performance Lock Arrangement**
- **Update Index Performance Lock Arrangement**

The initiative modernizes legacy batch/manual Index Performance Lock processes into RESTful APIs for secure, scalable, and interoperable processing. It leverages industry standards and provides a unified approach for carriers, distributors, and solution providers.

Index Performance Lock arrangements allow policy owners and financial professionals to establish performance-lock strategies based on defined threshold values. The arrangement supports upside performance locks, downside performance locks, and on-demand lock requests.

---

# Business Case

## Problem Statement

- Legacy XML/SOAP systems are inefficient, lack modern security, and are nearing end-of-life.
- REST APIs provide real-time, lightweight, and secure data exchange.

## Objectives

- Transition all IFT processing to RESTful API architecture.
- Reduce payload size and processing time.
- Improve integration velocity and security.
- Ensure long-term support and interoperability.

## Key Features

- OpenAPI/Swagger documentation for easy onboarding.
- Accelerated development using validated payload structures.
- Standardized testing framework for early and consistent validation.
- Open contribution to IRI standards for industry-wide adoption.
- OpenAPI 3.1.x specifications with example payloads and conditional validation.
- Data Dictionary for field-level definitions and code lists.
- Unified endpoint with transactionType routing.
- Asynchronous Day‑1 / Day‑2 processing model with lifecycle tracking.
- Event-driven Day‑2 confirmation notifications delivered through Advanced Event Mesh.

---

# Supported Transactions

## 1. Submit Index Performance Lock Arrangement

Handles creation of a new Index Performance Lock arrangement.

**Endpoint:**  
`/v1/policies/{policyNumber}/arrangements/index-performance-lock`

**Folder:**  
`./SubmitIndexPerformanceLock/`

### User Stories

- As a Policy Administrator, I want to create an Index Performance Lock arrangement so that performance lock strategies can be established on a policy.
- As a System Integrator, I want to submit Index Performance Lock arrangements and receive lifecycle updates so enterprise systems remain synchronized.
- As a Financial Advisor, I want to establish performance lock thresholds aligned with client investment objectives.

### Personas

**Policy Administrator**  
Goals: Create and maintain Index Performance Lock arrangements.  
Needs: Clear validation rules and submission confirmations.  
Pain Points: Missing configuration data and invalid threshold settings.

**System Integrator**  
Goals: Integrate submission and lifecycle tracking capabilities.  
Needs: Stable request/response contracts and event-based confirmations.  
Pain Points: Asynchronous processing visibility and downstream synchronization.

**Financial Advisor**  
Goals: Implement performance lock strategies that align with client objectives.  
Needs: Visibility into threshold values, arrangement types, and participating funds.  
Pain Points: Complex lock configurations and monitoring requirements.

---

## 2. Update Index Performance Lock Arrangement

Handles scenarios where an existing Index Performance Lock arrangement is modified, overridden, corrected, or cancelled.

**Endpoint:**  
`/v1/policies/{policyNumber}/arrangements/index-performance-lock/{arrangementId}`

**Folder:**  
`./UpdateIndexPerformanceLock/`

### User Stories

- As a Policy Administrator, I want to update an Index Performance Lock arrangement so that threshold values remain accurate.
- As a System Integrator, I want to synchronize IPL updates across enterprise systems so arrangement data remains consistent.
- As a Financial Advisor, I want to modify lock thresholds and arrangements when client goals change.

### Personas

**Policy Administrator**  
Goals: Maintain accurate and current performance lock arrangements.  
Needs: Validation rules, update constraints, and lifecycle visibility.  
Pain Points: Incorrect threshold values, missing identifiers, inconsistent updates.

**System Integrator**  
Goals: Synchronize arrangement updates across connected systems.  
Needs: Consistent update endpoints and clear schema behavior.  
Pain Points: Conditional validation complexity and lifecycle coordination.

**Financial Advisor**  
Goals: Maintain client lock strategies over time.  
Needs: Ability to adjust lock thresholds and arrangement types.  
Pain Points: Complexity in lock modifications and business rule interpretation.

---

# Schema Overview

Sample of what most schemas include (Submit and Update IPL arrangements share a common structure, with Update additionally requiring an arrangementId path parameter):

- **Root Attributes:** effectiveDate, externalArrangementId, nsccParticipantId, cusip, actionIndicator, allocationOption.
- **Arrangement Details:** productCode, arrangementType (PERFORMANCE_LOCK), arrangementSubType, startDate.
- **Fund Details:** transferToFunds, fundId, requestedPercentage.
- **Audit Information:** auditTotalType, auditTotal, correlationGuid, correlationIdState.
- **Parties:** IndividualParty or EntityParty participants associated with the arrangement.
- **Producer Information:** producerNumber, npn, crdNumber.

Detailed schemas for Submit and Update transactions are available in their respective folders.

---

# Response Schema Overview

All API operations—across synchronous and asynchronous processing models—adhere to a unified **Error schema**.

## Success Response Expectations

### Submit (POST) Response

- Return **HTTP 201 Created**
- Include a Location header pointing to:

```text
/v1/policies/{policyNumber}/arrangement-requests/{requestId}
```

- Return an `IndexPerformanceLockResponse` object including:
  - requestId
  - status (ACCEPTED)

### Update (PUT) Response

- Return **HTTP 202 Accepted**
- Include a Location header pointing to:

```text
/v1/policies/{policyNumber}/arrangement-requests/{requestId}
```

- Return an `IndexPerformanceLockResponse` object including:
  - requestId
  - status (ACCEPTED)

---

## Lifecycle Status (GET Response)

A GET endpoint is available to retrieve the current status of the arrangement request.

- Return **HTTP 200**
- Response includes:
  - requestId
  - status
  - effectiveDate
  - message

### Status Values

| Status | Description |
|----------|----------|
| IN_PROGRESS | Request is being processed asynchronously |
| SUCCESS | Transaction completed successfully |
| REJECTED | Transaction failed validation or processing |

---

# Standard Error Schema

Every error response includes:

- An HTTP status code in the **400–599** range
- A structured and validated **error code**
- A **timestamp** indicating when the error occurred
- A developer-focused **technical message** (`message`)
- A **correlationId** for cross-system tracing
- A **field-level** or **rule-level** error collection

---

# Key Fields

| Field | Description |
|----------|----------|
| **httpStatus** | Numeric HTTP status code (400–599). |
| **code** | Structured error code identifier. |
| **correlationId** | Returned as a response header on all responses, including errors. |
| **message** | End-user-friendly explanation, safe to show in portals or consumer-facing applications. |
| **validationErrors** | Array of domain/business rule violations. |

---

# Purpose & Benefits

This standardized error structure ensures:

- A predictable experience across all APIs.
- Clear differentiation between developer diagnostics and user-safe messages.
- Enhanced traceability.
- Support for granular validation feedback.
- Easier monitoring, logging, and cross-system troubleshooting.

---

# Day‑2 Asynchronous Processing

Index Performance Lock transactions use an asynchronous processing model after initial submission.

Upon successful validation:

- **POST (Submit)** returns HTTP 201 and assigns a requestId.
- **PUT (Update)** returns HTTP 202 and assigns a requestId.

These responses confirm acceptance but do not indicate final processing completion.

Processing continues asynchronously in downstream systems.

## Delivery Model

Day‑2 confirmation events are published to the enterprise event mesh (Advanced Event Mesh).

Consumers receive confirmations through topic-based subscriptions.

Day‑2 confirmations are event-driven only.

## Day‑2 Confirmation

Final transaction outcomes are communicated through a Day‑2 arrangement confirmation event.

Supported outcomes include:

- SUCCESS
- SUCCESS_WITH_INFO
- FAILURE

## Day‑2 Schema

Day‑2 confirmation events conform to a canonical Day‑2 Arrangement Confirmation schema.

The schema includes:

- Event metadata (eventId, eventTimestamp, eventType)
- Transaction identifiers (requestId, policyNumber)
- Processing outcome (status, message)
- Execution information (transactionType, execution date and time)

Supported transactionType values include:

- SUBMIT_INDEX_PERFORMANCE_LOCK
- UPDATE_INDEX_PERFORMANCE_LOCK
- CANCEL_INDEX_PERFORMANCE_LOCK

## Status Visibility

A GET lifecycle endpoint is available to retrieve processing status using the requestId.

The GET endpoint provides operational visibility and audit support.

The GET endpoint does not replace Day‑2 confirmation events as the source of final confirmation.

---

# OpenAPI Specs

Unified Swagger documentation for all Index Performance Lock arrangement endpoints is available in the `openapi-specs/` folder.

The specifications include:

- Submit (POST) Index Performance Lock endpoint definitions
- Update (PUT) Index Performance Lock endpoint definitions
- Lifecycle (GET) status retrieval endpoint
- Request and response schemas with example payloads
- Day‑2 confirmation event schema
- Conditional validation rules using OpenAPI 3.1 constructs
- Standardized error responses and reusable components

These OpenAPI definitions are designed to support consistent integration, validation, and implementation across carriers, distributors, and solution providers.

# Code of Conduct

Please review and adhere to the **Code of Conduct** and **Style Guide** provided in the repository.

---

# How to Contribute

- Fork the repository and submit pull requests.
- Report issues via the Issues tab.
- Join working groups: **hpikus@irionline.org**

---

# Business Owners

- **Carrier Business Owner:** digitalfirst@brighthousefinancial.com
- **Distributor Business Owner:** [contact]
- **Solution Provider Business Owner:** [contact]

---

# Versioning

- Follow semantic versioning for specification updates.
- Document changes in commit messages and changelogs.

---