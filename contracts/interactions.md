# Contract Interactions

Guide to interacting with ReFlow smart contracts programmatically.

## Setup

### Required Libraries

```typescript
// Using Viem (recommended)
import { createPublicClient, createWalletClient, http } from 'viem';
import { mantleSepoliaTestnet } from 'viem/chains';

// Client setup
const publicClient = createPublicClient({
  chain: mantleSepoliaTestnet,
  transport: http('https://rpc.sepolia.mantle.xyz')
});
```

### Contract Addresses

```typescript
const addresses = {
  usdt: '0xe01c5464816a544d4d0d6a336032578bd4629F10',
  factoryP2P: '0xa411df45e20d266500363c76ecbf0b8e483fd408',
  factoryManager: '0x4d1a3d97109b2fb7e81023cf0a97aea4277d7235'
} as const;
```

## Reading Data

### Get All Loans

```typescript
const loans = await publicClient.readContract({
  address: addresses.factoryP2P,
  abi: factoryP2PAbi,
  functionName: 'getAllLoans'
});

console.log('All loans:', loans);
```

### Get Loan Details

```typescript
const loanAddress = '0x...'; // Specific loan token address

// Read multiple properties
const [principal, interestRate, duration, maturityDate] = await Promise.all([
  publicClient.readContract({
    address: loanAddress,
    abi: loanTokenAbi,
    functionName: 'principal'
  }),
  publicClient.readContract({
    address: loanAddress,
    abi: loanTokenAbi,
    functionName: 'interestRate'
  }),
  publicClient.readContract({
    address: loanAddress,
    abi: loanTokenAbi,
    functionName: 'duration'
  }),
  publicClient.readContract({
    address: loanAddress,
    abi: loanTokenAbi,
    functionName: 'maturityDate'
  })
]);
```

### Get Token Balance

```typescript
const balance = await publicClient.readContract({
  address: loanAddress,
  abi: loanTokenAbi,
  functionName: 'balanceOf',
  args: [userAddress]
});

// Format balance (assuming 18 decimals)
const formatted = formatUnits(balance, 18);
```

### Get Pending Distribution

```typescript
const pending = await publicClient.readContract({
  address: loanAddress,
  abi: loanTokenAbi,
  functionName: 'pendingDistribution',
  args: [userAddress]
});
```

## Writing Transactions

### Approve Token Spending

```typescript
import { createWalletClient, http } from 'viem';
import { privateKeyToAccount } from 'viem/accounts';

const account = privateKeyToAccount('0x...');

const walletClient = createWalletClient({
  account,
  chain: mantleSepoliaTestnet,
  transport: http()
});

// Approve USDT spending
const approveTx = await walletClient.writeContract({
  address: addresses.usdt,
  abi: erc20Abi,
  functionName: 'approve',
  args: [loanAddress, parseUnits('1000', 6)] // 1000 USDT
});

// Wait for confirmation
await publicClient.waitForTransactionReceipt({ hash: approveTx });
```

### Invest in Loan

```typescript
// After approval
const investTx = await walletClient.writeContract({
  address: loanAddress,
  abi: loanTokenAbi,
  functionName: 'invest',
  args: [parseUnits('1000', 6)] // Amount in USDT
});

const receipt = await publicClient.waitForTransactionReceipt({ hash: investTx });
console.log('Investment confirmed:', receipt.transactionHash);
```

### Claim Distribution

```typescript
const claimTx = await walletClient.writeContract({
  address: loanAddress,
  abi: loanTokenAbi,
  functionName: 'claimDistribution'
});

await publicClient.waitForTransactionReceipt({ hash: claimTx });
```

### Transfer Tokens

```typescript
// Standard ERC-20 transfer
const transferTx = await walletClient.writeContract({
  address: loanAddress,
  abi: loanTokenAbi,
  functionName: 'transfer',
  args: [recipientAddress, amount]
});
```

## Event Listening

### Watch for Investments

```typescript
const unwatch = publicClient.watchContractEvent({
  address: loanAddress,
  abi: loanTokenAbi,
  eventName: 'Investment',
  onLogs: (logs) => {
    logs.forEach(log => {
      console.log('New investment:', {
        investor: log.args.investor,
        amount: log.args.amount
      });
    });
  }
});

// Stop watching
// unwatch();
```

### Watch for Distributions

```typescript
publicClient.watchContractEvent({
  address: loanAddress,
  abi: loanTokenAbi,
  eventName: 'Distribution',
  onLogs: (logs) => {
    logs.forEach(log => {
      console.log('Distribution:', {
        amount: log.args.amount,
        perToken: log.args.perToken
      });
    });
  }
});
```

### Get Historical Events

```typescript
const investmentEvents = await publicClient.getContractEvents({
  address: loanAddress,
  abi: loanTokenAbi,
  eventName: 'Investment',
  fromBlock: 0n,
  toBlock: 'latest'
});

console.log('All investments:', investmentEvents);
```

## Common Patterns

### Check Allowance Before Investing

```typescript
async function investWithApproval(loanAddress: string, amount: bigint) {
  // Check current allowance
  const allowance = await publicClient.readContract({
    address: addresses.usdt,
    abi: erc20Abi,
    functionName: 'allowance',
    args: [account.address, loanAddress]
  });

  // Approve if needed
  if (allowance < amount) {
    const approveTx = await walletClient.writeContract({
      address: addresses.usdt,
      abi: erc20Abi,
      functionName: 'approve',
      args: [loanAddress, amount]
    });
    await publicClient.waitForTransactionReceipt({ hash: approveTx });
  }

  // Invest
  const investTx = await walletClient.writeContract({
    address: loanAddress,
    abi: loanTokenAbi,
    functionName: 'invest',
    args: [amount]
  });

  return publicClient.waitForTransactionReceipt({ hash: investTx });
}
```

### Batch Read Multiple Loans

```typescript
async function getLoansSummary(loanAddresses: string[]) {
  const summaries = await Promise.all(
    loanAddresses.map(async (address) => {
      const [principal, rate, totalInvested] = await Promise.all([
        publicClient.readContract({
          address,
          abi: loanTokenAbi,
          functionName: 'principal'
        }),
        publicClient.readContract({
          address,
          abi: loanTokenAbi,
          functionName: 'interestRate'
        }),
        publicClient.readContract({
          address,
          abi: loanTokenAbi,
          functionName: 'totalInvested'
        })
      ]);

      return {
        address,
        principal,
        rate,
        totalInvested,
        available: principal - totalInvested
      };
    })
  );

  return summaries;
}
```

## Error Handling

### Common Errors

```typescript
try {
  await walletClient.writeContract({
    // ...
  });
} catch (error) {
  if (error.message.includes('insufficient funds')) {
    console.error('Not enough MNT for gas');
  } else if (error.message.includes('user rejected')) {
    console.error('User rejected transaction');
  } else if (error.message.includes('execution reverted')) {
    // Contract error
    console.error('Contract error:', error.message);
  }
}
```

### Decoding Contract Errors

```typescript
import { decodeErrorResult } from 'viem';

try {
  // ... transaction
} catch (error) {
  if (error.data) {
    const decoded = decodeErrorResult({
      abi: loanTokenAbi,
      data: error.data
    });
    console.error('Contract error:', decoded);
  }
}
```

## Related Pages

- [Contract ABIs](../developers/web3-integration/contract-abis.md)
- [Events and Logs](events.md)
- [Blockchain Layer](../technical/blockchain-layer.md)
