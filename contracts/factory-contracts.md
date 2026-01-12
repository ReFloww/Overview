# Factory Contracts

Documentation for ReFlow's factory contracts that deploy and manage loan tokens.

## Factory P2P

### Overview

The Factory P2P contract is responsible for creating new loan token contracts.

**Address (Testnet):** `0xa411df45e20d266500363c76ecbf0b8e483fd408`

### Interface

```solidity
interface IFactoryP2P {
    // Events
    event LoanCreated(
        address indexed loanToken,
        string name,
        string symbol,
        uint256 principal,
        uint256 interestRate,
        uint256 duration
    );

    // Admin Functions
    function createLoan(
        string memory name,
        string memory symbol,
        uint256 principal,
        uint256 interestRate,
        uint256 duration
    ) external returns (address);

    function setStablecoin(address _stablecoin) external;

    // View Functions
    function getAllLoans() external view returns (address[] memory);
    function getLoan(uint256 index) external view returns (address);
    function loanCount() external view returns (uint256);
    function isLoan(address addr) external view returns (bool);
    function stablecoin() external view returns (address);
}
```

### Functions

#### createLoan

Creates a new loan token contract.

```solidity
function createLoan(
    string memory name,
    string memory symbol,
    uint256 principal,
    uint256 interestRate,
    uint256 duration
) external onlyOwner returns (address)
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| name | string | Token name (e.g., "ReFlow Loan #1") |
| symbol | string | Token symbol (e.g., "RFL1") |
| principal | uint256 | Total loan amount |
| interestRate | uint256 | Annual interest rate (basis points) |
| duration | uint256 | Loan duration in seconds |

**Returns:** Address of the new loan token contract.

#### getAllLoans

Returns array of all deployed loan token addresses.

```solidity
function getAllLoans() external view returns (address[] memory)
```

#### loanCount

Returns total number of deployed loans.

```solidity
function loanCount() external view returns (uint256)
```

### Usage Example

```typescript
// Read all loans
const loans = await client.readContract({
  address: factoryAddress,
  abi: factoryP2PAbi,
  functionName: 'getAllLoans'
});

// Check if address is a valid loan
const isValid = await client.readContract({
  address: factoryAddress,
  abi: factoryP2PAbi,
  functionName: 'isLoan',
  args: [loanAddress]
});
```

## Factory Manager

### Overview

Creates and manages fund manager contracts for managed investing.

**Address (Testnet):** `0x4d1a3d97109b2fb7e81023cf0a97aea4277d7235`

### Interface

```solidity
interface IFactoryManager {
    // Events
    event FundCreated(
        address indexed fundManager,
        string name,
        address manager
    );

    // Admin Functions
    function createFund(
        string memory name,
        address manager
    ) external returns (address);

    // View Functions
    function getAllFunds() external view returns (address[] memory);
    function getFund(uint256 index) external view returns (address);
    function fundCount() external view returns (uint256);
    function isFund(address addr) external view returns (bool);
}
```

### Functions

#### createFund

Creates a new fund manager contract.

```solidity
function createFund(
    string memory name,
    address manager
) external onlyOwner returns (address)
```

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| name | string | Fund name |
| manager | address | Manager's wallet address |

**Returns:** Address of the new fund manager contract.

## Access Control

### Owner Functions

Only the contract owner can:
- Create new loans
- Create new funds
- Update configuration
- Emergency operations

### Public Functions

Anyone can:
- View loan list
- Check if address is loan/fund
- Read configuration

## Security Considerations

### Deployment Security

- Only owner can create loans
- Parameters validated on creation
- Events emitted for tracking

### Verification

All deployed contracts can be verified:
1. Check `isLoan()` function
2. Verify on block explorer
3. Cross-reference with factory events

## Related Pages

- [Loan Token Contracts](loan-token-contracts.md)
- [Fund Manager Contracts](fund-manager-contracts.md)
- [Contract Architecture](architecture.md)
