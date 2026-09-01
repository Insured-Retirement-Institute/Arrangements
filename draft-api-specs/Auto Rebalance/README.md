# IRI Auto Rebalance (AR) API

## Overview
This repository consolidates **Auto Rebalance (AR) arrangement transactions** for annuity products under the **IRI Digital First vision**.

- **Submit Auto Rebalance Arrangement**
- **Update / Override / Cancel Auto Rebalance Arrangement**

The initiative modernizes legacy batch/manual Auto Rebalance processes into RESTful APIs for secure, scalable, and interoperable processing. It leverages industry standards and provides a unified approach for carriers, distributors, and solution providers.

Auto Rebalance is an investment management strategy that periodically realigns contract allocations to a predefined target allocation by transferring values among investment options. This helps maintain the desired asset allocation over time and reduces portfolio drift caused by market performance differences.

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

---

## Supported Transactions

### 1. Submit Auto Rebalance Arrangement
Handles scenarios where a new Auto Rebalance arrangement is submitted.
**Endpoint:** `/v1/policies/{policyNumber}/arrangements/auto-rebalance`

### 2. Update / Override / Cancel Auto Rebalance Arrangement
Handles scenarios where an existing Auto Rebalance arrangement is updated, overridden, or cancelled.  
**Endpoint:** `/v1/policies/{policyNumber}/arrangements/auto-rebalance/{arrangementId}`

---
## Supported Transactions

### 1. Submit (Create) Auto Rebalance Arrangement
Handles scenarios where a new Auto Rebalance arrangement is established for a policy, enabling periodic reallocation of contract values to target investment allocations.
**Folder:** `./submitautorebalance/`

#### User Stories
- As a Policy Administrator, I want to validate all required fields before submitting an Auto Rebalance arrangement so that I can avoid processing delays. 
- As a System Integrator, I want to map the arrangement schema to our internal data model so that we can automate rebalance setup and maintenance. 
- As a Financial Advisor, I want to configure rebalance frequency and target fund allocations so that client investment allocations remain aligned with long-term objectives. 

#### Personas
**Policy Administrator**  
Goals: Ensure accurate and timely setup of Auto Rebalance arrangements. 
Needs: Clear schema definitions, validation rules, and lifecycle visibility.  
Pain Points: Manual setup errors, missing required fields, inconsistent formats.  

**System Integrator**  
Goals: Implement Auto Rebalance submission workflows in backend systems and APIs. 
Needs: JSON schema formats, sample payloads, and validation guidance.  
Pain Points: Ambiguous field definitions, conditional rule complexity, integration inconsistencies.  

**Financial Advisor**  
Goals: Maintain target allocation strategies over time. 
Needs: Clear understanding of rebalance frequencies, allocation percentages, and arrangement rules.
Pain Points: Complex configuration logic, lack of standardization, manual processes.  

---

### 2. Update / Override / Cancel Auto Rebalance Arrangement
Handles scenarios where an existing Auto Rebalance arrangement is modified, overridden, corrected, or cancelled using an arrangementId. 
**Folder:** `./updateautorebalance/`

#### User Stories
- As a Policy Administrator, I want to update an existing arrangement so that I can correct or adjust rebalance details. 
- As a System Integrator, I want to synchronize updated arrangement data across systems so that lifecycle management remains consistent.  
- As a Financial Advisor, I want to adjust allocation strategies or transfer frequency so that they reflect changing client needs.  

#### Personas
**Policy Administrator**  
Goals: Maintain accurate and up-to-date Auto Rebalance arrangements.
Needs: Clear validation rules, update constraints, and audit traceability.  
Pain Points: Inconsistent updates, missing identifiers, lack of visibility into lifecycle changes.  

**System Integrator**  
Goals: Synchronize arrangement updates efficiently across systems.
Needs: Reliable update endpoints and consistent schema behavior.  
Pain Points: Versioning challenges, conditional validation complexity, handling partial updates.  

**Financial Advisor**  
Goals: Modify rebalancing strategies as client needs evolve. 
Needs: Flexible update mechanisms and clarity on the impact of changes.  
Pain Points: Complex override logic, unclear constraints when modifying arrangements.  

---

## Schema Overview
Sample of what most schemas include (Submit and Update Auto Rebalance arrangements share a common structure, with Update additionally requiring an arrangementId path parameter):

