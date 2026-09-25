# IRI Systematic Investment (SI) API

## Overview
This repository consolidates **Systematic Investment (SI) arrangement transactions** for annuity products under the **IRI Digital First vision**.

- **Submit Systematic Investment Arrangement**
- **Update Systematic Investment Arrangement**

The initiative modernizes legacy batch/manual Systematic Investment processes into RESTful APIs for secure, scalable, and interoperable processing. It leverages industry standards and provides a unified approach for carriers, distributors, and solution providers.

Systematic Investment is an investment funding strategy that automatically directs recurring premiums or contributions into investment options according to predefined allocation instructions. The arrangement supports both amount-based and percentage-based allocations, helping contract owners maintain disciplined investing practices and pursue long-term financial objectives through scheduled investment activity.

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

### 1. Submit Systematic Investment Arrangement

Handles scenarios where a new Systematic Investment arrangement is submitted.

**Endpoint:** /v1/policies/{policyNumber}/arrangements/systematic-investment

### 2. Update Systematic Investment Arrangement

Handles scenarios where an existing Systematic Investment arrangement is updated, overridden, or cancelled.

**Endpoint:** /v1/policies/{policyNumber}/arrangements/systematic-investment/{arrangementId}

## Supported Transactions

### 1. Submit (Create) Systematic Investment Arrangement

Handles scenarios where a new Systematic Investment arrangement is established for a policy, enabling recurring investment of premiums or contributions according to predefined allocation instructions.

**Folder:** ./submitsystematicinvestment/

#### User Stories

- As a Policy Administrator, I want to validate all required fields before submitting a Systematic Investment arrangement so that I can avoid processing delays.
- As a System Integrator, I want to map the arrangement schema to our internal data model so that we can automate Systematic Investment setup and maintenance.
- As a Financial Advisor, I want to establish recurring investment allocations and funding instructions so that policy owners can systematically invest according to their financial goals.

#### Personas

**Policy Administrator**  
Goals: Ensure accurate and timely setup of Systematic Investment arrangements.  
Needs: Clear schema definitions, validation rules, and lifecycle visibility.  
Pain Points: Manual setup errors, missing required fields, inconsistent formats.

**System Integrator**  
Goals: Implement Systematic Investment submission workflows in backend systems and APIs.  
Needs: JSON schema formats, sample payloads, and validation guidance.  
Pain Points: Ambiguous field definitions, conditional rule complexity, integration inconsistencies.

**Financial Advisor**  
Goals: Establish recurring investment programs aligned with client objectives.  
Needs: Flexibility in allocation methods, funding schedules, and investment frequencies.  
Pain Points: Complex setup logic, inconsistent carrier implementations, manual processing requirements.

### 2. Update Systematic Investment Arrangement

Handles scenarios where an existing Systematic Investment arrangement is modified, overridden, corrected, or cancelled using an arrangementId.

**Folder:** ./updatesystematicinvestment/

#### User Stories

- As a Policy Administrator, I want to update an existing Systematic Investment arrangement so that I can correct or modify investment details.
- As a System Integrator, I want to synchronize arrangement updates across systems so that investment instructions remain consistent throughout the arrangement lifecycle.
- As a Financial Advisor, I want to adjust contribution allocations, frequencies, or funding information so that the arrangement reflects changing client needs.

#### Personas

**Policy Administrator**  
Goals: Maintain accurate and current Systematic Investment arrangements.  
Needs: Clear validation rules, update constraints, and audit traceability.  
Pain Points: Inconsistent updates, missing identifiers, lack of visibility into arrangement lifecycle changes.

**System Integrator**  
Goals: Synchronize arrangement updates efficiently across connected systems.  
Needs: Reliable update endpoints and consistent schema behavior.  
Pain Points: Versioning challenges, conditional validation complexity, maintaining data consistency.

**Financial Advisor**  
Goals: Update investment strategies and funding instructions as client objectives evolve.  
Needs: Flexible update mechanisms and transparency into arrangement impacts.  
Pain Points: Complex update logic, uncertainty regarding downstream effects of arrangement changes.

---

### Schema Overview

Sample of what most schemas include (Submit and Update Systematic Investment arrangements share a common structure, with Update additionally requiring an arrangementId path parameter):

