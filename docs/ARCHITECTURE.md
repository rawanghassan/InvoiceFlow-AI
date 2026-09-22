# Architecture Overview

InvoiceFlow AI separates invoice-processing logic from reporting and presentation so each layer can evolve independently.

## 1. Processing path

```text
Invoice PDF
  → Streamlit upload
  → n8n ingestion webhook
  → PDF text extraction
  → invoice parser
  → validation engine
  → overdue check
  → duplicate check
  → supplier-history analysis
  → review routing
  → Master Ledger
```

### Validation layer

The validation layer checks:

- required fields
- invoice-number format
- invoice and due-date validity
- due date before invoice date
- negative financial values
- subtotal + VAT vs total
- expected VAT by supported currency

### Operational controls

- Paid invoices are not classified as overdue.
- Open statuses can be evaluated for days overdue.
- Duplicate detection uses both invoice number and supplier.
- Supplier amount anomalies are evaluated against same-supplier, same-currency history.
- Category shifts require a minimum historical sample and a dominant historical category.

## 2. Review routing

Invoices requiring attention retain all review reasons in a unified review message. Telegram alerts are side branches and do not overwrite invoice data passed to the ledger or UI.

## 3. Dashboard path

```text
Streamlit Dashboard
  → Dashboard Data API (GET)
  → Master Ledger rows
  → response builder
  → JSON response
  → dashboard KPIs and views
```

The dashboard is therefore backed by the complete ledger rather than only the local Streamlit session history.

## 4. Public repository boundary

The implementation details that reproduce the core parser, validation engine, anomaly logic, workflow nodes, and production endpoints are private and intentionally excluded from this repository.
