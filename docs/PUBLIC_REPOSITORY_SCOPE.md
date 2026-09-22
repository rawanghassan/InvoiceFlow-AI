# Public Repository Scope

This repository is designed for a public GitHub portfolio while protecting the project's proprietary implementation.

## Included

- Product overview
- High-level system architecture
- Technology stack
- Conceptual product visualizations
- Testing summary
- Sanitized sample response
- Illustrated project overview PDF

The images in this public repository are presentation-oriented concept visuals. They communicate the intended product experience without exposing production data, account details, endpoints, or private implementation artifacts.

## Intentionally withheld

The following files and implementation details are not included in the public repository:

- Full `.ipynb` notebooks
- Python processing modules
- Production Streamlit application source
- n8n workflow JSON exports
- n8n Python Code-node implementations
- Webhook URLs and account-specific endpoints
- Telegram credentials, identifiers, and connection data
- Master Ledger export
- Training / benchmark Excel workbooks
- Full PDF invoice dataset
- Power BI `.pbix` project
- DAX measures and internal model configuration
- Local history / state files
- Pickle or JSON notebook-state backups

## Why

The public repository is intended to demonstrate engineering capability and product thinking without making the full system trivially reproducible or exposing operational configuration.

A private implementation repository can be maintained separately for backup, deployment, or controlled review.
