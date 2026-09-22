<div align="center">

# InvoiceFlow AI

### Intelligent Invoice Automation & Financial Operations Workflow

**From PDF invoices to structured, validated, review-ready business data.**

<br>

![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=for-the-badge&logo=python&logoColor=white)
![n8n](https://img.shields.io/badge/n8n-Cloud-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-Operational_UI-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-Reporting-F2C811?style=for-the-badge&logo=powerbi&logoColor=111111)
![Telegram](https://img.shields.io/badge/Telegram-Alerts-2AABEE?style=for-the-badge&logo=telegram&logoColor=white)

<br>

**Invoice Parsing · Validation · Duplicate Detection · Overdue Monitoring · Supplier Anomalies · Alerts · Dashboard API**

</div>

---

> **Public portfolio repository — implementation intentionally withheld.**
>
> This repository demonstrates the product architecture, automation approach, validation strategy, interface concept, testing methodology, and project outcomes while keeping the proprietary production implementation private.

---

## Overview

**InvoiceFlow AI** is an automated invoice-processing and financial-operations MVP designed to transform PDF invoices into structured, validated, review-ready records and operational insights.

Instead of manually reviewing and transferring invoice information across spreadsheets and disconnected tools, InvoiceFlow AI connects the full invoice journey into one automated workflow.

The system can:

- extract invoice information from PDF files,
- validate financial and operational fields,
- detect duplicate invoices,
- identify overdue invoices,
- analyze supplier-level anomalies,
- route invoices requiring human review,
- send automated review alerts,
- store operational records in a Master Ledger,
- and expose ledger data to dashboards through an API.

The goal is simple:

> **Reduce repetitive invoice work while improving data quality, exception visibility, and financial control.**

---

## The Business Problem

Invoice processing can become highly repetitive when teams receive invoices in different formats and manually transfer their information into spreadsheets or financial systems.

Common operational challenges include:

- manual invoice data entry,
- inconsistent invoice formats,
- duplicate invoices,
- incorrect VAT calculations,
- arithmetic errors,
- missing invoice fields,
- overdue payments,
- unusual supplier charges,
- fragmented reporting,
- and delayed exception review.

These issues increase manual effort and make it harder for teams to focus on invoices that actually require attention.

---

## The Solution

InvoiceFlow AI turns invoice processing into a connected decision workflow:

```text
PDF Invoice
      ↓
Extraction
      ↓
Parsing
      ↓
Validation
      ↓
Operational Controls
      ↓
Duplicate / Anomaly Checks
      ↓
Review Decision
      ↓
Ledger
      ↓
Alerts + Dashboard
```

Rather than stopping after data extraction, the system continues through validation, exception handling, storage, and reporting.

---

## Product Concept

![InvoiceFlow AI concept](assets/invoiceflow_hero_concept.png)

> *Conceptual product visualization for presentation purposes. It is not a literal application screenshot.*

The interface is designed around a simple operational experience:

1. Upload an invoice.
2. Let the automation process and validate it.
3. Review exceptions only when necessary.
4. Track processed invoices through the dashboard.

---

## Key Capabilities

| Capability | Description |
|---|---|
| **PDF Invoice Ingestion** | Accepts invoice PDF files for automated processing |
| **Invoice Field Parsing** | Extracts invoice number, supplier, dates, currency, category, totals, VAT, and payment status |
| **Required-Field Validation** | Detects missing operational and financial information |
| **Invoice Number Validation** | Verifies invoice identifier structure before downstream processing |
| **Financial Arithmetic Validation** | Checks whether subtotal + VAT approximately equals total |
| **VAT Validation** | Applies currency-aware VAT validation rules |
| **Date Validation** | Checks invoice dates, due dates, and date consistency |
| **Negative Value Detection** | Flags invalid negative financial values |
| **Overdue Monitoring** | Identifies open invoices that have passed their due date |
| **Duplicate Detection** | Detects invoices already processed using invoice number and supplier |
| **Supplier Amount Anomaly Detection** | Flags invoice totals that are unusually high relative to supplier history |
| **Supplier Category Shift Detection** | Identifies unexpected changes in supplier expense categories |
| **Human Review Routing** | Routes invoices with exceptions to a review workflow |
| **Telegram Alerts** | Sends automated review and duplicate notifications |
| **Master Ledger Storage** | Stores processed invoice records in a central workflow table |
| **Dashboard Data API** | Provides Master Ledger data to the Streamlit dashboard |
| **Operational Dashboard** | Displays invoice KPIs, supplier activity, payment status, exceptions, and review metrics |

---

## Demo Scope

The MVP was developed and tested using a synthetic invoice environment designed to represent realistic invoice-processing scenarios.

| Demo Element | Scope |
|---|---|
| **Synthetic invoices** | 250 |
| **PDF layouts** | 3 different invoice structures |
| **Currencies** | AED, SAR, USD |
| **Invoice outcomes** | Clean, Review Required, Duplicate |
| **Exception scenarios** | Missing fields, VAT issues, arithmetic issues, overdue invoices, duplicates, unusual supplier amounts, category shifts |
| **Primary workflow storage** | n8n Master Ledger |
| **User interface** | Streamlit |
| **BI layer** | Power BI |
| **Alerts** | Telegram |

The public portfolio uses only synthetic/demo information and does not contain real client invoices or production credentials.

---

# System Architecture

## How It Works

![InvoiceFlow AI workflow concept](assets/invoiceflow_workflow_concept.png)

> *Conceptual workflow visualization. The private production workflow contains the implementation details and node logic.*

At a high level, the processing path is:

```text
PDF Invoice
    │
    ▼
Streamlit UI
    │
    ▼
n8n Webhook
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
    ├─────────────────────────────┐
    │                             │
    ▼                             ▼
Check Duplicate Invoice     Check New Invoice
    │                             │
    │ Duplicate                   ▼
    ▼                       Get Supplier History
Flag Duplicate                   │
    │                             ▼
    ├──► Duplicate Alert     Detect Supplier Anomalies
    │                             │
    └──► Duplicate UI             ▼
          Response           Needs Review?
                                  │
                           ┌──────┴──────┐
                           │             │
                          Yes            No
                           │             │
                           ▼             │
                     Review Alert        │
                           │             │
                           └──────┬──────┘
                                  ▼
                         Insert New Invoice
                                  │
                                  ▼
                         Build UI Response
                                  │
                                  ▼
                       Return to Streamlit
```

A separate dashboard path provides reporting data:

```text
Streamlit Dashboard
       │
       ▼
Dashboard Data API
       │
       ▼
Get Master Ledger
       │
       ▼
Build Dashboard Data
       │
       ▼
Return Dashboard Response
```

---

## Invoice Processing Stages

### 1. Invoice Intake

A user uploads a PDF invoice through the Streamlit interface.

The file is sent to the automation workflow through a webhook.

---

### 2. PDF Text Extraction

The workflow extracts the textual content of the invoice.

The parser supports multiple synthetic invoice layouts rather than depending on only one fixed document format.

---

### 3. Invoice Parsing

The extracted text is transformed into structured invoice fields.

Typical fields include:

- invoice number,
- invoice date,
- supplier name,
- customer name,
- expense category,
- currency,
- subtotal,
- VAT amount,
- total amount,
- payment status,
- due date.

---

### 4. Validation

The structured invoice is checked against a set of deterministic financial and operational rules.

This makes validation explainable and allows every exception to have a visible reason.

---

### 5. Operational Controls

The system checks whether the invoice is overdue and whether the payment status allows overdue classification.

Paid invoices are not treated as overdue.

---

### 6. Duplicate Detection

Duplicate detection uses a combination of:

```text
invoice_number + supplier_name
```

A duplicate invoice is blocked from insertion into the Master Ledger and generates a dedicated alert.

---

### 7. Supplier Analysis

For new invoices, the workflow retrieves supplier history and analyzes the current invoice against previous records.

The workflow can identify:

- unusual invoice amounts,
- unexpected category changes,
- supplier patterns requiring review.

Currency separation is preserved so invoices in different currencies are not incorrectly compared.

---

### 8. Review Decision

Invoices containing validation, overdue, or anomaly issues are routed through the review path.

Invoices without review conditions continue through the normal processing path.

---

### 9. Ledger Storage

New invoice records are inserted into the `InvoiceFlow_Master_Ledger`.

Duplicate records are intentionally blocked from insertion.

---

### 10. Alerts

Review-required and duplicate invoices can trigger automated Telegram notifications.

The alert includes relevant information such as:

- invoice number,
- supplier,
- amount,
- payment status,
- review reason,
- overdue information,
- anomaly indicators.

---

### 11. Dashboard Reporting

A dedicated Dashboard Data API reads the Master Ledger and provides structured data to the Streamlit dashboard.

This keeps the dashboard connected to the operational source rather than browser-local history.

---

# Validation & Controls

## Financial and Data Validation

InvoiceFlow AI applies multiple validation checks before determining the invoice outcome.

### Required Fields

The workflow checks the presence of required fields such as:

```text
invoice_number
invoice_date
supplier_name
currency
subtotal
vat_amount
total_amount
due_date
```

---

### Invoice Number Validation

Invoice identifiers are validated before downstream processing.

This prevents unrelated text or PDF table headers from being accepted as valid invoice numbers.

---

### Arithmetic Validation

The workflow verifies:

```text
Subtotal + VAT ≈ Total
```

A small tolerance is allowed for normal financial rounding differences.

---

### VAT Validation

The current demo configuration applies currency-aware VAT rules.

The system verifies whether the calculated VAT matches the expected VAT amount for the configured currency rule.

---

### Date Validation

The workflow checks:

- invoice date format,
- due date format,
- whether the due date occurs before the invoice date.

---

### Negative Financial Values

Unexpected negative subtotal, VAT, or total values can be flagged for review.

---

# Overdue Monitoring

Open invoices can be evaluated against an operational reference date.

The workflow considers payment status before marking an invoice overdue.

Typical open statuses include:

- Unpaid
- Partial
- Partially Paid
- Pending

Paid invoices are excluded from overdue classification.

The workflow can return:

```text
is_overdue
days_overdue
review_required
review_reason
```

---

# Duplicate Detection

Duplicate detection protects the Master Ledger from repeated invoice insertion.

A duplicate match uses:

```text
Invoice Number + Supplier Name
```

When a duplicate is detected:

```text
Duplicate Detected
        ↓
Flag Duplicate
        ↓
Review Required
        ↓
Send Duplicate Alert
        ↓
Block Ledger Insert
        ↓
Return Duplicate Result
```

The Streamlit response clearly reports:

```text
status = DUPLICATE
duplicate = true
exact_duplicate = true
ledger_added = false
```

---

# Supplier Anomaly Detection

InvoiceFlow AI includes supplier-level controls designed to identify potentially unusual activity.

## Supplier Amount Anomaly

The invoice amount can be compared with previous valid invoices from the same supplier and same currency.

The demo rule requires sufficient historical records before an anomaly can be triggered.

---

## Category Shift

Supplier category history can be analyzed to identify when the current invoice uses an unexpected expense category compared with the supplier's dominant historical behavior.

---

# Validation & Review Experience

![InvoiceFlow AI analysis concept](assets/invoiceflow_analysis_concept.png)

> *Conceptual product visualization for presentation purposes. Values and UI details are illustrative.*

The operational result view brings the most important invoice information into one interface.

It can present:

### Invoice Summary

- Invoice number
- Supplier
- Total amount
- Payment status
- Invoice date
- Due date
- Expense category
- Ledger status

### Automated Checks

- Required fields
- VAT validation
- Financial arithmetic
- Date validation
- Overdue status
- Duplicate detection

### Risk & Workflow Outcome

- Supplier amount anomaly
- Category shift
- Human review status
- Telegram alert status

This provides a human-readable explanation of what the workflow decided and why.

---

# Dashboard & Insights

![InvoiceFlow AI dashboard concept](assets/invoiceflow_dashboard_concept.png)

> *Conceptual product visualization for presentation purposes. It illustrates the intended analytics experience rather than exposing production data.*

The dashboard reads data from the complete Master Ledger through a dedicated n8n API.

It does not depend solely on browser-local invoice history.

## Dashboard KPIs

The operational dashboard can include:

- Processed files
- Ledger invoices
- Total spend
- Review-required invoices
- Overdue invoices
- Supplier count
- Duplicate count

## Dashboard Views

### Overview

Provides an executive-level summary of invoice operations.

### Payment & Aging

Supports monitoring of:

- paid invoices,
- open invoices,
- overdue invoices,
- payment status,
- due dates,
- days overdue.

### Exceptions

Supports monitoring of:

- validation issues,
- review-required invoices,
- duplicates,
- overdue invoices,
- amount anomalies,
- category shifts.

### Suppliers

Provides supplier-level invoice counts, spend, review activity, and overdue activity.

---

## Currency Handling

Financial totals are analyzed by currency rather than mixing values across:

- AED
- SAR
- USD

This avoids misleading totals created by adding values from different currencies together.

---

# Power BI Reporting

In addition to the Streamlit operational interface, the project includes a Power BI reporting layer.

The Power BI dashboard was designed around several reporting areas:

1. **Executive Overview**
2. **Supplier Analysis**
3. **Supplier Anomalies**
4. **Expense Categories**
5. **Payment & Aging**
6. **Data Quality & Exceptions**

The reporting layer supports KPI monitoring, exception analysis, supplier behavior, overdue activity, and financial visibility.

The `.pbix` implementation is intentionally not included in the public repository.

---

# n8n Workflow Automation

The production automation layer is implemented using **n8n Cloud**.

The main automation coordinates:

```text
Webhook
   ↓
Extract PDF Text
   ↓
Parse Invoice Fields
   ↓
Validate Invoice
   ↓
Check Overdue Status
   ↓
Duplicate Check
   ↓
Supplier History
   ↓
Supplier Anomaly Detection
   ↓
Review Decision
   ├── Review Alert
   └── Insert into Ledger
            ↓
       UI Response
```

The architecture uses separate branches for clean invoices, review-required invoices, and duplicate invoices.

---

# Telegram Alerts

Telegram is used for operational review notifications.

Two primary alert scenarios are supported:

### Review Alert

Triggered when an invoice requires attention because of:

- validation problems,
- overdue status,
- supplier anomaly,
- category shift,
- or another review condition.

### Duplicate Alert

Triggered when the invoice already exists in the Master Ledger.

The duplicate is blocked instead of being inserted again.

---

# Technology Stack

| Layer | Technology |
|---|---|
| **Programming** | Python |
| **Data Processing** | Python / pandas |
| **Notebook Development** | Jupyter Notebook |
| **Workflow Automation** | n8n Cloud |
| **Operational UI** | Streamlit |
| **Workflow Storage** | n8n Data Tables |
| **Alerts & Notifications** | Telegram |
| **Business Intelligence** | Power BI |
| **Data Exchange** | JSON / CSV / Excel / PDF |
| **Integration** | Webhooks / API-based communication |
| **Version Control & Portfolio** | GitHub |

---

# Skills Demonstrated

## Python Development

- Data processing
- Data transformation
- Invoice parsing
- Business-rule implementation
- Validation logic
- Financial calculations
- Structured API responses
- Exception handling

---

## Workflow Automation

- n8n workflow architecture
- Webhooks
- Conditional branching
- Automated processing pipelines
- Data Table integration
- API response handling
- Automated notifications
- Human-in-the-loop workflows

---

## Data Engineering

- Data extraction
- Data normalization
- Data-quality validation
- Schema design
- Master Ledger design
- Duplicate handling
- Exception management
- Structured financial data preparation

---

## Business Intelligence

- KPI design
- Power BI dashboard development
- Supplier analysis
- Expense-category analysis
- Payment and aging analysis
- Data-quality reporting
- Exception monitoring
- Operational dashboards

---

## Automation & Integration

- REST-style webhook workflows
- Streamlit-to-n8n integration
- Dashboard APIs
- Telegram notification workflows
- Workflow storage
- JSON data exchange

---

## Product Development

- MVP architecture
- Streamlit interface design
- Business workflow design
- Financial automation
- End-to-end testing
- Documentation
- Public portfolio packaging
- Security-conscious public release

---

# Validation & Workflow Testing

The workflow was tested across the three main operational branches.

---

## Test 1 — Clean Invoice

Expected processing path:

```text
Invoice
   ↓
Parsing Successful
   ↓
Validation Passed
   ↓
No Duplicate
   ↓
No Review Required
   ↓
Added to Ledger
   ↓
Return Result
```

Expected result:

```text
PROCESSED
```

Verified behavior included:

- structured invoice fields,
- validation pass,
- no duplicate,
- ledger insertion,
- successful Streamlit response.

---

## Test 2 — Review-Required Invoice

Expected processing path:

```text
Invoice
   ↓
Validation / Operational Issue
   ↓
Review Required
   ├── Telegram Alert
   └── Insert Record
          ↓
      UI Response
```

Expected result:

```text
REVIEW_REQUIRED
```

The workflow preserves review reasons so multiple issues can be communicated together.

For example:

```text
Due date is before invoice date
+
Invoice is overdue
```

---

## Test 3 — Duplicate Invoice

Expected processing path:

```text
Invoice
   ↓
Duplicate Found
   ↓
Flag Duplicate
   ├── Duplicate Alert
   └── Duplicate UI Response
```

Expected result:

```text
DUPLICATE
```

Verified duplicate behavior:

```text
duplicate = true
exact_duplicate = true
ledger_added = false
telegram_alert_sent = true
```

---

## Dashboard API Test

The Dashboard Data API was also tested end-to-end against the Master Ledger.

Expected path:

```text
Dashboard Data API
      ↓
Get Dashboard Ledger
      ↓
Build Dashboard Data
      ↓
Return Dashboard Data
```

The API successfully returned structured Master Ledger data for dashboard consumption.

---

# Example Workflow Response

A public example is available at:

[`examples/sample_result.json`](examples/sample_result.json)

Simplified structure:

```json
{
  "status": "REVIEW_REQUIRED",
  "invoice_number": "INV-2026-XXXX",
  "supplier_name": "Example Supplier",
  "currency": "AED",
  "review_required": true,
  "is_overdue": true,
  "duplicate": false,
  "ledger_added": true
}
```

The sample is intentionally simplified and does not contain production information.

---

# Project Documentation

Additional documentation is included in this repository.

### Architecture

[Architecture Overview](docs/ARCHITECTURE.md)

### Testing

[Testing Summary](docs/TESTING_SUMMARY.md)

### Public Repository Policy

[Public Repository Scope](docs/PUBLIC_REPOSITORY_SCOPE.md)

### Illustrated Project Overview

[InvoiceFlow AI — Project Overview (PDF)](docs/InvoiceFlow_AI_Project_Overview_EN.pdf)

---

# Repository Scope

This repository is intentionally a **portfolio-safe public release**.

It demonstrates:

- the business problem,
- project architecture,
- automation design,
- interface concept,
- validation strategy,
- workflow logic,
- analytics approach,
- testing methodology,
- technology stack,
- and selected project outcomes.

The proprietary implementation remains private.

---

## Intentionally Not Included

The following materials are intentionally excluded:

- Full Python processing implementation
- Jupyter development notebooks
- Production Streamlit source code
- n8n workflow export
- n8n Python node implementation
- Production parser code
- Production validation code
- Supplier anomaly implementation
- Power BI `.pbix` file
- DAX implementation
- Master Ledger records
- Private datasets
- Generated invoice corpus
- Production webhook URLs
- Telegram identifiers
- Telegram credentials
- Authentication secrets
- Environment configuration
- Private execution history
- Client-specific mappings
- Sensitive configuration

For additional details:

[Public Repository Scope](docs/PUBLIC_REPOSITORY_SCOPE.md)

---

# Security

No production credentials, access tokens, authentication secrets, private identifiers, or confidential invoice data are intentionally included in this repository.

The project uses repository safety rules through:

```text
.gitignore
```

These rules help prevent accidental publication of:

- `.env` files,
- credentials,
- private invoice files,
- datasets,
- notebooks,
- workflow exports,
- Power BI files,
- local configuration,
- and private backups.

See:

[SECURITY.md](SECURITY.md)

---

# Project Overview PDF

A concise illustrated overview of the project is available here:

### [InvoiceFlow AI — Project Overview (PDF)](docs/InvoiceFlow_AI_Project_Overview_EN.pdf)

The document provides a visual explanation of:

- the business problem,
- project architecture,
- invoice-processing flow,
- validation controls,
- automation,
- dashboard reporting,
- and project outcomes.

---

# Project Status

## Production-Ready MVP / Portfolio Release

Completed components include:

- Synthetic invoice environment
- Multi-layout PDF invoice parsing
- Financial field extraction
- Required-field validation
- Invoice-number validation
- Date validation
- Arithmetic validation
- VAT validation
- Negative-value checks
- Overdue monitoring
- Duplicate detection
- Supplier-history analysis
- Supplier amount anomaly detection
- Supplier category-shift detection
- Review routing
- Telegram review alerts
- Duplicate alerts
- Master Ledger integration
- Dashboard Data API
- Streamlit operational interface
- Light / Dark interface modes
- Power BI reporting
- End-to-end workflow testing
- Public project documentation
- GitHub portfolio release

---

# Future Enhancements

Potential next-stage improvements include:

- OCR support for scanned invoices
- Arabic / English OCR improvements
- Document confidence scoring
- Supplier master-data management
- Fuzzy supplier-name matching
- Purchase-order matching
- Three-way matching
- Approval workflows for high-value invoices
- QuickBooks integration
- Xero integration
- ERP API integration
- Multi-company dashboards
- Multi-branch reporting
- Database-backed storage for larger workloads
- AI-assisted exception summaries
- Configurable client-specific VAT rules
- Configurable anomaly thresholds

These improvements are part of the broader product roadmap rather than the current public MVP.

---

# Why InvoiceFlow AI?

Traditional invoice processes often require people to spend time checking documents that do not actually require human judgment.

InvoiceFlow AI is designed around a different model:

> **Automate the predictable work and surface the exceptions that need attention.**

By connecting:

```text
Document Processing
        +
Financial Validation
        +
Operational Controls
        +
Exception Handling
        +
Alerts
        +
Reporting
```

the workflow aims to deliver:

- less manual work,
- fewer repetitive checks,
- cleaner financial data,
- faster exception handling,
- better operational visibility,
- stronger supplier monitoring,
- and more informed decisions.

---

# Portfolio & Client Use

The public repository is designed to demonstrate the project's capabilities without giving away the proprietary implementation.

The full implementation can remain private while this repository acts as:

- a technical portfolio,
- a public case study,
- an architecture showcase,
- a workflow automation example,
- a financial-operations project,
- and a client-facing proof of concept.

---

# Usage & Intellectual Property

This repository is provided for:

**Portfolio review and demonstration purposes only.**

No permission is granted to:

- copy the proprietary implementation,
- reproduce the private workflow,
- redistribute project materials,
- reverse engineer private implementation details,
- commercialize the proprietary work,
- or create derivative implementations from protected project materials.

See:

[LICENSE](LICENSE)

for the repository terms.

---

# GitHub Repository

### InvoiceFlow AI

https://github.com/rawanghassan/InvoiceFlow-AI

---

<div align="center">

## InvoiceFlow AI

### Automate invoices. Detect issues. Improve financial operations.

**Python · n8n · Streamlit · Power BI · Telegram**

<br>

**Less manual work. Fewer errors. Better visibility. Faster decisions.**

</div>
