# Initial Baseline Use Case vs. Advanced Implementation

[![Pega Platform](https://img.shields.io/badge/Platform-Pega%20Community%20Edition-001F5F.svg)](https://community.pega.com/)
[![Reference Doc](https://img.shields.io/badge/Baseline-Student%20Registration%20to%20UAP--R.pdf-red.svg)](./Student%20Registration%20to%20UAP-R.pdf)

[← Back to Main Repository](../../README.md)

This document provides a detailed, technical comparison between the initial baseline training requirement specification and the advanced, 7-stage enterprise application actually implemented in Pega Community Edition (`TSPP`).

---

## Baseline Document Overview

* **Document File:** [📄 `Student Registration to UAP-R.pdf`](./Student%20Registration%20to%20UAP-R.pdf)
* **Use Case ID:** `UC-21` (Version 2.0)
* **Original Platform Target:** Pega 8.4
* **Original Target Completion:** 6 Hours
* **Original Scope:** A rudimentary, single-screen student registration form for TSP college with cascading Country $\rightarrow$ State $\rightarrow$ City dropdowns, ZIP code lookup, form submission, email confirmation, and session logout.

---

## Detailed Comparison Table

| Functional Area | Initial Baseline Use Case (`UC-21`) | Advanced Implementation in Pega (`TSPP`) | Architectural Evolution |
| :--- | :--- | :--- | :--- |
| **Case Lifecycle** | Single-form collection with email confirmation step. | Full 7-stage enterprise lifecycle (`CaseCreation`, `EligibilityCheck`, `StudentRegistration`, `InstituteApproval`, `EligibilityTest`, `EnrollmentPayment`, `RegistrationConfirmation`). | **Significantly Expanded:** Re-architected from a flat form into an enterprise multi-stage lifecycle. |
| **Eligibility Gate** | Conceptual precondition statement. | Dedicated `EligibilityCheck` stage validating affiliation, semester (3rd/4th year), and active backlogs upfront. | **Implemented & Automated:** Front-line gatekeeping halts ineligible applicants before registration. |
| **Address Cascading** | Country $\rightarrow$ State $\rightarrow$ City dropdowns; ZIP code lookup. | Dynamic cascading dropdowns populated from data tables; auto-populates Location and Landmark on ZIP tab-out. | **Fully Implemented:** Integrated with `TSPO-FW-TSPP-Data-LocationAndLandmark`. |
| **Educational Details** | Basic percentages input (SSC, Intermediate, B.Tech). | Institution and semester tracking, percentage validation, and document upload attachments for certificates/memos. | **Enhanced with File Uploads:** Supports binary multi-file attachments linked to the work object. |
| **Course Selection** | Not specified in original use case. | Dynamic course selection (e.g., *Certified Pega Senior System Architect*) with fee (₹17,500) and duration (3 Months). | **Custom Enhancement:** Dynamic data calculation model. |
| **Approvals & Routing** | No approval stage defined. | Dedicated `InstituteApproval` stage dynamically routed to the Academic Officer of the student's institute (e.g., `AO@ACE`). | **Custom Enhancement:** Dynamic routing based on college data tables. |
| **SLA Enforcement** | Not specified in original use case. | 2-Day Service Level Agreement with automated escalation/rejection if the institute fails to review. | **Custom Enhancement:** Enforces business governance and deadline handling. |
| **Assessment / Exam** | Not specified in original use case. | Sub-case `EligibilityTest` (`E-`) with 5 low-code questions, automated scoring ($\ge 30/50$ pass mark), and parent case update. | **Custom Enhancement:** Decoupled parent/child case architecture with Wait shape synchronization. |
| **Payment Processing** | Not specified in original use case. | `EnrollmentPayment` stage supporting UPI and Card payments with input validation and savable data pages. | **Custom Enhancement:** Multi-mode payment transaction handling. |
| **Case ID & Payment ID** | Generic default ID. | Standardized compound identifier combining Case ID and Roll Number (`S-XXXX-STU-XXXXX`). | **Custom Enhancement:** Traceable compound key architecture. |
| **Modified Logout Screen** | Specified in UC-21 flow (Step 10). | Not implemented; standard portal behavior preserved. | **Omitted / Out of Scope:** Kept standard Pega session termination. |
| **Email Domain Filtering** | Planned in notes (blocking `@gmail.com`). | Not enforced in the final validation rules (allows standard email domains). | **Relaxed for Testing:** Avoided blocking valid testing emails during demonstration. |

---

## Alternate Stages (Exception Flows)

Unlike the baseline specification, the actual application implements **3 automated Alternate Stages** in Pega:

1. **Rejection due to Ineligibility (`Rejection due InEligibility`):**
   * Triggered in Stage 2 if the student has active backlogs or is not in 3rd/4th year.
   * Directly sets status to `Not Eligible` and halts case progression.
2. **Rejection by Institution (`Rejection by Instution`):**
   * Triggered in Stage 4 if the Academic Officer rejects the application or if the 2-day SLA deadline expires.
   * Dispatches automated rejection correspondence.
3. **Failure Handling (`Failure Handling`):**
   * Triggered in Stage 5 if the student scores below the 30-point threshold on the entrance assessment.
   * Dispatches ineligibility notice and restricts program access.

---

[← Back to Main Repository](../../README.md)
