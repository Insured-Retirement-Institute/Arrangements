# IRI Investment Arrangements API

## Overview

This repository consolidates investment arrangement transactions for annuity products under the **IRI Digital First vision**.

Supported arrangement families include:

- Allocation Instructions (AI)
- Dollar Cost Average (DCA)
- Auto Rebalance (AR)
- Systematic Investment (SI)
- Index Performance Lock (IPL)

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
- Asynchronous Day‑1 / Day‑2 processing model with lifecycle tracking.
- Event-driven Day‑2 confirmation notifications delivered through Advanced Event Mesh.

---

## Supported Transactions

### 1. Allocation Instructions (AI)

#### Submit Allocation Instructions Arrangement

Handles creation of a new Allocation Instructions arrangement.

**Endpoint:** `/v1/policies/{policyNumber}/arrangements/allocation-instructions`

**Folder:** `./SubmitAllocationInstructions/`

##### User Stories

- As a Policy Administrator, I want to create Allocation Instructions so that future allocations follow the selected investment strategy.
- As a Financial Advisor, I want to establish allocation instructions for a client so future premiums and transfers are distributed according to the desired investment allocation.
- As a System Integrator, I want to submit Allocation Instructions arrangements and receive lifecycle updates so enterprise systems remain synchronized.

##### Personas

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

#### Update Allocation Instructions Arrangement

Handles scenarios where an existing Allocation Instructions arrangement is updated.

**Endpoint:** `/v1/policies/{policyNumber}/arrangements/allocation-instructions/{arrangementId}`

**Folder:** `./UpdateAllocationInstructions/`

##### User Stories

- As a Policy Administrator, I want to update an Allocation Instructions arrangement so that future allocations follow the revised investment strategy.
- As a System Integrator, I want to synchronize Allocation Instructions updates across enterprise platforms so that investment allocation data remains consistent.
- As a Financial Advisor, I want to modify investment allocation instructions so that future deposits and transfers align with client objectives.

##### Personas

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

### 2. Dollar Cost Average (DCA)

#### Submit (Create) DCA Arrangement

Handles scenarios where a new Dollar Cost Average arrangement is established for a policy, enabling systematic and recurring fund transfers.

**Endpoint:** `/v1/policies/{policyNumber}/arrangements/dollar-cost-average`

**Folder:** `./submitdca/`

##### User Stories

- As a Policy Administrator, I want to validate all required fields before submitting a DCA arrangement so that I can avoid processing delays.
- As a System Integrator, I want to map the arrangement schema to our internal data model so that we can automate recurring transfer setup.
- As a Financial Advisor, I want to configure transfer amount, frequency, and fund allocation so that I can align with the client’s investment strategy.

##### Personas

**Policy Administrator**  
Goals: Ensure accurate and timely setup of DCA arrangements.  
Needs: Clear schema definitions, validation rules, and lifecycle visibility.  
Pain Points: Manual setup errors, missing required fields, inconsistent formats.

**System Integrator**  
Goals: Implement DCA submission workflows in backend systems and APIs.  
Needs: JSON schema formats, sample payloads, and validation guidance.  
Pain Points: Ambiguous field definitions, conditional rule complexity, integration inconsistencies.

**Financial Advisor**  
Goals: Configure systematic investment strategies correctly.  
Needs: Clear understanding of transfer rules, frequency, and allocation structures.  
Pain Points: Complex configuration logic, lack of standardization, manual processes.

#### Update / Override / Cancel DCA Arrangement

Handles scenarios where an existing DCA arrangement is modified, overridden, or corrected using an arrangementId.

**Endpoint:** `/v1/policies/{policyNumber}/arrangements/dollar-cost-average/{arrangementId}`

**Folder:** `./updatedca/`

##### User Stories

- As a Policy Administrator, I want to update an existing arrangement so that I can correct or adjust transfer details.
- As a System Integrator, I want to synchronize updated arrangement data across systems so that lifecycle management remains consistent.
- As a Financial Advisor, I want to adjust allocation strategies or transfer frequency so that they reflect changing client needs.

##### Personas

**Policy Administrator**  
Goals: Maintain accurate and up-to-date DCA arrangements.  
Needs: Clear validation rules, update constraints, and audit traceability.  
Pain Points: Inconsistent updates, missing identifiers, lack of visibility into lifecycle changes.

