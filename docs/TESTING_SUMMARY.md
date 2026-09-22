# Testing Summary

The final workflow was validated using controlled synthetic invoices and branch-level smoke tests.

## Branch tests

| Test | Expected result | Outcome |
|---|---|---|
| Clean new invoice | Processed, ledger insert, no review alert | Passed |
| Review-required invoice | Review status, reason preserved, alert sent, ledger insert | Passed |
| Duplicate invoice | Duplicate status, alert sent, ledger insert blocked | Passed |
| Dashboard API | Return Master Ledger rows as JSON | Passed |

## Key behaviors verified

- Structured invoice response returned to Streamlit
- Required-field validation
- Invoice-number format validation
- VAT and arithmetic checks
- Paid invoices excluded from overdue status
- Open invoices can be marked overdue
- Duplicate invoice is not inserted again
- Review reasons can contain multiple causes
- Review and duplicate Telegram branches do not replace invoice payloads
- Dashboard data is sourced from the Master Ledger

## Note

Detailed test fixtures, execution logs, benchmark notebooks, and synthetic invoice datasets are retained privately and are not part of the public GitHub package.
