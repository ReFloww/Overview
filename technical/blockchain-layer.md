# Blockchain Layer

Documentation of ReFlow's blockchain integration, smart contract architecture, and on-chain operations.

## Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                       BLOCKCHAIN LAYER                               │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌──────────────────────────────────────────────────────────────┐  │
│   │                    Mantle Network (L2)                        │  │
│   │   • Low gas costs      • EVM compatible                       │  │
│   │   • Fast finality      • Ethereum security                    │  │
│   └──────────────────────────────────────────────────────────────┘  │
│                              │                                       │
│   ┌──────────────────────────┴──────────────────────────────────┐   │
│   │                    Smart Contracts                           │   │
│   │   ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │   │
│   │   │  Factory    │  │  Factory    │  │    Loan Token       │ │   │
│   │   │  P2P        │  │  Manager    │  │    (ERC-20)         │ │   │
│   │   └─────────────┘  └─────────────┘  └─────────────────────┘ │   │
│   └──────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## Network Configuration

### Mantle Sepolia Testnet

| Parameter | Value |
|-----------|-------|
| **Network Name** | Mantle Sepolia Testnet |
| **Chain ID** | 5003 |
| **RPC URL** | https://rpc.sepolia.mantle.xyz |
| **Currency Symbol** | MNT |
| **Block Explorer** | https://sepolia.mantlescan.xyz |
| **Faucet** | https://faucet.sepolia.mantle.xyz |

### Mantle Mainnet (Production)

| Parameter | Value |
|-----------|-------|
| **Network Name** | Mantle |
| **Chain ID** | 5000 |
| **RPC URL** | https://rpc.mantle.xyz |
| **Currency Symbol** | MNT |
| **Block Explorer** | https://explorer.mantle.xyz |

## Deployed Contracts

### Testnet (Mantle Sepolia)

| Contract | Address |
|----------|---------|
| Mock USDT | `0xe01c5464816a544d4d0d6a336032578bd4629F10` |
| Factory P2P | `0xa411df45e20d266500363c76ecbf0b8e483fd408` |
| Factory Manager | `0x4d1a3d97109b2fb7e81023cf0a97aea4277d7235` |

## Contract Architecture

### Factory Pattern

```
┌────────────────────────┐
│     Factory P2P        │
│   (Single Instance)    │
└───────────┬────────────┘
            │ deploys
            ▼
┌────────────────────────┐     ┌────────────────────────┐
│   Loan Token #1        │     │   Loan Token #2        │
│   (Per Loan)           │ ... │   (Per Loan)           │
└────────────────────────┘     └────────────────────────┘
```

### Contract Relationships

```
Factory P2P
    │
    ├──▶ Creates Loan Token contracts
    ├──▶ Tracks all deployed loans
    └──▶ Provides registry functions

Factory Manager
    │
    ├──▶ Creates Fund Manager contracts
    ├──▶ Tracks approved managers
    └──▶ Manages fund operations

Loan Token (ERC-20)
    │
    ├──▶ Represents loan ownership
    ├──▶ Handles distributions
    └──▶ Manages token transfers
```

## Smart Contract Functions

### Factory P2P

```solidity
interface IFactoryP2P {
    // Create new loan token
    function createLoan(
        string memory name,
        string memory symbol,
        uint256 principal,
        uint256 interestRate,
        uint256 duration,
        address stablecoin
    ) external returns (address loanToken);

    // Get all loans
    function getAllLoans() external view returns (address[] memory);

    // Get loan by index
    function getLoan(uint256 index) external view returns (address);

    // Get loan count
    function loanCount() external view returns (uint256);
}
```

### Loan Token

```solidity
interface ILoanToken {
    // ERC-20 standard functions
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);

    // Loan-specific functions
    function invest(uint256 amount) external;
    function distribute(uint256 amount) external;
    function claimDistribution() external;

    // View functions
    function loanDetails() external view returns (LoanDetails memory);
    function accruedInterest(address holder) external view returns (uint256);
    function pendingDistribution(address holder) external view returns (uint256);
}
```

## Transaction Types

### Investment Transaction