- Root Attributes: effectiveDate, externalArrangementId (Submit only), nsccParticipantId, cusip, actionIndicator.
- **Arrangement Details:** productCode, arrangementType, arrangementSubType, startDate, endDate, frequency, destinationTransferAmountType, nextTransactionDate (Update only).
- **Funds:** destinationTransferAmountType controls whether allocations are amount-based, percentage-based, standing allocation, or pro-rata allocation.
- **Audit Information:** auditTotalType, auditTotal, correlationGuid, correlationIdState.
- **Payer Information:** payer as either an individual or entity, including optional bank and address details used to support funding instructions.
- **Producer Info:** producerNumber, npn, crdNumber.

Detailed schemas for Submit and Update transactions are available in their respective folders.

---

## Response Schema Overview

All API operations, across synchronous and asynchronous processing models, adhere to a unified **Error schema**. This ensures consistent error handling, predictable integration behavior, and standardized troubleshooting across all Systematic Investment arrangement transactions.

### Success Response Expectations

#### Submit (POST) Response

- Return **HTTP 201 Created**.
- Include a Location header pointing to /v1/policies/{policyNumber}/arrangement-requests/{requestId}
- Return a 'SystematicInvestmentResponse' object in the response body, including 'status' (CREATED) and 'requestId'
- A GET endpoint is available to retrieve the lifecycle status with **HTTP 200 — SUCCESS**.

#### Update (PUT) Response

- Return **HTTP 202 Accepted**.
- Include a Location header pointing to /v1/policies/{policyNumber}/arrangement-requests/{requestId}
- Return a 'SystematicInvestmentResponse' object in the response body, including 'status' (ACCEPTED) and 'requestId'
- A GET endpoint is available to retrieve the lifecycle status with **HTTP 200 — SUCCESS**.

### Lifecycle Status (GET Response)

A GET endpoint is available to retrieve the current status of the arrangement request.

- Return **HTTP 200 SUCCESS**
- Response includes 'requestId', 'status', 'effectiveDate', and 'message'

#### Status Values

| Status | Description |
|----------|----------|
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


## Day‑2 Asynchronous Processing

Systematic Investment arrangement transactions use an asynchronous processing model after initial request submission.

Upon successful validation:

- **POST (Submit)** returns HTTP 201 (Created) and assigns a unique requestId.
- **PUT (Update)** returns HTTP 202 (Accepted) and assigns a unique requestId.

These responses confirm acceptance of the request but do not indicate final transaction completion.

Final processing occurs asynchronously in downstream systems.

### Delivery Model

Day‑2 confirmation events are published to the enterprise event mesh (Advanced Event Mesh).

Consumers receive confirmations through topic-based subscriptions.

Day‑2 confirmations are event-driven only.

### Day‑2 Confirmation

Final transaction outcomes are communicated via a Day‑2 arrangement confirmation event.

The event represents a terminal state of the arrangement lifecycle.

Each event includes the original requestId for correlation and traceability.

Supported outcomes include:

- SUCCESS
- SUCCESS_WITH_INFO
- FAILURE

### Day‑2 Schema

Day‑2 confirmation events conform to a canonical Day‑2 Arrangement Confirmation schema.

The schema defines the standardized event structure, including:

- Event metadata (eventId, eventTimestamp, eventType)
- Transaction identifiers (requestId, policyNumber)
- Processing outcome (status, message)
- Execution details (transactionType, execution date and time)

Supported transactionType values include:

- SUBMIT_SYSTEMATIC_INVESTMENT
- UPDATE_SYSTEMATIC_INVESTMENT
- CANCEL_SYSTEMATIC_INVESTMENT

### Status Visibility

A GET lifecycle endpoint is available to retrieve the current processing status using the requestId.

The GET endpoint provides operational visibility and audit support.

The GET endpoint does not replace Day‑2 event delivery as the source of final confirmation.

### OpenAPI Specs

Unified Swagger documentation for all Systematic Investment arrangement endpoints is available in the openapi-specs/ folder.

The specifications include:

- Submit (POST) Systematic Investment arrangement endpoint definitions
- Update (PUT) Systematic Investment arrangement endpoint definitions
- Lifecycle (GET) status retrieval endpoint
- Request and response schemas with example payloads
- Conditional validation rules using OpenAPI 3.1 constructs
- Standardized error responses and reusable components
- Canonical Day‑2 Arrangement Confirmation event schema
- Event mesh integration metadata
- Payer, Producer, Address, and Bank reusable schemas

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
