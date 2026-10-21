# IRI Allocation Instructions (AI) API

## Overview
**This repository consolidates Allocation Instructions (AI) arrangement transactions for annuity products under the IRI Digital First vision**.

- **Submit Allocation Instructions Arrangement**
- **Update Allocation Instructions Arrangement**

The initiative modernizes legacy batch/manual Allocation Instructions processes into RESTful APIs for secure, scalable, and interoperable processing. It leverages industry standards and provides a unified approach for carriers, distributors, and solution providers.

Allocation Instructions allow contract owners and financial professionals to specify how premiums, transfers, or other transaction values should be allocated among available investment options. These instructions help ensure that future allocations are directed according to the desired investment strategy.

---

## Business Case

### Problem Statement
- Legacy XML/SOAP systems are inefficient, lack modern security, and are nearing end-of-life.
- REST APIs provide real-time, lightweight, and secure data exchange.

### Objectives
- Transition all IFT processing to RESTful API architecture.
- Reduce payload size and processing time.
- Improve integration velocity and security.
- Ensure long-term support and interoperability.

### Key Features
- OpenAPI/Swagger documentation for easy onboarding.
- Accelerated development using validated payload structures.
- Standardized testing framework for early and consistent validation.
- Open contribution to IRI standards for industry-wide adoption.
- OpenAPI 3.1.x specifications with example payloads and conditional validation.
- Data Dictionary for field-level definitions and code lists.
- Unified endpoint with transactionType routing.
- Asynchronous Day-1 / Day-2 processing model with lifecycle tracking.
- Event-driven Day-2 confirmation notifications delivered through Advanced Event Mesh.

---

## Supported Transactions

### 1. Submit Allocation Instructions Arrangement
Handles creation of a new Allocation Instructions arrangement.

**Endpoint:** `/v1/policies/{policyNumber}/arrangements/allocation-instructions`

**Folder:** `./SubmitAllocationInstructions/`

#### User Stories
- As a Policy Administrator, I want to create Allocation Instructions so that future allocations follow the selected investment strategy.
- As a Financial Advisor, I want to establish allocation instructions for a client so future premiums and transfers are distributed according to the desired investment allocation.
- As a System Integrator, I want to submit Allocation Instructions arrangements and receive lifecycle updates so enterprise systems remain synchronized.

#### Personas

**Policy Administrator**  
Goals: Create and maintain Allocation Instructions arrangements.  
Needs: Clear validation rules and submission confirmations.  
Pain Points: Missing allocation data and invalid fund selections.

**System Integrator**  
Goals: Integrate submission and lifecycle tracking capabilities.  
Needs: Stable request/response contracts and event-based confirmations.  
Pain Points: Asynchronous tracking and data synchronization.

**Financial Advisor**  
Goals: Establish allocation instructions aligned to client objectives.  
Needs: Visibility into destination funds and allocation percentages.  
Pain Points: Complex investment allocation configurations.

---

### 2. Update Allocation Instructions Arrangement

Handles scenarios where an existing Allocation Instructions arrangement is updated.

**Endpoint:** `/v1/policies/{policyNumber}/arrangements/allocation-instructions/{arrangementId}`

**Folder:** `./UpdateAllocationInstructions/`

#### User Stories
- As a Policy Administrator, I want to update an Allocation Instructions arrangement so that future allocations follow the revised investment strategy.
- As a System Integrator, I want to synchronize Allocation Instructions updates across enterprise platforms so that investment allocation data remains consistent.
- As a Financial Advisor, I want to modify investment allocation instructions so that future deposits and transfers align with client objectives.

#### Personas
**Policy Administrator**  
Goals: Maintain accurate and up-to-date Allocation Instructions arrangements.
Needs: Clear validation rules, update constraints, and lifecycle visibility.
Pain Points: Incorrect allocation data, missing required fields, inconsistent updates.

**System Integrator**  
Goals: Implement and maintain Allocation Instructions updates across systems.
Needs: Clear API contracts, example payloads, and validation guidance.
Pain Points: Schema interpretation differences, conditional validation complexity, lifecycle synchronization. 

**Financial Advisor**  
Goals: Ensure future allocations follow the intended investment strategy.
Needs: Visibility into allocation percentages, destination funds, and arrangement rules.
Pain Points: Complex allocation configurations and inconsistent implementation across carriers.

---

## Schema Overview

Sample of what the Allocation Instructions Submit and Update schemas include:

- Root Attributes: effectiveDate, nsccParticipantId, externalArrangementId (Submit only), cusip, actionIndicator.
- Arrangement Details:  productCode, arrangementType, arrangementSubType.
- Fund Details:
  - transferFromFunds
  - transferToFunds
  - openDate 
- Audit Information:  auditTotalType, auditTotal, correlationGuid, correlationIdState.
- Parties:  IndividualParty or EntityParty participants associated with the arrangement.
- Producer Information:  producerNumber, npn, crdNumber.

Detailed schemas for Submit and Update transactions are available in their respective folders.

---

# Response Schema Overview

All API operations—across synchronous and asynchronous processing models—adhere to a unified **Error schema**. This ensures consistent error handling, predictable integration behavior, and standardized troubleshooting across all Allocation Instructions transactions.

## Success Response Expectations

