# Contract Architecture

Detailed documentation of ReFlow's smart contract architecture and design patterns.

## Design Principles

### 1. Factory Pattern

New loan and fund contracts are created through factory contracts:

```
Factory (Singleton) → deploys → Instance Contracts (Many)
```

**Benefits:**
- Standardized deployment
- Easy tracking of all instances
- Consistent initialization

### 2. Minimal Proxy (Optional)

For gas-efficient deployment:

```
Implementation (Logic) ← Proxy (Minimal) → delegatecall
```

### 3. Non-Custodial

User funds flow directly:
```
User Wallet → Smart Contract → User Wallet
(Never held by ReFlow)
```

## Contract Hierarchy

```
┌─────────────────────────────────────────────────────────────────────┐
│                        CONTRACT HIERARCHY                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Ownable (OpenZeppelin)                                            │
│       │                                                              │
│       ├── Factory P2P                                                │
│       │       └── creates → Loan Token (ERC20 + Custom)             │
│       │                                                              │
│       └── Factory Manager                                            │
│               └── creates → Fund Manager                             │
│                                                                      │
│   ERC20 (OpenZeppelin)                                              │
│       │                                                              │
│       └── Loan Token                                                 │
│               ├── Investment functions                               │
│               └── Distribution functions                             │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## Factory P2P Contract

### Purpose
Creates and tracks loan token contracts.

### State Variables

```solidity
contract FactoryP2P is Ownable {
    // Array of all deployed loan tokens
    address[] public loans;

    // Mapping for quick lookup
    mapping(address => bool) public isLoan;

    // Stable coin for investments
    address public stablecoin;
}
```

### Key Functions

```solidity
// Create new loan token
function createLoan(
    string memory name,
    string memory symbol,
    uint256 principal,
    uint256 interestRate,
    uint256 duration
) external onlyOwner returns (address);

// Get all loans
function getAllLoans() external view returns (address[] memory);

// Get loan count
function loanCount() external view returns (uint256);
```

### Events

```solidity
event LoanCreated(
    address indexed loanToken,
    string name,
    uint256 principal,
    uint256 interestRate,
    uint256 duration
);
```

## Loan Token Contract

### Purpose
Represents fractional ownership of a loan.

### State Variables

```solidity
contract LoanToken is ERC20, Ownable, ReentrancyGuard {
    // Loan parameters
    uint256 public principal;
    uint256 public interestRate;
    uint256 public duration;
    uint256 public maturityDate;

    // Investment tracking
    address public stablecoin;
    uint256 public totalInvested;

    // Distribution tracking
    uint256 public totalDistributed;
    uint256 public distributionPerToken;
    mapping(address => uint256) public claimedDistribution;
}
```

### Investment Functions

```solidity
// Invest in loan
function invest(uint256 amount) external nonReentrant {
    require(amount > 0, "Amount must be positive");
    require(block.timestamp < maturityDate, "Loan closed");

    // Transfer stablecoin from investor
    IERC20(stablecoin).transferFrom(msg.sender, address(this), amount);

    // Mint loan tokens
    _mint(msg.sender, amount);

    totalInvested += amount;

    emit Investment(msg.sender, amount);
}
```

### Distribution Functions

```solidity
// Called by admin to distribute repayments
function distribute(uint256 amount) external onlyOwner {
    require(totalSupply() > 0, "No token holders");

    IERC20(stablecoin).transferFrom(msg.sender, address(this), amount);

    distributionPerToken += (amount * 1e18) / totalSupply();
    totalDistributed += amount;

    emit Distribution(amount, distributionPerToken);
}

// Claim pending distribution
function claimDistribution() external nonReentrant {
    uint256 pending = pendingDistribution(msg.sender);
    require(pending > 0, "Nothing to claim");

    claimedDistribution[msg.sender] = distributionPerToken;

    IERC20(stablecoin).transfer(msg.sender, pending);

    emit Claimed(msg.sender, pending);
}

// View pending distribution
function pendingDistribution(address holder) public view returns (uint256) {
    uint256 owed = distributionPerToken - claimedDistribution[holder];
    return (balanceOf(holder) * owed) / 1e18;
}
```

## Fund Manager Contract

### Purpose
Manages pooled investments across multiple loans.

### Key Functions

```solidity
contract FundManager is Ownable, ReentrancyGuard {
    // Deposit to fund
    function deposit(uint256 amount) external;

    // Withdraw from fund
    function withdraw(uint256 amount) external;

    // Manager: Invest in loans
    function investInLoan(address loan, uint256 amount) external onlyOwner;

    // View fund value
    function totalValue() external view returns (uint256);

    // View user share
    function shareOf(address user) external view returns (uint256);
}
```

## Access Control

### Roles

| Role | Permissions |
|------|-------------|
| **Owner** | Create loans, distribute, admin functions |
| **User** | Invest, claim, transfer tokens |

### Modifiers

```solidity
modifier onlyOwner() {
    require(msg.sender == owner(), "Not owner");
    _;
}

modifier nonReentrant() {
    require(!_locked, "Reentrant");
    _locked = true;
    _;
    _locked = false;
}
```

## Security Features

### Reentrancy Protection

```solidity
// Using OpenZeppelin ReentrancyGuard
import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

function invest(uint256 amount) external nonReentrant {
    // Safe from reentrancy
}
```

### Safe Math

Solidity 0.8+ provides built-in overflow protection:

```solidity
uint256 total = a + b; // Reverts on overflow
```

### Input Validation

```solidity
function invest(uint256 amount) external {
    require(amount > 0, "Amount must be positive");
    require(amount <= maxInvestment, "Exceeds max");
    // ...
}
```

## Gas Optimization

### Techniques Used

| Technique | Implementation |
|-----------|---------------|
| Packed storage | Group small variables |
| Minimal proxies | For frequent deployments |
| Batch operations | Where applicable |
| Event usage | Instead of storage for history |

## Upgrade Considerations

### Current Design

- Loan tokens are immutable
- Factory can be versioned
- New features via new contracts

### Migration Path

If upgrades needed:
1. Deploy new factory
2. New loans use new factory
3. Old loans continue functioning

## Related Pages

- [Factory Contracts](factory-contracts.md)
- [Loan Token Contracts](loan-token-contracts.md)
- [Contract Interactions](interactions.md)
