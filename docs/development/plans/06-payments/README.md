# Payments Plan Index

## Objective

Own the internal wallet, wallet funding transactions, payment-method state, and manual review processes defined for Rehla.

## Features

| ID | Feature | Dependencies | Status |
|---|---|---|---|
| `06-01-wallet-domain` | Wallet Domain | 02-01-customer-profile, 00-03-domain-package-boundaries | DRAFT |
| `06-02-wallet-funding-request` | Wallet Funding Request | 06-01-wallet-domain | DRAFT |
| `06-03-bank-transfer-funding` | Bank Transfer Funding | 06-02-wallet-funding-request | DRAFT |
| `06-04-instant-payment-providers` | Instant Payment Providers | 06-02-wallet-funding-request | DRAFT |
| `06-05-wallet-transaction-lifecycle` | Wallet Transaction Lifecycle | 06-03-bank-transfer-funding, 06-04-instant-payment-providers | DRAFT |
| `06-06-payment-review` | Bank Transfer Review | 06-03-bank-transfer-funding, 01-04-role-based-access | DRAFT |
| `06-07-banks-and-payment-methods` | Banks and Payment Methods | 06-03-bank-transfer-funding, 06-04-instant-payment-providers | DRAFT |
| `06-08-wallet-history-and-balance` | Wallet Balance and Transaction History | 06-05-wallet-transaction-lifecycle | DRAFT |

## Feature Bundle Contract

Every feature directory has exactly:

```text
<feature>/
├── spec.md
├── checklist.md
├── plan.md
└── tasks.md
```

The files are intentionally identical in structure across all domains. Feature-specific content is filled from the approved requirements, domain boundary, dependency graph, and technical plan.
