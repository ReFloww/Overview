# Contract ABIs

Application Binary Interfaces for ReFlow contracts.

## Using ABIs

### Import

```typescript
import { erc20Abi } from 'viem';
import { factoryP2PAbi } from '@/lib/contracts/factoryP2P';
import { loanTokenAbi } from '@/lib/contracts/loanToken';
```

### With Viem

```typescript
const balance = await client.readContract({
  address: tokenAddress,
  abi: erc20Abi,
  functionName: 'balanceOf',
  args: [userAddress]
});
```

## ERC-20 ABI

Standard ERC-20 is available from Viem:

```typescript
import { erc20Abi } from 'viem';
```

## Loan Token ABI

```typescript
export const loanTokenAbi = [
  // ERC-20 Standard
  { name: 'name', type: 'function', stateMutability: 'view', inputs: [], outputs: [{ type: 'string' }] },
  { name: 'symbol', type: 'function', stateMutability: 'view', inputs: [], outputs: [{ type: 'string' }] },
  { name: 'decimals', type: 'function', stateMutability: 'view', inputs: [], outputs: [{ type: 'uint8' }] },
  { name: 'totalSupply', type: 'function', stateMutability: 'view', inputs: [], outputs: [{ type: 'uint256' }] },
  { name: 'balanceOf', type: 'function', stateMutability: 'view', inputs: [{ name: 'account', type: 'address' }], outputs: [{ type: 'uint256' }] },
  { name: 'transfer', type: 'function', stateMutability: 'nonpayable', inputs: [{ name: 'to', type: 'address' }, { name: 'amount', type: 'uint256' }], outputs: [{ type: 'bool' }] },
  { name: 'approve', type: 'function', stateMutability: 'nonpayable', inputs: [{ name: 'spender', type: 'address' }, { name: 'amount', type: 'uint256' }], outputs: [{ type: 'bool' }] },
  { name: 'allowance', type: 'function', stateMutability: 'view', inputs: [{ name: 'owner', type: 'address' }, { name: 'spender', type: 'address' }], outputs: [{ type: 'uint256' }] },
  { name: 'transferFrom', type: 'function', stateMutability: 'nonpayable', inputs: [{ name: 'from', type: 'address' }, { name: 'to', type: 'address' }, { name: 'amount', type: 'uint256' }], outputs: [{ type: 'bool' }] },

  // Loan Specific
  { name: 'principal', type: 'function', stateMutability: 'view', inputs: [], outputs: [{ type: 'uint256' }] },
  { name: 'interestRate', type: 'function', stateMutability: 'view', inputs: [], outputs: [{ type: 'uint256' }] },
  { name: 'duration', type: 'function', stateMutability: 'view', inputs: [], outputs: [{ type: 'uint256' }] },
  { name: 'maturityDate', type: 'function', stateMutability: 'view', inputs: [], outputs: [{ type: 'uint256' }] },
  { name: 'invest', type: 'function', stateMutability: 'nonpayable', inputs: [{ name: 'amount', type: 'uint256' }], outputs: [] },
  { name: 'claimDistribution', type: 'function', stateMutability: 'nonpayable', inputs: [], outputs: [] },
  { name: 'pendingDistribution', type: 'function', stateMutability: 'view', inputs: [{ name: 'holder', type: 'address' }], outputs: [{ type: 'uint256' }] },

  // Events
  { name: 'Investment', type: 'event', inputs: [{ name: 'investor', type: 'address', indexed: true }, { name: 'amount', type: 'uint256', indexed: false }] },
  { name: 'Distribution', type: 'event', inputs: [{ name: 'amount', type: 'uint256', indexed: false }, { name: 'perToken', type: 'uint256', indexed: false }] },
  { name: 'Claimed', type: 'event', inputs: [{ name: 'holder', type: 'address', indexed: true }, { name: 'amount', type: 'uint256', indexed: false }] },
  { name: 'Transfer', type: 'event', inputs: [{ name: 'from', type: 'address', indexed: true }, { name: 'to', type: 'address', indexed: true }, { name: 'value', type: 'uint256', indexed: false }] },
  { name: 'Approval', type: 'event', inputs: [{ name: 'owner', type: 'address', indexed: true }, { name: 'spender', type: 'address', indexed: true }, { name: 'value', type: 'uint256', indexed: false }] }
] as const;
```

## Factory P2P ABI

```typescript
export const factoryP2PAbi = [
  { name: 'getAllLoans', type: 'function', stateMutability: 'view', inputs: [], outputs: [{ type: 'address[]' }] },
  { name: 'getLoan', type: 'function', stateMutability: 'view', inputs: [{ name: 'index', type: 'uint256' }], outputs: [{ type: 'address' }] },
  { name: 'loanCount', type: 'function', stateMutability: 'view', inputs: [], outputs: [{ type: 'uint256' }] },
  { name: 'isLoan', type: 'function', stateMutability: 'view', inputs: [{ name: 'addr', type: 'address' }], outputs: [{ type: 'bool' }] },
  { name: 'stablecoin', type: 'function', stateMutability: 'view', inputs: [], outputs: [{ type: 'address' }] },
  { name: 'LoanCreated', type: 'event', inputs: [{ name: 'loanToken', type: 'address', indexed: true }, { name: 'name', type: 'string', indexed: false }, { name: 'principal', type: 'uint256', indexed: false }] }
] as const;
```

## Type Generation

Use `viem` for type inference:

```typescript
import { Abi } from 'viem';

// Types are automatically inferred
const result = await client.readContract({
  address,
  abi: loanTokenAbi,
  functionName: 'balanceOf', // Autocomplete works
  args: [userAddress]        // Types checked
});
// result is typed as bigint
```
