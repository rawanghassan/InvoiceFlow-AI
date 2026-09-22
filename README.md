<div align="center">

# InvoiceFlow AI

### Intelligent Invoice Automation & Financial Operations Workflow

**From PDF invoices to validated, review-ready business data.**

<br>

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![n8n](https://img.shields.io/badge/n8n-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Telegram](https://img.shields.io/badge/Telegram-2AABEE?style=for-the-badge&logo=telegram&logoColor=white)

<br>

**Automation · Validation · Duplicate Detection · Anomaly Monitoring · Alerts · Dashboards**

</div>

---

> **Public portfolio repository — implementation intentionally withheld.**  
> This repository presents the architecture, product design, workflow logic, validation strategy, testing approach, and project documentation without exposing the proprietary production implementation.

---

## Overview

**InvoiceFlow AI** is an automated invoice-processing and financial-operations MVP designed to transform PDF invoices into structured, validated, and actionable business records.

Instead of manually reviewing every invoice, the workflow coordinates invoice extraction, financial validation, duplicate detection, overdue monitoring, supplier anomaly checks, review alerts, ledger updates, and operational reporting.

The project demonstrates how **Python, workflow automation, data validation, business intelligence, and API-based integrations** can be combined into one connected operational system.

---

## Product Concept

![InvoiceFlow AI](assets/invoiceflow_hero_concept.png)

> *Conceptual product visualization for presentation purposes. It is not a literal application screenshot.*

InvoiceFlow AI was designed around one core idea:

### Turn invoice processing from a manual task into an automated decision workflow.

The system moves invoices through a structured operational pipeline:

**PDF → Extraction → Validation → Risk Checks → Review Decision → Ledger → Dashboard**

---

## Key Capabilities

| Capability | What It Does |
|---|---|
| **PDF Invoice Processing** | Accepts invoice PDFs and extracts structured invoice information |
| **Invoice Field Parsing** | Identifies invoice number, supplier, dates, totals, VAT, currency, category, and payment status |
| **Financial Validation** | Checks required fields, arithmetic consistency, VAT rules, dates, and financial values |
| **Duplicate Detection** | Identifies invoices already processed using invoice and supplier matching |
| **Overdue Monitoring** | Detects open invoices that have passed their due date |
| **Supplier Anomaly Detection** | Flags unusual supplier amounts and unexpected category changes |
| **Automated Review Routing** | Determines whether an invoice requires human review |
| **Telegram Alerts** | Sends review and duplicate notifications automatically |
| **Ledger Integration** | Stores validated records in the operational Master Ledger |
| **Dashboard API** | Exposes ledger data for live dashboard reporting |
| **Business Intelligence** | Supports financial monitoring and operational analysis through dashboards |

---

## System Architecture

![InvoiceFlow AI Workflow](assets/invoiceflow_workflow_concept.png)

> *Conceptual workflow visualization. The private production workflow contains the implementation details and node logic.*

### High-Level Processing Flow

```text
PDF Invoice
    │
    ▼
Invoice Upload
    │
    ▼
Extract PDF Text
    │
    ▼
Parse Invoice Fields
    │
    ▼
Validate Invoice
    │
    ▼
Check Overdue Status
    │
    ├───────────────► Duplicate Check
    │                     │
    │                     ├── Duplicate → Alert + Block Ledger Insert
    │                     │
    │                     └── New Invoice
    │
    ▼
Get Supplier History
    │
    ▼
Detect Supplier Anomalies
    │
    ▼
Needs Review?
   / \
 Yes  No
  │    │
  ▼    │
Review │
Alert  │
  \    /
   ▼  ▼
Insert into Master Ledger
        │
        ▼
Dashboard Data API
        │
        ▼
Operational Dashboard
