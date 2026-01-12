# Transaction Signing

Guide to sending transactions with ReFlow contracts.

## Basic Transactions

### Using Wagmi Hooks

```typescript
import { useWriteContract, useWaitForTransactionReceipt } from 'wagmi';

function InvestButton({ loanAddress, amount }) {
  const { writeContract, data: hash, isPending } = useWriteContract();

  const { isLoading: isConfirming, isSuccess } = useWaitForTransactionReceipt({
    hash
  });

  const handleInvest = () => {
    writeContract({
      address: loanAddress,
      abi: loanTokenAbi,
      functionName: 'invest',
      args: [amount]
    });
  };

  return (
    <button onClick={handleInvest} disabled={isPending || isConfirming}>
      {isPending ? 'Confirming...' : isConfirming ? 'Processing...' : 'Invest'}
    </button>
  );
}
```

## Common Transactions

### Approve Token Spending

```typescript
import { erc20Abi } from 'viem';

const { writeContract: approve } = useWriteContract();

await approve({
  address: usdtAddress,
  abi: erc20Abi,
  functionName: 'approve',
  args: [spenderAddress, amount]
});
```

### Invest in Loan

```typescript
await writeContract({
  address: loanAddress,
  abi: loanTokenAbi,
  functionName: 'invest',
  args: [parseUnits('1000', 6)] // 1000 USDT
});
```

### Claim Distribution

```typescript
await writeContract({
  address: loanAddress,
  abi: loanTokenAbi,
  functionName: 'claimDistribution'
});
```

### Transfer Tokens

```typescript
await writeContract({
  address: loanAddress,
  abi: loanTokenAbi,
  functionName: 'transfer',
  args: [recipientAddress, amount]
});
```

## Investment Flow

Complete investment with approval check:

```typescript
function useInvestment(loanAddress: string) {
  const { address } = useAccount();
  const { writeContractAsync } = useWriteContract();

  // Check allowance
  const { data: allowance } = useReadContract({
    address: USDT_ADDRESS,
    abi: erc20Abi,
    functionName: 'allowance',
    args: [address, loanAddress]
  });

  const invest = async (amount: bigint) => {
    // Approve if needed
    if (!allowance || allowance < amount) {
      await writeContractAsync({
        address: USDT_ADDRESS,
        abi: erc20Abi,
        functionName: 'approve',
        args: [loanAddress, amount]
      });
    }

    // Invest
    await writeContractAsync({
      address: loanAddress,
      abi: loanTokenAbi,
      functionName: 'invest',
      args: [amount]
    });
  };

  return { invest };
}
```

## Error Handling

```typescript
try {
  await writeContractAsync({
    address: loanAddress,
    abi: loanTokenAbi,
    functionName: 'invest',
    args: [amount]
  });
} catch (error) {
  if (error.message.includes('user rejected')) {
    // User rejected in wallet
  } else if (error.message.includes('insufficient funds')) {
    // Not enough gas
  } else {
    // Contract error
    console.error('Transaction failed:', error);
  }
}
```

## Gas Estimation

```typescript
import { useEstimateGas } from 'wagmi';

const { data: gasEstimate } = useEstimateGas({
  address: loanAddress,
  abi: loanTokenAbi,
  functionName: 'invest',
  args: [amount]
});

// Use estimate
await writeContract({
  // ...
  gas: gasEstimate
});
```

## Wait for Confirmation

```typescript
const { data: hash } = useWriteContract();

const { isLoading, isSuccess, data: receipt } = useWaitForTransactionReceipt({
  hash,
  confirmations: 1
});

if (isSuccess) {
  console.log('Transaction confirmed:', receipt.transactionHash);
}
```
