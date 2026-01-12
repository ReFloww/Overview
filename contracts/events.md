# Events and Logs

Documentation of smart contract events emitted by ReFlow contracts.

## Overview

Events are used to:
- Track contract activity
- Enable off-chain indexing
- Provide transaction receipts
- Support UI updates

## Factory P2P Events

### LoanCreated

Emitted when a new loan token is deployed.

```solidity
event LoanCreated(
    address indexed loanToken,
    string name,
    string symbol,
    uint256 principal,
    uint256 interestRate,
    uint256 duration
);
```

| Parameter | Type | Indexed | Description |
|-----------|------|---------|-------------|
| loanToken | address | Yes | New contract address |
| name | string | No | Token name |
| symbol | string | No | Token symbol |
| principal | uint256 | No | Loan amount |
| interestRate | uint256 | No | Interest rate (bps) |
| duration | uint256 | No | Duration (seconds) |

**Usage:**
```typescript
const events = await publicClient.getContractEvents({
  address: factoryAddress,
  abi: factoryP2PAbi,
  eventName: 'LoanCreated',
  fromBlock: 0n
});
```

## Loan Token Events

### Investment

Emitted when someone invests in a loan.

```solidity
event Investment(
    address indexed investor,
    uint256 amount
);
```

| Parameter | Type | Indexed | Description |
|-----------|------|---------|-------------|
| investor | address | Yes | Investor address |
| amount | uint256 | No | Amount invested |

**Usage:**
```typescript
// Watch for new investments
publicClient.watchContractEvent({
  address: loanAddress,
  abi: loanTokenAbi,
  eventName: 'Investment',
  onLogs: (logs) => {
    logs.forEach(log => {
      console.log(`${log.args.investor} invested ${log.args.amount}`);
    });
  }
});
```

### Distribution

Emitted when repayment is distributed.

```solidity
event Distribution(
    uint256 amount,
    uint256 perToken
);
```

| Parameter | Type | Indexed | Description |
|-----------|------|---------|-------------|
| amount | uint256 | No | Total distributed |
| perToken | uint256 | No | Amount per token |

### Claimed

Emitted when a holder claims distribution.

```solidity
event Claimed(
    address indexed holder,
    uint256 amount
);
```

| Parameter | Type | Indexed | Description |
|-----------|------|---------|-------------|
| holder | address | Yes | Claimer address |
| amount | uint256 | No | Amount claimed |

### Transfer (ERC-20)

Standard ERC-20 transfer event.

```solidity
event Transfer(
    address indexed from,
    address indexed to,
    uint256 value
);
```

### Approval (ERC-20)

Standard ERC-20 approval event.

```solidity
event Approval(
    address indexed owner,
    address indexed spender,
    uint256 value
);
```

## Fund Manager Events

### Deposit

Emitted when user deposits to fund.

```solidity
event Deposit(
    address indexed user,
    uint256 amount,
    uint256 shares
);
```

### Withdraw

Emitted when user withdraws from fund.

```solidity
event Withdraw(
    address indexed user,
    uint256 amount,
    uint256 shares
);
```

### InvestmentMade

Emitted when manager invests fund assets.

```solidity
event InvestmentMade(
    address indexed loan,
    uint256 amount
);
```

## Querying Events

### Get Historical Events

```typescript
// All investments for a loan
const investments = await publicClient.getContractEvents({
  address: loanAddress,
  abi: loanTokenAbi,
  eventName: 'Investment',
  fromBlock: 0n,
  toBlock: 'latest'
});

// Filter by investor
const myInvestments = await publicClient.getContractEvents({
  address: loanAddress,
  abi: loanTokenAbi,
  eventName: 'Investment',
  args: {
    investor: myAddress
  },
  fromBlock: 0n
});
```

### Get Events in Block Range

```typescript
const recentEvents = await publicClient.getContractEvents({
  address: loanAddress,
  abi: loanTokenAbi,
  eventName: 'Investment',
  fromBlock: currentBlock - 1000n,
  toBlock: currentBlock
});
```

### Watch Real-Time Events

```typescript
const unwatch = publicClient.watchContractEvent({
  address: loanAddress,
  abi: loanTokenAbi,
  eventName: 'Distribution',
  onLogs: (logs) => {
    // Handle new distributions
    logs.forEach(handleDistribution);
  }
});

// Later: stop watching
unwatch();
```

## Event Indexing

### Backend Event Processing

```typescript
// Example: Index investments to database
async function indexInvestments(loanAddress: string) {
  const events = await publicClient.getContractEvents({
    address: loanAddress,
    abi: loanTokenAbi,
    eventName: 'Investment',
    fromBlock: lastIndexedBlock
  });

  for (const event of events) {
    await db.investment.create({
      data: {
        loanAddress,
        investor: event.args.investor,
        amount: event.args.amount.toString(),
        blockNumber: event.blockNumber,
        txHash: event.transactionHash
      }
    });
  }
}
```

### Real-Time Sync

```typescript
// Start real-time sync
publicClient.watchContractEvent({
  address: loanAddress,
  abi: loanTokenAbi,
  onLogs: async (logs) => {
    for (const log of logs) {
      // Process and store
      await processEvent(log);
      // Update UI
      notifyClients(log);
    }
  }
});
```

## Log Analysis

### Calculate Total Invested

```typescript
async function getTotalInvested(loanAddress: string) {
  const events = await publicClient.getContractEvents({
    address: loanAddress,
    abi: loanTokenAbi,
    eventName: 'Investment',
    fromBlock: 0n
  });

  return events.reduce(
    (total, event) => total + event.args.amount,
    0n
  );
}
```

### Track Investor Activity

```typescript
async function getInvestorStats(loanAddress: string, investor: string) {
  const investments = await publicClient.getContractEvents({
    address: loanAddress,
    abi: loanTokenAbi,
    eventName: 'Investment',
    args: { investor },
    fromBlock: 0n
  });

  const claims = await publicClient.getContractEvents({
    address: loanAddress,
    abi: loanTokenAbi,
    eventName: 'Claimed',
    args: { holder: investor },
    fromBlock: 0n
  });

  return {
    totalInvested: investments.reduce((sum, e) => sum + e.args.amount, 0n),
    totalClaimed: claims.reduce((sum, e) => sum + e.args.amount, 0n),
    investmentCount: investments.length,
    claimCount: claims.length
  };
}
```

## Block Explorer

View events on Mantlescan:

1. Go to contract address
2. Click "Events" tab
3. Filter by event name
4. View decoded parameters

**Example URL:**
```
https://sepolia.mantlescan.xyz/address/0x.../events
```

## Related Pages

- [Contract Interactions](interactions.md)
- [Blockchain Layer](../technical/blockchain-layer.md)
- [Event Listening](../developers/web3-integration/event-listening.md)