**System Integrator**  
Goals: Synchronize arrangement updates efficiently across systems.  
Needs: Reliable update endpoints and consistent schema behavior.  
Pain Points: Versioning challenges, conditional validation complexity, handling partial updates.

**Financial Advisor**  
Goals: Adjust investment strategies dynamically based on client needs.  
Needs: Flexible update mechanisms and clarity on the impact of changes.  
Pain Points: Complex override logic, unclear constraints when modifying arrangements.

---

### 3. Auto Rebalance (AR)

#### Submit (Create) Auto Rebalance Arrangement

Handles scenarios where a new Auto Rebalance arrangement is established for a policy, enabling periodic reallocation of contract values to target investment allocations.

**Endpoint:** `/v1/policies/{policyNumber}/arrangements/auto-rebalance`

**Folder:** `./submitautorebalance/`

##### User Stories

- As a Policy Administrator, I want to validate all required fields before submitting an Auto Rebalance arrangement so that I can avoid processing delays.
- As a System Integrator, I want to map the arrangement schema to our internal data model so that we can automate rebalance setup and maintenance.
- As a Financial Advisor, I want to configure rebalance frequency and target fund allocations so that client investment allocations remain aligned with long-term objectives.

##### Personas

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

#### Update / Override / Cancel Auto Rebalance Arrangement

Handles scenarios where an existing Auto Rebalance arrangement is modified, overridden, corrected, or cancelled using an arrangementId.

**Endpoint:** `/v1/policies/{policyNumber}/arrangements/auto-rebalance/{arrangementId}`

**Folder:** `./updateautorebalance/`

##### User Stories

- As a Policy Administrator, I want to update an existing arrangement so that I can correct or adjust rebalance details.
- As a System Integrator, I want to synchronize updated arrangement data across systems so that lifecycle management remains consistent.
- As a Financial Advisor, I want to adjust allocation strategies or transfer frequency so that they reflect changing client needs.

##### Personas

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

### 4. Systematic Investment (SI)

#### Submit (Create) Systematic Investment Arrangement

Handles scenarios where a new Systematic Investment arrangement is established for a policy, enabling recurring investment of premiums or contributions according to predefined allocation instructions.

**Endpoint:** `/v1/policies/{policyNumber}/arrangements/systematic-investment`

**Folder:** `./submitsystematicinvestment/`

##### User Stories

- As a Policy Administrator, I want to validate all required fields before submitting a Systematic Investment arrangement so that I can avoid processing delays.
- As a System Integrator, I want to map the arrangement schema to our internal data model so that we can automate Systematic Investment setup and maintenance.
- As a Financial Advisor, I want to establish recurring investment allocations and funding instructions so that policy owners can systematically invest according to their financial goals.

##### Personas

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

#### Update Systematic Investment Arrangement

Handles scenarios where an existing Systematic Investment arrangement is modified, overridden, corrected, or cancelled using an arrangementId.

**Endpoint:** `/v1/policies/{policyNumber}/arrangements/systematic-investment/{arrangementId}`

**Folder:** `./updatesystematicinvestment/`

##### User Stories

- As a Policy Administrator, I want to update an existing Systematic Investment arrangement so that I can correct or modify investment details.
- As a System Integrator, I want to synchronize arrangement updates across systems so that investment instructions remain consistent throughout the arrangement lifecycle.
- As a Financial Advisor, I want to adjust contribution allocations, frequencies, or funding information so that the arrangement reflects changing client needs.

##### Personas

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

### 5. Index Performance Lock (IPL)

#### Submit (Create) Index Performance Lock Arrangement

Handles scenarios where a new Index Performance Lock arrangement is established for a policy, enabling policy owners to lock gains or protect values associated with indexed investment options according to predefined threshold criteria.

**Endpoint:** `/v1/policies/{policyNumber}/arrangements/index-performance-lock`

**Folder:** `./submitindexperformancelock/`

##### User Stories

- As a Policy Administrator, I want to validate all required fields before submitting an Index Performance Lock arrangement so that I can avoid processing delays and rejected requests.
- As a System Integrator, I want to map the Index Performance Lock arrangement schema to our internal data model so that we can automate arrangement creation and lifecycle tracking.
- As a Financial Advisor, I want to establish upside, downside, or on-demand performance lock arrangements so that clients can manage indexed account performance according to their investment goals.

##### Personas

