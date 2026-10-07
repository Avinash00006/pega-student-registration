# Student Registration Application — Pega Community Edition

[![Pega Platform](https://img.shields.io/badge/Platform-Pega%20Community%20Edition-001F5F.svg)](https://community.pega.com/)
[![Application ID](https://img.shields.io/badge/Application%20ID-TSPP%2001.01.01-blue.svg)]()
[![Case Status](https://img.shields.io/badge/Case%20Status-Resolved--Completed-success.svg)]()
[![Architecture](https://img.shields.io/badge/Architecture-Parent%2FChild%20Case-orange.svg)]()

An enterprise case-management application built on **Pega Community Edition** for the **University Academic Program (UAP)** under TalentSprint / TSP. 

This repository serves as a **portfolio and technical documentation archive** featuring the exported Pega application bundle (`.zip`), live case lifecycle walkthroughs, architectural design, and academic presentation.

---

## Quick Navigation

| Resource | Description | Direct Link |
| :--- | :--- | :---: |
| 📸 **Live Workflow Demo** | Step-by-step visual walkthrough of Case `S-3` (15 Screenshots) | [**View Demo Walkthrough**](docs/demo/README.md) |
| 📐 **Case Lifecycle Architecture** | 7 Primary Stages & 3 Automated Rejection Alternate Stages | [**View Architecture**](#case-lifecycle) |
| 📋 **Baseline vs. Evolution** | Comparison between initial training use-case (`UC-21`) and final app | [**View Comparison**](docs/initial-use-case/README.md) |
| 📦 **Pega Application Package** | Downloadable application bundle (`StudentRegistrationToUAP.zip`) | [**View Package**](application/) |
| 📊 **Project Presentation** | Presentation slide deck for academic jury review (`.pptx`) | [**View Presentation**](presentation/) |

---

## Project Overview

The **Student Registration Application (`TSPP`)** automates the end-to-end onboarding and enrollment process for university students seeking admission into the University Academic Program (UAP). 

Starting from pre-screening eligibility qualification to dynamic institutional review, interactive online testing, course fee payment, and final enrollment confirmation, the application manages the complete student journey using **Pega's Case Management and Workflow Automation** capabilities.

### Technical Metadata

| Attribute | Details |
| :--- | :--- |
| **Application Name** | `TSPP` |
| **Application Version** | `01.01.01` |
| **Organization (Org / Div / Unit)** | `TSPO` / `Div` / `Unit` |
| **Class Structure** | `TSPO-FW-TSPP-Work` |
| **Parent Case Type** | `StudentRegistration` (`TSPO-FW-TSPP-Work-StudentRegistration`, Prefix: `S-`) |
| **Child Case Type** | `EligibilityTest` (`TSPO-FW-TSPP-Work-EligibilityTest`, Prefix: `E-`) |
| **Platform Version** | Pega Community Edition / Platform '24 (Engine: `8.24.52`) |
| **Built-On Application** | Constellation UI Architecture (`01.01`) |

---

## Problem Statement & Solution

* **The Problem:** Partner institutions affiliated with UAP previously handled student registration manually, resulting in delayed eligibility verification, uncoordinated document management, lack of review Service Level Agreements (SLAs), and decoupled pre-enrollment testing.
* **The Solution:** A centralized, low-code Pega application with automated eligibility validation, dynamic assignment routing to college Academic Officers, strict 2-day SLAs, parent/child case assessment scoring, and integrated fee payment via savable data pages.

---

## Case Lifecycle

The application architecture features **7 Primary Stages** and **3 Alternate Stages** configured in the Pega Case Life Cycle Designer:

![Pega Case Lifecycle Designer](docs/case-lifecycle.png)

### Primary Stages
1. **Create (`CaseCreation`):** Initializes the case instance (`Create Case` step).
2. **Eligibility validation (`EligibilityCheck`):** Front-line gatekeeper checking college affiliation, semester (3rd/4th year), and active backlogs.
3. **Registration Form (`StudentRegistration`):** Captures personal info, cascading address with auto ZIP lookup, academic records with file attachments, course selection, summary review, and SLA notice.
4. **Institute Approval (`InstituteApproval`):** Dynamically routed to the Academic Officer (`AO@ACE`), governed by a 2-day SLA deadline.
5. **Conduct Test (`EligibilityTest`):** Synchronized parent/child case execution (`E-1`) administering an interactive 5-question test with automated scoring ($\ge 30/50$ pass mark).
6. **Payment for Enrollment (`EnrollmentPayment`):** Supports UPI / Card modes and triggers `Enrollment Sync Flow` with savable data pages.
7. **Finalize Registration (`RegistrationConfirmation`):** Final success screen and automated onboarding email. Reaches `RESOLVED-COMPLETED`.

### Alternate Stages (Exception & Rejection Paths)
* 🔴 **Rejection due to Ineligibility (`Rejection due InEligibility`):** Invoked if the applicant has active backlogs or an unaffiliated college.
* 🔴 **Rejection by Institution (`Rejection by Instution`):** Invoked if the Academic Officer rejects the application or if the 2-day SLA deadline expires.
* 🔴 **Failure Handling (`Failure Handling`):** Invoked if the applicant scores below the 30-point passing threshold on the entrance test.

---

## Workflow Walkthrough & Demo

The live execution of Case **`S-3`** is documented across 15 high-resolution screenshots with complete stage, status, and technical implementation details:

> 👉 **[Explore the Full 15-Step Visual Demo Walkthrough](docs/demo/README.md)**

### Walkthrough Highlights
* **Step 01–02:** Case initialization & eligibility gatekeeping.
* **Step 03–07:** Cascading address lookups, certificate file uploads, course pricing, review, and SLA notice.
* **Step 08–09:** Dynamic work queue assignment and Academic Officer decision portal (`AO@ACE`).
* **Step 10–12:** Parent/child case synchronization (`E-1`), online assessment, and automated scoring (*50/50*).
* **Step 13–15:** Fee payment via savable data page, confirmation notification, and resolved case lifecycle.

---

## Initial Baseline vs. Advanced Implementation

The project evolved from an introductory 6-hour baseline use case (`UC-21`) into an advanced enterprise system:
* **Baseline Reference Document:** [📄 `Student Registration to UAP-R.pdf`](docs/initial-use-case/Student%20Registration%20to%20UAP-R.pdf)
* **Detailed Technical Comparison:** 👉 [**View Full Baseline vs. Implementation Comparison Table**](docs/initial-use-case/README.md)

---

## Pega Application Package & Import Guide

The downloadable Pega application bundle is archived under:
```text
application/StudentRegistrationToUAP.zip
```

### Quick Import Steps:
1. Log in to **Pega Community Edition** or Pega Platform as Administrator.
2. In Dev Studio, go to **Configure $\rightarrow$ Application $\rightarrow$ Distribution $\rightarrow$ Import**.
3. Upload `application/StudentRegistrationToUAP.zip` and follow the wizard prompts to import schema, tables, and rulesets (`TSPP`, `TSPPInt`, `TSPO`, `TSPOInt`).
4. Switch application to **`TSPP 01.01.01`**.
5. Click **+ Create $\rightarrow$ Student Registration** to run the workflow.

---

## Repository Structure

```
├── README.md                                 # Main portfolio overview & architecture
├── application/
│   └── StudentRegistrationToUAP.zip          # Exported Pega application bundle (download & import)
├── docs/
│   ├── case-lifecycle.png                    # Pega Case Lifecycle Designer diagram
│   ├── demo/                                 # Sequenced workflow screenshots & walkthrough
│   │   ├── README.md                         # Full 15-step visual walkthrough with screenshots
│   │   ├── 01_case_creation_modal.png
│   │   └── ... (02 to 15 screenshots)
│   └── initial-use-case/                     # Baseline training use-case comparison
│       ├── README.md                         # Detailed baseline vs. implementation comparison
│       └── Student Registration to UAP-R.pdf
└── presentation/
    └── Student Registration PPT.pptx         # Academic presentation deck
```

---

## Project Presentation

The slide deck presented for academic review is available in the repository:
* 📄 **Presentation File:** [**`presentation/Student Registration PPT.pptx`**](presentation/Student%20Registration%20PPT.pptx)

---

## Limitations & Scope Boundaries

* **Modified Logout Screen:** Omitted; standard Pega session termination is preserved.
* **Email Domain Filtering:** Relaxation applied to allow public email providers (`@gmail.com`) for testing.
* **Payment Gateway API:** Processes transactions via internal savable data pages rather than external third-party payment gateways.

---

## Academic Context & Credits

Developed as an academic capstone application for the **Pega University Academic Program (UAP)** under **TalentSprint / TSP**.

* **Application Architect & Implementation:** Built and configured in Pega Community Edition by **Koneti Sairam Avinash**.
* **Presentation & Academic Collaboration:** Developed in collaboration with **Team Cosmos** (*Koneti Sairam Avinash, Pakalapati Varshini, Nallani Adharsh*).
