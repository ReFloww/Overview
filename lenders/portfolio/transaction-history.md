# Transaction History

Your complete record of all activities on ReFlow. This guide explains how to view, filter, and export your transaction history.

## Accessing Transaction History

Navigate to **Portfolio** → **History** or use the main **History** tab.

## Transaction Types

### Investment Transactions

| Type | Description |
|------|-------------|
| **Buy** | Purchased loan tokens |
| **Sell** | Sold loan tokens |
| **Invest** | Invested in managed fund |
| **Withdraw** | Withdrew from managed fund |

### Earnings Transactions

| Type | Description |
|------|-------------|
| **Interest** | Interest payment received |
| **Principal** | Principal repayment received |
| **Distribution** | Fund manager distribution |

### System Transactions

| Type | Description |
|------|-------------|
| **Fee** | Platform or gas fees |
| **Approval** | Token spending approval |
| **Claim** | Claimed pending rewards |

## Transaction Details

Each transaction shows:

| Field | Description |
|-------|-------------|
| **Date/Time** | When transaction occurred |
| **Type** | Transaction category |
| **Asset** | Token or fund involved |
| **Amount** | USDT or token quantity |
| **Status** | Completed, Pending, Failed |
| **Tx Hash** | Blockchain transaction ID |
| **Gas Used** | Transaction fee paid |

## Filtering Transactions

### By Date Range

- Preset: 7 days, 30 days, 90 days, 1 year
- Custom: Select start and end dates

### By Type

Filter by transaction category:
- [ ] Investments
- [ ] Earnings
- [ ] Trades
- [ ] Withdrawals
- [ ] All

### By Asset

Filter by specific:
- Loan token
- Managed fund
- Token type (USDT, etc.)

### By Status

- Completed
- Pending
- Failed

## Understanding Status

| Status | Meaning | Action Needed |
|--------|---------|---------------|
| **Completed** | Successfully processed | None |
| **Pending** | Processing on blockchain | Wait |
| **Failed** | Transaction reverted | Review and retry |
| **Processing** | Being confirmed | Wait |

## Transaction Hash

### What Is a Tx Hash?

A unique identifier for your blockchain transaction. Example:
```
0x1234567890abcdef1234567890abcdef12345678...
```

### Verifying Transactions

1. Click the transaction hash
2. Opens block explorer (Mantlescan)
3. View full on-chain details
4. Verify transaction independently

### Explorer Shows

- Transaction status
- Block number
- From/To addresses
- Token transfers
- Gas details
- Contract interactions

## Search Functionality

### Quick Search

Type in search bar:
- Transaction hash
- Asset name
- Amount

### Advanced Search

Combine filters:
1. Set date range
2. Select transaction types
3. Choose asset
4. Apply filters

## Sorting Options

Sort by:
- Date (newest/oldest first)
- Amount (highest/lowest)
- Type

## Pagination

For long histories:
- 25, 50, 100 items per page
- Page navigation
- Jump to page

## Exporting History

### Export Formats

| Format | Best For |
|--------|----------|
| **CSV** | Spreadsheets, analysis |
| **PDF** | Records, printing |
| **JSON** | Data processing |

### Export Process

1. Set desired filters
2. Click **"Export"**
3. Select format
4. Choose date range
5. Download file

### Export Contents

CSV includes:
```
Date, Type, Asset, Amount, Status, Tx Hash, Fee, Notes
```

## Use Cases

### Portfolio Analysis

- Calculate total invested
- Track earnings over time
- Analyze trading performance

### Tax Reporting

- All income transactions
- Gains/losses from trades
- Fee deductions
- Annual summaries

### Dispute Resolution

- Transaction proof
- Timestamp verification
- On-chain confirmation

### Performance Review

- Investment timing
- Distribution frequency
- Fee impact analysis

## Tips for Managing History

### Regular Review

Check history weekly to:
- Verify all transactions
- Catch any issues early
- Stay informed

### Organize with Notes

Add notes to transactions:
1. Click transaction
2. Add personal note
3. Save for reference

### Monthly Exports

Download monthly records for:
- Personal records
- Tax preparation
- Portfolio tracking

## Troubleshooting

### Missing Transaction

If a transaction doesn't appear:
1. Check pending transactions
2. Wait for blockchain confirmation
3. Refresh the page
4. Check correct filters

### Transaction Failed

If a transaction failed:
1. View error message
2. Check gas/balance
3. Retry transaction
4. Contact support if persists

### Duplicate Entries

If you see duplicates:
1. Check transaction hashes
2. Verify blockchain explorer
3. Report if confirmed duplicate

## Related Pages

- [Dashboard Overview](dashboard.md)
- [Tracking Earnings](tracking-earnings.md)
- [Portfolio Overview](README.md)