**Policy Administrator**  
Goals: Ensure accurate and timely setup of Index Performance Lock arrangements.  
Needs: Clear schema definitions, validation rules, threshold requirements, and lifecycle visibility.  
Pain Points: Missing required fields, incorrect arrangement configuration, and validation failures.

**System Integrator**  
Goals: Implement Index Performance Lock submission workflows within enterprise systems.  
Needs: JSON schema definitions, example payloads, lifecycle events, and validation guidance.  
Pain Points: Conditional validation complexity, asynchronous processing, and integration consistency.

**Financial Advisor**  
Goals: Configure performance lock strategies that align with client objectives and risk management preferences.  
Needs: Visibility into performance lock options, threshold configurations, and destination fund selections.  
Pain Points: Understanding arrangement subtype rules, threshold requirements, and downstream processing behavior.

#### Update Index Performance Lock Arrangement

Handles scenarios where an existing Index Performance Lock arrangement is modified, updated, overridden, or cancelled using an arrangementId.

**Endpoint:** `/v1/policies/{policyNumber}/arrangements/index-performance-lock/{arrangementId}`

**Folder:** `./updateindexperformancelock/`

##### User Stories

- As a Policy Administrator, I want to update an existing Index Performance Lock arrangement so that I can correct or modify arrangement details.
- As a System Integrator, I want to synchronize arrangement updates across connected systems so that arrangement lifecycle data remains consistent.
- As a Financial Advisor, I want to adjust threshold values, destination funds, or performance lock configurations so that the arrangement reflects changing client needs and market conditions.

##### Personas

**Policy Administrator**  
Goals: Maintain accurate and current Index Performance Lock arrangements.  
Needs: Clear validation rules, update constraints, audit traceability, and lifecycle visibility.  
Pain Points: Missing arrangement identifiers, inconsistent updates, and limited visibility into processing status.

**System Integrator**  
Goals: Synchronize arrangement updates efficiently across enterprise applications.  
Needs: Reliable update endpoints, lifecycle status tracking, and event-driven confirmations.  
Pain Points: Conditional validation rules, asynchronous processing, and maintaining data consistency across platforms.

**Financial Advisor**  
Goals: Modify performance lock strategies as investment objectives and market conditions evolve.  
Needs: Flexibility in threshold configuration, arrangement management capabilities, and transparency into processing outcomes.  
Pain Points: Complex update requirements, uncertainty regarding downstream impacts, and arrangement lifecycle management.

---

## Schema Overview

Common schema categories across arrangement families include:

- Root Attributes
- Arrangement Details
- Fund Allocation Details
- Transfer Configuration
- Audit Information
- Party / Payer Information
- Producer Information
- Validation Rules

Detailed schemas for each arrangement family are available in their respective folders:

- ./AllocationInstructions/
- ./DollarCostAverage/
- ./AutoRebalance/
- ./SystematicInvestment/
- ./IndexPerformanceLock/
---

## Response Schema Overview

All API operations—across synchronous and asynchronous processing models—adhere to a unified **Error schema**. This ensures consistent error handling, predictable integration behavior, and standardized troubleshooting across all arrangement transactions.

### Success Response Expectations

#### Submit (POST) Response

- Return **HTTP 201 Created**
- Include a Location header pointing to `/v1/policies/{policyNumber}/arrangement-requests/{requestId}`
- Return a transaction-specific response object containing `requestId` and `status`
- A GET endpoint is available to retrieve lifecycle status using **HTTP 200**

#### Update (PUT) Response

- Return **HTTP 202 Accepted**
- Include a Location header pointing to `/v1/policies/{policyNumber}/arrangement-requests/{requestId}`
- Return a transaction-specific response object containing `requestId` and `status`
- A GET endpoint is available to retrieve lifecycle status using **HTTP 200**

### Lifecycle Status (GET Response)

A GET endpoint is available to retrieve the current status of the arrangement request.

- Return **HTTP 200**
- Response includes `requestId`, `status`, `effectiveDate`, and `message`

#### Status Values

| Status | Description |
|----------|----------|
| IN_PROGRESS | Request is being processed asynchronously |
| SUCCESS | Transaction completed successfully |
| REJECTED | Transaction failed validation or processing |

---

## Standard Error Schema

Every error response—regardless of transaction type—includes:

- An HTTP status code in the **400–599** range
- A structured and validated **error code**
- A **timestamp** of when the error was generated
- A developer-focused **technical message** (`message`)
- A **correlationId** for cross-system tracing
- A **field-level** or **rule-level** error collection

