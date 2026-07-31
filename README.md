# IRI Investment Arrangement Management APIs

## Overview

The IRI Investment Arrangement Management APIs define industry-standard RESTful interfaces for establishing, maintaining, updating recurring investment-related arrangements on in-force annuity policies.

These APIs support the industry's transition from legacy XML/SOAP integrations to modern RESTful APIs utilizing JSON payloads and OAuth-based authentication under the IRI Digital First Architecture (DFA) initiative. The objective is to provide a consistent, interoperable, and scalable framework that supports arrangement lifecycle management across carriers, distributors, and solution providers.

This consolidated specification includes the following arrangement families:

- Dollar Cost Average (DCA)
- Auto Rebalance (AR)
- Systematic Investment (SI)
- Allocation Instructions (AI)

## Business Case

### Problem Statement

- Legacy XML/SOAP systems are inefficient, lack modern security capabilities, and are approaching end-of-life.
- Firms require secure, real-time, standards-based arrangement servicing capabilities.
- Manual maintenance of investment arrangements increases operational risk and processing delays.
- Inconsistent carrier implementations increase integration complexity and maintenance costs.
- The industry requires a common framework for arrangement setup, updates tracking.

### Objectives

- Transition investment arrangement processing to RESTful APIs.
- Improve security, scalability, and interoperability.
- Reduce payload size and processing overhead.
- Accelerate integration and implementation efforts.
- Standardize arrangement lifecycle management across the industry.
- Promote industry-wide alignment through a common Data Dictionary and standardized OpenAPI specifications.
- Support long-term adoption of IRI Digital First Architecture standards.

### Key Features

- OpenAPI 3.1.x specifications with examples.
- Standardized JSON schemas.
- Reusable schema components.
- Standardized error handling.
- Conditional validation rules.
- Consistent Day-2 processing model.
- Event-driven lifecycle confirmations.
- Data Dictionary alignment.
- Unified arrangement lifecycle patterns.

## Supported Transactions

### 1. Dollar Cost Average (DCA)

#### Supported Transactions
- Submit DCA Arrangement
- Update DCA Arrangement

### 2. Auto Rebalance (AR)

#### Supported Transactions
- Submit Auto Rebalance Arrangement
- Update Auto Rebalance Arrangement

### 3. Systematic Investment (SI)

#### Supported Transactions
- Submit Systematic Investment Arrangement
- Update Systematic Investment Arrangement

### 4. Allocation Instructions (AI)

#### Supported Transactions
- Update Allocation Instructions Arrangement


## Personas and User Stories

### Policy Administrator
Responsibilities, user stories, lifecycle monitoring, validation and arrangement maintenance.

### System Integrator
Responsibilities include integration, synchronization, lifecycle processing and event consumption.

### Financial Advisor
Responsibilities include configuring investment strategies, allocations and recurring investment instructions.

## High-Level Schema Concepts

### Root Attributes
- effectiveDate
- externalArrangementId
- arrangementId
- nsccParticipantId
- cusip
- actionIndicator
- associatedFirmId

### Arrangement Details
- arrangementType
- arrangementSubType
- frequency
- startDate
- endDate
- nextTransactionDate
- productCode

### Funds and Segments
- transferFromFunds
- transferToFunds
- fund allocations
- segment allocations
- target percentages

### Producer Information
- producerNumber
- npn
- crdNumber

## Response Model Overview

### Submit Transactions
HTTP 201 Created

### Update Transactions
HTTP 202 Accepted

### Lifecycle Status Values
- IN_PROGRESS
- SUCCESS
- REJECTED

---

# Day‑2 Asynchronous Processing
Arrangement transactions use an asynchronous processing model after initial request submission.
Upon successful validation, the API returns HTTP 201 (Created) and assigns a unique requestId. This response confirms acceptance of the request but does not indicate final transaction completion.
Final processing occurs asynchronously in downstream systems.

## Delivery Model
Day‑2 confirmation events are published to the enterprise event mesh (SAP Advanced Event Mesh / Solace).
Consumers receive confirmations through topic‑based subscriptions.
Day‑2 confirmations are event‑driven only.

## Day‑2 Confirmation
Final transaction outcomes are communicated via a Day‑2 withdrawal confirmation event.
The event represents a terminal state of the transaction. Each event includes the original requestId for correlation and traceability. Supported outcomes include:
- SUCCESS
- SUCCESS_WITH_INFO
- FAILURE

## Day‑2 Schema
Day‑2 confirmation events conform to a canonical Day‑2 Withdrawal Confirmation schema.
The schema defines the standardized event structure, including:
- Event metadata (eventId, eventTimestamp, eventType)
- Transaction identifiers (requestId, policyNumber)
- Processing outcome (status, message)
- Execution details (transactionType, execution date and time)

## Status Visibility
A GET lifecycle endpoint is available to retrieve the current processing status using the requestId.
The GET endpoint provides operational visibility and audit support.
The GET endpoint does not replace Day‑2 event delivery as the source of final confirmation.

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

## OpenAPI Specs

Contains submit, update, lifecycle, schema and event specifications.

## Change Submissions and Reporting Issues

Issues may be reported through repository issue tracking.

## Versioning

Follow semantic versioning.

## Code of Conduct

Follow repository Code of Conduct and Style Guide.

## How to Contribute

Working Group Contact: hpikus@irionline.org

## Business Owners

- Carrier Business Owner: digitalfirst@brighthousefinancial.com
- Distributor Business Owner: [contact]
- Solution Provider Business Owner: [contact]
