# Fund Manager Contracts

Documentation for ReFlow's fund manager smart contracts used in Managed Mode.

## Overview

Fund Manager contracts enable pooled investment management, where a designated manager invests on behalf of depositors.

## Contract Structure

```
┌────────────────────────────────────────┐
│         Fund Manager Contract          │
├────────────────────────────────────────┤
│ • Accepts deposits from users          │
│ • Manager invests in loans             │
│ • Tracks proportional ownership        │
│ • Distributes returns                  │
│ • Handles withdrawals                  │
└────────────────────────────────────────┘
```

## Interface

```solidity
interface IFundManager {
    // ===================
    // Events
    // ===================
    event Deposit(address indexed user, uint256 amount, uint256 shares);
    event Withdraw(address indexed user, uint256 amount, uint256 shares);
    event InvestmentMade(address indexed loan, uint256 amount);
    event DistributionReceived(uint256 amount);

    // ===================
    // User Functions
    // ===================
    function deposit(uint256 amount) external;
    function withdraw(uint256 shares) external;
    function claimRewards() external;

    // ===================
    // Manager Functions
    // ===================
    function investInLoan(address loan, uint256 amount) external;
    function claimFromLoan(address loan) external;

    // ===================
    // View Functions
    // ===================
    function totalAssets() external view returns (uint256);
    function totalShares() external view returns (uint256);
    function shareOf(address user) external view returns (uint256);
    function shareValue(uint256 shares) external view returns (uint256);
    function pendingRewards(address user) external view returns (uint256);
    function manager() external view returns (address);
    function name() external view returns (string memory);
}
```

## Core Functions

### User Functions

#### deposit

Deposit stablecoin into the fund.

```solidity
function deposit(uint256 amount) external
```

**Process:**
1. Transfer USDT from user
2. Calculate shares based on current NAV
3. Mint shares to user
4. Emit Deposit event

**Share Calculation:**
```
If first deposit: shares = amount
Otherwise: shares = amount × totalShares / totalAssets
```

#### withdraw

Withdraw from the fund.

```solidity
function withdraw(uint256 shares) external
```

**Process:**
1. Calculate USDT value of shares
2. Burn user's shares
3. Transfer USDT to user
4. Emit Withdraw event

**Value Calculation:**
```
value = shares × totalAssets / totalShares
```

### Manager Functions

#### investInLoan

Manager invests fund assets in a loan.

```solidity
function investInLoan(address loan, uint256 amount) external onlyManager
```

**Process:**
1. Verify caller is manager
2. Approve USDT to loan contract
3. Call loan's invest function
4. Track investment

#### claimFromLoan

Claim distributions from a loan.

```solidity
function claimFromLoan(address loan) external onlyManager
```

**Process:**
1. Call loan's claimDistribution
2. Receive USDT into fund
3. Update fund value

## State Variables

```solidity
// Fund configuration
string public name;
address public manager;
address public stablecoin;

// Share tracking
uint256 public totalShares;
mapping(address => uint256) public shares;

// Investment tracking
address[] public investments;
mapping(address => uint256) public investedAmount;

// Reward tracking
uint256 public totalRewards;
uint256 public rewardsPerShare;
mapping(address => uint256) public claimedRewards;
```

## Share Mechanics

### How Shares Work

Shares represent proportional ownership of the fund:

```
Your Ownership = Your Shares / Total Shares
Your Value = Fund Total Assets × Your Ownership
```

### Example

```
Fund Stats:
- Total Assets: 100,000 USDT
- Total Shares: 100,000

User A:
- Shares: 10,000 (10%)
- Value: 10,000 USDT

User A deposits 5,000 USDT:
- New shares = 5,000 × 100,000 / 100,000 = 5,000
- User A total: 15,000 shares
- New total: 105,000 shares
- User A %: 14.3%
```

### NAV Calculation

Net Asset Value per share:

```
NAV = Total Assets / Total Shares
```

When profits are made:
```
NAV increases → Existing shares worth more
```

## Fee Handling

### Management Fee

```solidity
uint256 public managementFee; // Annual fee in basis points

function calculateManagementFee() internal view returns (uint256) {
    return (totalAssets * managementFee * timeElapsed) / (365 days * 10000);
}
```

### Performance Fee

```solidity
uint256 public performanceFee; // Fee on profits in basis points

function calculatePerformanceFee(uint256 profit) internal view returns (uint256) {
    return (profit * performanceFee) / 10000;
}
```

## Access Control

### Manager Role

```solidity
modifier onlyManager() {
    require(msg.sender == manager, "Not manager");
    _;
}
```

Manager can:
- Invest fund assets in loans
- Claim distributions from loans
- Execute fund strategy

Manager cannot:
- Withdraw user funds
- Change fund parameters
- Access restricted admin functions

### Owner Role

Owner can:
- Update manager address
- Pause/unpause fund
- Emergency functions

## Security Features

### Withdrawal Protection

```solidity
function withdraw(uint256 shares) external nonReentrant {
    require(shares > 0, "Invalid amount");
    require(shares <= balanceOf(msg.sender), "Insufficient shares");

    uint256 value = (shares * totalAssets()) / totalShares;
    require(liquidAssets() >= value, "Insufficient liquidity");

    // Process withdrawal...
}
```

### Investment Limits

```solidity
uint256 public maxSingleInvestment;
uint256 public maxTotalInvestment;

function investInLoan(address loan, uint256 amount) external onlyManager {
    require(amount <= maxSingleInvestment, "Exceeds single limit");
    require(totalInvested + amount <= maxTotalInvestment, "Exceeds total limit");
    // ...
}
```

## Usage Examples

### Depositing

```typescript
// Approve USDT
await writeContract({
  address: usdtAddress,
  abi: erc20Abi,
  functionName: 'approve',
  args: [fundAddress, amount]
});

// Deposit
await writeContract({
  address: fundAddress,
  abi: fundManagerAbi,
  functionName: 'deposit',
  args: [amount]
});
```

### Checking Value

```typescript
const shares = await client.readContract({
  address: fundAddress,
  abi: fundManagerAbi,
  functionName: 'shareOf',
  args: [userAddress]
});

const value = await client.readContract({
  address: fundAddress,
  abi: fundManagerAbi,
  functionName: 'shareValue',
  args: [shares]
});
```

## Related Pages

- [Factory Contracts](factory-contracts.md)
- [Managed Mode](../lenders/investment-modes/managed-mode.md)
- [Contract Interactions](interactions.md)