- **Root Attributes:** effectiveDate, externalArrangementId (Submit only), nsccParticipantId, cusip, actionIndicator.
- **Arrangement Details:**  productCode, arrangementType (ASSET_REBALANCING), arrangementSubType, frequency, startDate, endDate.
- **Transfer Configuration:** destinationTransferAmountType (PERCENT).
- **Funds:** transferToFunds[] with target allocation percentages and fund identifiers.
- **Audit Information:** auditTotalType, auditTotal, correlationGuid, correlationIdState.
- **Parties:** parties[] as a direct array of individual or entity objects with allocationPercentage, paymentForm, and relationships.
- **Producer Info:** producerNumber, npn, crdNumber.
- **Validation Rules:** conditional enforcement of amount vs percentage fields.

Detailed schemas for Submit and Update transactions are available in their respective folders.

---

# Response Schema Overview

All API operations—across synchronous and asynchronous processing models—adhere to a unified **Error schema**. This ensures consistent error handling, predictable integration behavior, and standardized troubleshooting across all Auto Rebalance arrangement transactions.

## Success Response Expectations

### Submit (POST) Response
- Return **HTTP 201 Created**.
- Include a Location header pointing to /v1/policies/{policyNumber}/arrangement-requests/{requestId}
- Return a 'Auto RebalanceResponse' object in the response body, including 'status'(CREATED, REJECTED), and 'correlationId'
- A GET endpoint is available to retrieve the lifecycle status with **HTTP 200 — SUCCESS**.

### Update (PUT) Response
- Return **HTTP 202 Accepted**.
- Include a Location header pointing to /v1/policies/{policyNumber}/arrangement-requests/{requestId}
- Return a 'Auto RebalanceResponse' object in the response body, including 'status'(ACCEPTED, REJECTED) and 'correlationId'
- A GET endpoint is available to retrieve the lifecycle status with **HTTP 200 — SUCCESS**.

---

## Lifecycle Status (GET Response)

A GET endpoint is available to retrieve the current status of the arrangement request.

- Return **HTTP 200 SUCCESS**
- Response includes 'requestId', 'status', 'effectiveDate', and 'message'

### Status Values

| Status | Description |
|--------|-------------|
| **IN_PROGRESS** | Request is being processed asynchronously |
| **SUCCESS** | Transaction completed successfully |
| **REJECTED** | Transaction failed validation or processing |

---

## Standard Error Schema

Every error response—regardless of transaction type—includes:
- An HTTP status code in the **400–599** range
- A structured and validated **error code**
- A **timestamp** of when the error was generated
- A developer‑focused **technical message** (`message`)
- A **correlationId** for cross‑system tracing
- A **field‑level** or **rule‑level** error collections

---

## Key Fields

| Field | Description |
|-------|-------------|
| **httpStatus** | Numeric HTTP status code (400–599) representing the type and severity of the failure. |
| **code** | Structured identifier in the enforced format: `domain.category.subcategory`. Enables machine‑readable error handling. |
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
Auto Rebalance arrangement transactions use an asynchronous processing model after initial request submission.  
Upon successful validation:
- **POST (Submit)** returns HTTP 201 (Created) and assigns a unique requestId.  
- **PUT (Update)** returns HTTP 202 (Accepted) and assigns a unique requestId.  

These responses confirm acceptance of the request but do not indicate final transaction completion.  
Final processing occurs asynchronously in downstream systems.

## Delivery Model
Day‑2 confirmation events are published to the enterprise event mesh (SAP Advanced Event Mesh / Solace).  
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
- SUBMIT_AUTO_REBALANCE
- UPDATE_AUTO_REBALANCE

## Status Visibility
A GET lifecycle endpoint is available to retrieve the current processing status using the requestId.  

The GET endpoint provides operational visibility and audit support.  
The GET endpoint does not replace Day‑2 event delivery as the source of final confirmation.

---

## OpenAPI Specs
Unified Swagger documentation for all Auto Rebalance arrangement endpoints is available in the `openapi-specs/` folder.

The specifications include:
- Submit (POST) Auto Rebalance arrangement endpoint definitions
- Update (PUT) Auto Rebalance arrangement endpoint definitions
- Lifecycle (GET) status retrieval endpoint
- Request and response schemas with example payloads
- Conditional validation rules using OpenAPI 3.1 constructs
- Standardized error responses and reusable components

These OpenAPI definitions are designed to support consistent integration, validation, and implementation across carriers, distributors, and solution providers.

---

## Versioning ##
- Follow semantic versioning for spec updates.
- Document changes in commit messages and changelogs to support integrator adoption.
---

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
