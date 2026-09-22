# Security and Disclosure

This public portfolio package contains no intended production credentials or secrets.

Do not commit any of the following to the public repository:

- `.env` files
- API keys or tokens
- Telegram bot tokens or chat identifiers
- n8n credentials
- production webhook URLs
- private workflow exports
- private datasets or customer invoices
- Power BI files containing operational data
- notebook state files such as `.pkl`

If a secret is accidentally committed, rotate it immediately and remove it from Git history rather than only deleting the latest file.