### Submit (POST) Response
- Return **HTTP 201 Created**
- Include a Location header pointing to:
  `/v1/policies/{policyNumber}/arrangement-requests/{requestId}`
- Return an `AllocationInstructionsResponse` including:
  - requestId
  - status (CREATED)

### Update (PUT) Response
- Return **HTTP 202 Accepted**
- Include a Location header pointing to:
  `/v1/policies/{policyNumber}/arrangement-requests/{requestId}`
- Return an `AllocationInstructionsResponse` including:
  - requestId
  - status (ACCEPTED)

---

## Lifecycle Status (GET Response)

A GET endpoint is available to retrieve the current status of the arrangement request.

- Return **HTTP 200 SUCCESS**
- Response includes 'requestId', 'status', 'effectiveDate', and 'message'

### Status Values

| Status | Description |
|----------|----------|
| IN_PROGRESS | Request is actively being processed |
| SUCCESS | Processing completed successfully |
| REJECTED | Processing failed validation or business rules |

---

## Standard Error Schema

Every error response—regardless of transaction type—includes:
- An HTTP status code in the **400–599** range
- A structured and validated **error code**
- A **timestamp** of when the error was generated
- A developer‑focused **technical message** (`message`)
- A **correlationId** for cross‑system tracing
- A **field‑level** or **rule‑level** error collection

---

## Key Fields

| Field | Description |
|-------|-------------|
| **httpStatus** | Numeric HTTP status code (400–599) representing the type and severity of the failure. |
| **code** | Structured identifier in the enforced format. Enables machine‑readable error handling. |
| **correlationId** | correlationId is returned as a response header on all responses, including errors. It is not included in the response body. |
| **message** | End‑user‑friendly explanation, safe to show in portals or consumer‑facing apps. |
| **validationErrors** | Array describing domain/business rule violations; each entry requires its own code and message. |

---

## Purpose & Benefits

This standardized error structure ensures:
- A **predictable experience** across all APIs (synchronous + asynchronous)
- Clear differentiation between **developer diagnostics** and **user‑safe messages**
- Enhanced **traceability** for carriers, distributors, and integrators
- Support for granular **validation feedback** and complex business rule logic
- Easier **monitoring, logging, and cross‑system troubleshooting**


# Day‑2 Asynchronous Processing
Allocation Instructions arrangement transactions use an asynchronous processing model after initial request submission.  
Upon successful validation:
- POST (Submit) returns HTTP 201 (Created) and assigns a unique requestId.
- PUT (Update) returns HTTP 202 (Accepted) and assigns a unique requestId.

These responses confirm acceptance of the request but do not indicate final transaction completion.  
Final processing occurs asynchronously in downstream systems.

## Delivery Model
Day‑2 confirmation events are published to the enterprise event mesh (Advanced Event Mesh).  
Consumers receive confirmations through topic‑based subscriptions.  
Day‑2 confirmations are event‑driven only.

## Day‑2 Confirmation
Final transaction outcomes are communicated via a Day‑2 arrangement confirmation event.  
The event represents a terminal state of the arrangement lifecycle.  
Each event includes the original requestId for correlation and traceability.  

Supported outcomes include:
- SUCCESS
- SUCCESS_WITH_INFO
- FAILURE

## Day‑2 Schema
Day‑2 confirmation events conform to a canonical Day‑2 Arrangement Confirmation schema.  

The schema defines the standardized event structure, including:
- Event metadata (eventId, eventTimestamp, eventType)
- Transaction identifiers (requestId, policyNumber)
- Processing outcome (status, message)
- Execution details (transactionType, execution date and time)

Supported transactionType values include:
- SUBMIT_ALLOCATION_INSTRUCTIONS
- UPDATE_ALLOCATION_INSTRUCTIONS
- CANCEL_ALLOCATION_INSTRUCTIONS

## Status Visibility
A GET lifecycle endpoint is available to retrieve the current processing status using the requestId.  

The GET endpoint provides operational visibility and audit support.  
The GET endpoint does not replace Day‑2 event delivery as the source of final confirmation.

---

## OpenAPI Specs
Unified Swagger documentation for all Allocation Instructions arrangement endpoints is available in the `openapi-specs/` folder.

The specifications include:
- Submit (POST) Allocation Instructions endpoint definitions
- Update (PUT) Allocation Instructions endpoint definitions
- Lifecycle (GET) status retrieval endpoint
- Request and response schemas with example payloads
- Day‑2 confirmation event schema
- Conditional validation rules using OpenAPI 3.1 constructs
- Standardized error responses and reusable components

These OpenAPI definitions are designed to support consistent integration, validation, and implementation across carriers, distributors, and solution providers.


## Code of Conduct

Please review and adhere to the **Code of Conduct** and **Style Guide** provided in the repository to ensure consistency and professionalism.

---

## How to Contribute

- Fork the repo and submit pull requests.
- Report issues via the **Issues** tab.
- Join working groups: **hpikus@irionline.org**.

---

## Business Owners

- **Carrier Business Owner:** digitalfirst@brighthousefinancial.com  
- **Distributor Business Owner:** [contact]  
- **Solution Provider Business Owner:** [contact] 

---

## Versioning ##
- Follow semantic versioning for spec updates.
- Document changes in commit messages and changelogs to support integrator adoption.
---
