# Smart Contract Overview

ReFlow uses smart contracts on Mantle Network to manage loan tokenization, investments, and distributions.

## Contract Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                    SMART CONTRACT ARCHITECTURE                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│    ┌──────────────────────┐      ┌──────────────────────┐          │
│    │     Factory P2P      │      │   Factory Manager    │          │
│    │    (Singleton)       │      │    (Singleton)       │          │
│    └──────────┬───────────┘      └──────────┬───────────┘          │
│               │                              │                      │
│               │ deploys                      │ deploys              │
│               ▼                              ▼                      │
│    ┌──────────────────────┐      ┌──────────────────────┐          │
│    │     Loan Token       │      │    Fund Manager      │          │
│    │    (Per Loan)        │      │    (Per Fund)        │          │
│    └──────────────────────┘      └──────────────────────┘          │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## Contract Summary

| Contract | Purpose | Instances |
|----------|---------|-----------|
| **Factory P2P** | Creates loan tokens | 1 (singleton) |
| **Factory Manager** | Creates fund contracts | 1 (singleton) |
| **Loan Token** | Represents loan ownership | 1 per loan |
| **Fund Manager** | Manages pooled investments | 1 per fund |

## Key Features

### Factory Contracts

- Deploy new token contracts
- Track all deployed contracts
- Provide registry functionality
- Admin-controlled deployment

### Loan Token Contracts

- ERC-20 compliant
- Investment functionality
- Distribution management
- Claim mechanism for holders

### Fund Manager Contracts

- Pooled investment management
- Investment allocation
- Fee distribution
- Manager operations

## Token Standard

All loan tokens follow the ERC-20 standard with extensions:

```solidity
// Standard ERC-20
function name() external view returns (string memory);
function symbol() external view returns (string memory);
function decimals() external view returns (uint8);
function totalSupply() external view returns (uint256);
function balanceOf(address account) external view returns (uint256);
function transfer(address to, uint256 amount) external returns (bool);
function approve(address spender, uint256 amount) external returns (bool);
function transferFrom(address from, address to, uint256 amount) external returns (bool);

// ReFlow Extensions
function invest(uint256 amount) external;
function claimDistribution() external;
function loanDetails() external view returns (LoanDetails memory);
```

## Deployed Addresses

### Mantle Sepolia Testnet

| Contract | Address |
|----------|---------|
| Mock USDT | `0xe01c5464816a544d4d0d6a336032578bd4629F10` |
| Factory P2P | `0xa411df45e20d266500363c76ecbf0b8e483fd408` |
| Factory Manager | `0x4d1a3d97109b2fb7e81023cf0a97aea4277d7235` |

## Security

### Implemented Measures

- Role-based access control
- Reentrancy protection
- Integer overflow protection (Solidity 0.8+)
- Input validation

### Audit Status

| Contract | Status |
|----------|--------|
| Factory P2P | Pending |
| Loan Token | Pending |
| Factory Manager | Pending |

## Related Pages

- [Deployed Contracts](deployed-contracts.md)
- [Contract Architecture](architecture.md)
- [Audit Reports](audits.md)
