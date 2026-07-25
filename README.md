
![Hero Section](image_9c7b40.png)# Enterprise Procure-to-Pay (P2P) Lifecycle

**Author:** Ashish Augustine  
**Version:** 1.1.0  

## Overview
This repository contains the standard operating procedure and workflow documentation for the Enterprise Procure-to-Pay (P2P) process. The documentation is designed to align cross-functional teams—specifically Production & Warehouse, Procurement, and Finance—by providing a clear, visual, and text-based roadmap of purchasing operations and system integrations.

## Workflow Diagram
The following swimlane diagram maps the end-to-end lifecycle from material shortage identification through to the final payment run.

![Enterprise P2P Swimlane Diagram](https://github.com/user-attachments/assets/3a5ebe22-bcfc-42dc-b84e-8d250d43da8d)
*(Diagram authored in Lucidchart. See the `/source` folder for the raw `.vsdx` export).*

---

## Process Breakdown
To ensure accessibility and provide a quick-reference guide, the visual workflow is translated into the following step-by-step procedural breakdown, categorized by departmental responsibility.

### 1. Production & Warehouse
The lifecycle is initiated and ultimately validated on the warehouse floor.
*   **Identify Shortage:** The process begins when a physical or system-flagged **Material Shortage** is identified.
*   **Requisition:** The system or user **Generates a Purchase Requisition (PR)**, which is automatically routed to the Procurement department.
*   **Receiving:** Once the vendor fulfills the order, the warehouse team **Receives Goods (GRN)**. This receipt triggers the final financial reconciliation phase.

### 2. Procurement
Procurement acts as the gateway for vendor relations and budget compliance.
*   **Review:** The procurement team conducts a **Review of the Purchase Requisition**.
*   **Approval Gate:** 
    *   **If Approved:** The PR is **Converted to a Purchase Order (PO)** and subsequently **Transmitted to the Vendor**.
    *   **If Rejected:** The workflow terminates at the **PR Rejected** state. *(See Technical Integrations below for automated system actions during rejection).*

### 3. Finance
Finance ensures that all documentation aligns before releasing company funds.
*   **Invoicing:** Finance **Receives the Vendor Invoice** upon goods delivery.
*   **Verification:** A **3-Way Match** is performed (comparing the PO, the GRN, and the Invoice).
*   **Reconciliation Gate:**
    *   **Match Successful:** Finance **Executes the Payment Run**, resulting in the **Payment Issued** state.
    *   **Match Unsuccessful:** The discrepancy is routed to **Exception Handling** for manual review and correction before returning to the matching queue.

---

## System Integrations: Azure DevOps Trigger

To maintain strict audit trails and minimize workflow bottlenecks, this P2P process features an automated pipeline integration with our project tracking infrastructure.

**Automated Work Item Creation**
As highlighted in the diagram, if a Purchase Requisition hits the **PR Rejected** state, a webhook automatically triggers and **Converts the rejection into an Azure DevOps work item**. 

*   **Trigger:** Rejection status logged in the procurement ERP.
*   **Action:** An API call (via Azure Logic Apps/Azure Functions) generates a task in the active Azure Boards backlog.
*   **Purpose:** This ensures that procurement blockers are instantly visible to the engineering and operations teams, allowing for rapid remediation of missing requirements, budget approvals, or vendor compliance issues without requiring manual email follow-ups.

---

## Repository Structure

```text
├── docs/
│   └── p2p_system_architecture.md   # Detailed API webhook documentation
├── images/
│   └── image_9deb23.png             # Master swimlane diagram (PNG)
├── source/
│   └── p2p_workflow_master.json     # Raw Lucidchart data file
└── README.md                        # This document