### Key Fields

| Field | Description |
|----------|----------|
| **httpStatus** | Numeric HTTP status code (400–599) representing the type and severity of the failure. |
| **code** | Structured identifier in the enforced format:|
| **correlationId** | Returned as a response header on all responses, including errors. |
| **message** | End‑user‑friendly explanation, safe to show in portals or consumer-facing applications. |
| **validationErrors** | Array describing domain/business rule violations. |

### Purpose & Benefits

This standardized error structure ensures:

- A predictable experience across all APIs.
- Clear differentiation between developer diagnostics and user-safe messages.
- Enhanced traceability.
- Support for granular validation feedback.
- Easier monitoring, logging, and cross-system troubleshooting.

---

## Day‑2 Asynchronous Processing

Arrangement transactions use an asynchronous processing model after initial request submission.

Upon successful validation:

- POST (Submit) returns HTTP 201 (Created) and assigns a unique `requestId`.
- PUT (Update) returns HTTP 202 (Accepted) and assigns a unique `requestId`.

These responses confirm acceptance of the request but do not indicate final transaction completion.

Final processing occurs asynchronously in downstream systems.

### Delivery Model

Day‑2 confirmation events are published to the enterprise event mesh (Advanced Event Mesh).

Consumers receive confirmations through topic-based subscriptions.

Day‑2 confirmations are event-driven only.

### Day‑2 Confirmation

Final transaction outcomes are communicated via a Day‑2 arrangement confirmation event.

Supported outcomes include:

- SUCCESS
- SUCCESS_WITH_INFO
- FAILURE

### Day‑2 Schema

The Day‑2 arrangement confirmation schema includes:

- Event metadata (eventId, eventTimestamp, eventType)
- Transaction identifiers (requestId, policyNumber)
- Processing outcome (status, message)
- Execution details (transactionType, execution date and time)

Supported transactionType values include:

#### Allocation Instructions

- SUBMIT_ALLOCATION_INSTRUCTIONS
- UPDATE_ALLOCATION_INSTRUCTIONS
- CANCEL_ALLOCATION_INSTRUCTIONS

#### Dollar Cost Average

- SUBMIT_DOLLAR_COST_AVERAGE
- UPDATE_DOLLAR_COST_AVERAGE
- CANCEL_DOLLAR_COST_AVERAGE

#### Auto Rebalance

- SUBMIT_AUTO_REBALANCE
- UPDATE_AUTO_REBALANCE
- CANCEL_AUTO_REBALANCE

#### Systematic Investment

- SUBMIT_SYSTEMATIC_INVESTMENT
- UPDATE_SYSTEMATIC_INVESTMENT
- CANCEL_SYSTEMATIC_INVESTMENT

#### Index Performance Lock

- SUBMIT_INDEX_PERFORMANCE_LOCK
- UPDATE_INDEX_PERFORMANCE_LOCK
- CANCEL_INDEX_PERFORMANCE_LOCK

### Status Visibility

A GET lifecycle endpoint is available to retrieve the current processing status using the requestId.

The GET endpoint provides operational visibility and audit support.

The GET endpoint does not replace Day‑2 event delivery as the source of final confirmation.

---

## OpenAPI Specs

Unified Swagger documentation for all arrangement endpoints is available in the `openapi-specs/` folder.

The specifications include:

- Submit (POST) arrangement endpoint definitions
- Update (PUT) arrangement endpoint definitions
- Lifecycle (GET) status retrieval endpoints
- Request and response schemas with example payloads
- Conditional validation rules using OpenAPI 3.1 constructs
- Standardized error responses and reusable components
- Day‑2 confirmation event schemas

These OpenAPI definitions are designed to support consistent integration, validation, and implementation across carriers, distributors, and solution providers.

---

## Versioning

- Follow semantic versioning for spec updates.
- Document changes in commit messages and changelogs to support integrator adoption.

---

## Code of Conduct

Please review and adhere to the **Code of Conduct** and **Style Guide** provided in the repository to ensure consistency and professionalism.

---

## How to Contribute

- Fork the repository and submit pull requests.
- Report issues via the Issues tab.
- Join working groups: **hpikus@irionline.org**

---

## Business Owners

- **Carrier Business Owner:** digitalfirst@brighthousefinancial.com
- **Distributor Business Owner:** [contact]
- **Solution Provider Business Owner:** [contact]