```
User → approve USDT spending → Loan Token contract
User → invest(amount) → Loan Token contract
Loan Token → transferFrom USDT → Loan Token holds USDT
Loan Token → mint loan tokens → User receives tokens
```

### Distribution Transaction

```
Admin → distribute(amount) → Loan Token contract
Loan Token → record distribution → Per-share calculation
Token holders → claimDistribution() → Receive USDT
```

### Secondary Market Transaction

```
Seller → approve tokens → Market contract
Buyer → purchase(listing) → Market contract
Market → transfer tokens → Seller → Buyer
Market → transfer USDT → Buyer → Seller
```

## Events

### Key Contract Events

```solidity
// Factory events
event LoanCreated(address indexed loanToken, uint256 principal);

// Loan token events
event Investment(address indexed investor, uint256 amount, uint256 tokens);
event Distribution(uint256 amount, uint256 perToken);
event Claimed(address indexed holder, uint256 amount);
event Transfer(address indexed from, address indexed to, uint256 value);
event Approval(address indexed owner, address indexed spender, uint256 value);
```

### Event Monitoring

```typescript
// Backend: Monitor investment events
client.watchContractEvent({
  address: loanTokenAddress,
  abi: loanTokenAbi,
  eventName: 'Investment',
  onLogs: (logs) => {
    logs.forEach(log => {
      // Process investment event
      recordInvestment(log.args);
    });
  }
});
```

## Gas Optimization

### Mantle Advantages

| Feature | Benefit |
|---------|---------|
| L2 Rollup | Lower gas costs than L1 |
| Efficient Execution | Optimized EVM |
| Batch Processing | Reduced per-tx costs |

### Typical Gas Costs

| Operation | Estimated Gas |
|-----------|---------------|
| Token Approval | ~50,000 |
| Investment | ~100,000 |
| Claim Distribution | ~80,000 |
| Token Transfer | ~60,000 |

*Costs vary based on network conditions.*

## Security Considerations

### Contract Security

| Measure | Implementation |
|---------|---------------|
| Access Control | Role-based permissions |
| Reentrancy Protection | ReentrancyGuard |
| Integer Safety | Solidity 0.8+ checks |
| Upgradability | Careful proxy patterns |

### Best Practices

```solidity
// Access control example
modifier onlyAdmin() {
    require(msg.sender == admin, "Not authorized");
    _;
}

// Reentrancy protection
uint256 private _status;
modifier nonReentrant() {
    require(_status != ENTERED, "Reentrant call");
    _status = ENTERED;
    _;
    _status = NOT_ENTERED;
}
```

## Integration Patterns

### Reading Contract State

```typescript
// Using Viem
const balance = await client.readContract({
  address: loanTokenAddress,
  abi: loanTokenAbi,
  functionName: 'balanceOf',
  args: [userAddress]
});

const loanDetails = await client.readContract({
  address: loanTokenAddress,
  abi: loanTokenAbi,
  functionName: 'loanDetails'
});
```

### Writing to Contracts

```typescript
// Using Wagmi (frontend)
const { writeContract } = useWriteContract();

await writeContract({
  address: loanTokenAddress,
  abi: loanTokenAbi,
  functionName: 'invest',
  args: [investmentAmount]
});
```

### Transaction Confirmation

```typescript
// Wait for confirmation
const receipt = await client.waitForTransactionReceipt({
  hash: txHash,
  confirmations: 1
});

if (receipt.status === 'success') {
  // Transaction confirmed
}
```

## Error Handling

### Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `InsufficientBalance` | Not enough tokens | Check balance first |
| `NotApproved` | Missing approval | Call approve first |
| `LoanClosed` | Loan already finished | Cannot invest |
| `NotAuthorized` | Missing permissions | Check role |

### Error Decoding

```typescript
try {
  await writeContract(/* ... */);
} catch (error) {
  if (error.name === 'ContractFunctionExecutionError') {
    const decodedError = decodeErrorResult({
      abi: loanTokenAbi,
      data: error.data
    });
    // Handle specific error
  }
}
```

## Related Pages

- [Smart Contract Overview](../contracts/overview.md)
- [Contract Architecture](../contracts/architecture.md)
- [Tech Stack](tech-stack.md)
