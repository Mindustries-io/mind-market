# Invoice Templates & Late-Payment Chase Sequence

Used by the `invoicing` agent. All business details come from config (`business.*`, `invoicing.*`) — never invent legal identifiers.

## Standard Invoice Template (markdown draft)

```
INVOICE {invoice_number}                         Date: {issue_date}
                                                 Due:  {due_date}  ({payment_terms_days} days)

FROM
{business.name} — {business.legal_form}
{business.address}
{business.tax_id_label}: {business.tax_id}      ← e.g. VAT ID / EIN / P.IVA, per jurisdiction
{business.email}

BILL TO
{client.name}
{client.address}
{client.tax_id (if B2B and required)}

| # | Description | Qty | Unit price | Amount |
|---|-------------|-----|-----------|--------|
| 1 | {line item} | {n} | {CURRENCY} {x} | {CURRENCY} {x·n} |

Subtotal:                {CURRENCY} {subtotal}
{VAT line — see rules}:  {CURRENCY} {vat_amount}
TOTAL DUE:               {CURRENCY} {total}

Payment details: {invoicing.payment_details}   (IBAN / account / payment link)
Reference: {invoice_number}
{invoicing.footer_notes — e.g. late-fee clause, reverse-charge note}
```

### VAT / GST rules

- `tax.vat.registered = false` → no VAT line; add the config's small-business exemption note if provided (e.g. the local "VAT-exempt under §X" wording the user's accountant supplied).
- Registered, domestic B2B/B2C → apply `tax.vat.rate` on the subtotal.
- Registered, cross-border B2B within a reverse-charge regime → 0% with note "VAT reverse-charged to recipient — {client VAT ID}".
- Exports / out-of-scope → 0% with the config's export note. When unsure, draft both variants and tell the user to confirm with their accountant.

### Numbering

Follow `invoicing.numbering` (e.g. `{YYYY}-{seq}`). Sequential, no gaps, never reuse a number. Ask before assuming the next number if the ledger is stale.

## Late-Payment Chase Sequence (3 steps)

Tone escalates politely → firmly. Wait times are defaults; config `invoicing.chase_days` overrides.

**Step 1 — Friendly nudge (due date + 3 days).**
Subject: `Invoice {number} — friendly reminder`
Assume good faith: "just making sure this didn't slip through", re-attach invoice, restate amount + payment details, offer to help if anything is unclear.

**Step 2 — Firm reminder (due date + 14 days).**
Subject: `Invoice {number} now {n} days overdue`
State plainly it is overdue, give the exact amount and original due date, request payment or a payment date within 5 business days, mention any contractual late fee/interest only if the config or contract provides one.

**Step 3 — Final notice (due date + 30 days).**
Subject: `Final notice — invoice {number}`
State that this is the final reminder before escalation (collections / legal / statutory interest where applicable), set a hard deadline (7 days), remain professional — no threats beyond stated remedies. Recommend the user check jurisdiction-specific statutory interest (via legal-os if installed) before sending.

After step 3: recommend a human decision — write-off, mediation, collections agency, or small-claims. Never send anything automatically; every message is a draft for the user.

## Outstanding-Invoice Ledger

Simple local ledger at `~/.claude/plugins/data/finance-os/invoices.json`:

```json
{
  "invoices": [
    {
      "number": "2026-014",
      "client": "Acme Studio",
      "issued": "2026-06-01",
      "due": "2026-06-15",
      "amount": 4200.00,
      "currency": "EUR",
      "status": "overdue",
      "chase_step": 1,
      "notes": "Aurora onboarding project, milestone 2"
    }
  ],
  "updated_at": "2026-07-01"
}
```

Statuses: `draft` → `sent` → `paid` | `overdue` (auto-derive from due date) | `written-off`. The user maintains truth (they confirm payments); the agent updates the file only on the user's say-so and always shows the diff first.
