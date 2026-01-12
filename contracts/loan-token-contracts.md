# Loan Token Contracts

Documentation for ReFlow's ERC-20 loan token contracts.

## Overview

Each loan on ReFlow has a dedicated ERC-20 token contract. These tokens represent fractional ownership of the underlying loan.

## Token Properties

| Property | Description |
|----------|-------------|
| **Standard** | ERC-20 |
| **Decimals** | 18 (standard) |
| **Supply** | Equal to loan principal |
| **Transferable** | Yes (secondary market) |

## Interface

```solidity
interface ILoanToken {
    // ===================
    // ERC-20 Standard
    // ===================
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);

    // ===================
    // Loan Information
    // ===================
    function principal() external view returns (uint256);
    function interestRate() external view returns (uint256);
    function duration() external view returns (uint256);
    function maturityDate() external view returns (uint256);
    function stablecoin() external view returns (address);

    // ===================
    // Investment
    // ===================
    function invest(uint256 amount) external;
    function totalInvested() external view returns (uint256);

    // ===================
    // Distributions
    // ===================
    function distribute(uint256 amount) external;
    function claimDistribution() external;
    function pendingDistribution(address holder) external view returns (uint256);
    function totalDistributed() external view returns (uint256);

    // ===================
    // Events
    // ===================
    event Investment(address indexed investor, uint256 amount);
    event Distribution(uint256 amount, uint256 perToken);
    event Claimed(address indexed holder, uint256 amount);
}
```

## Core Functions

### invest

Invest in the loan by purchasing tokens.

```solidity
function invest(uint256 amount) external
```

**Process:**
1. Caller must have approved USDT spending
2. USDT transferred from caller to contract
3. Loan tokens minted to caller
4. Investment event emitted

**Example:**
```typescript
// First approve USDT spending
await writeContract({
  address: usdtAddress,
  abi: erc20Abi,
  functionName: 'approve',
  args: [loanTokenAddress, amount]
});

// Then invest
await writeContract({
  address: loanTokenAddress,
  abi: loanTokenAbi,
  functionName: 'invest',
  args: [amount]
});
```

### distribute

Distribute repayment to all token holders (admin only).

```solidity
function distribute(uint256 amount) external onlyOwner
```

**Process:**
1. USDT transferred from caller to contract
2. Distribution per token calculated
3. Distribution event emitted
4. Holders can now claim

### claimDistribution

Claim pending distribution.

```solidity
function claimDistribution() external
```

**Process:**
1. Pending amount calculated
2. USDT transferred to caller
3. Claimed tracking updated
4. Claimed event emitted

### pendingDistribution

View pending distribution for an address.

```solidity
function pendingDistribution(address holder) external view returns (uint256)
```

**Calculation:**
```
pending = balance × (currentDistPerToken - claimedDistPerToken) / 1e18
```

## State Variables

### Loan Parameters

```solidity
uint256 public principal;      // Total loan amount
uint256 public interestRate;   // Annual rate (basis points)
uint256 public duration;       // Duration in seconds
uint256 public maturityDate;   // Maturity timestamp
address public stablecoin;     // USDT address
```

### Investment Tracking

```solidity
uint256 public totalInvested;  // Total USDT invested
```

### Distribution Tracking

```solidity
uint256 public totalDistributed;     // Total distributed
uint256 public distributionPerToken; // Cumulative per token
mapping(address => uint256) public claimedDistribution; // Per holder
```

## Events

### Investment

```solidity
event Investment(address indexed investor, uint256 amount);
```

Emitted when someone invests in the loan.

### Distribution

```solidity
event Distribution(uint256 amount, uint256 perToken);
```

Emitted when admin distributes repayment.

### Claimed

```solidity
event Claimed(address indexed holder, uint256 amount);
```

Emitted when holder claims their distribution.

## Usage Examples

### Reading Loan Details

```typescript
const principal = await client.readContract({
  address: loanTokenAddress,
  abi: loanTokenAbi,
  functionName: 'principal'
});

const interestRate = await client.readContract({
  address: loanTokenAddress,
  abi: loanTokenAbi,
  functionName: 'interestRate'
});
```

### Checking Balance

```typescript
const balance = await client.readContract({
  address: loanTokenAddress,
  abi: loanTokenAbi,
  functionName: 'balanceOf',
  args: [userAddress]
});
```

### Checking Pending Distribution

```typescript
const pending = await client.readContract({
  address: loanTokenAddress,
  abi: loanTokenAbi,
  functionName: 'pendingDistribution',
  args: [userAddress]
});
```

### Claiming Distribution

```typescript
await writeContract({
  address: loanTokenAddress,
  abi: loanTokenAbi,
  functionName: 'claimDistribution'
});
```

## Security Features

### Access Control

| Function | Access |
|----------|--------|
| invest | Public (pre-maturity) |
| distribute | Owner only |
| claimDistribution | Public |
| transfer | Public (ERC-20) |

### Protection Mechanisms

- Reentrancy guard on all state-changing functions
- Integer overflow protection (Solidity 0.8+)
- Balance checks before transfers
- Maturity date enforcement

## Related Pages

- [Factory Contracts](factory-contracts.md)
- [Contract Interactions](interactions.md)
- [Events and Logs](events.md)
