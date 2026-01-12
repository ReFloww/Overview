# Event Listening

Guide to listening to smart contract events.

## Real-Time Events

### Using Wagmi Hook

```typescript
import { useWatchContractEvent } from 'wagmi';

function InvestmentWatcher({ loanAddress }) {
  useWatchContractEvent({
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

  return <div>Watching for investments...</div>;
}
```

### Using Viem Client

```typescript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  chain: mantleSepoliaTestnet,
  transport: http()
});

const unwatch = client.watchContractEvent({
  address: loanAddress,
  abi: loanTokenAbi,
  eventName: 'Investment',
  onLogs: (logs) => {
    console.log('Investment events:', logs);
  }
});

// Later: stop watching
unwatch();
```

## Historical Events

### Get Past Events

```typescript
const events = await client.getContractEvents({
  address: loanAddress,
  abi: loanTokenAbi,
  eventName: 'Investment',
  fromBlock: 0n,
  toBlock: 'latest'
});
```

### With Filters

```typescript
// Filter by investor
const myEvents = await client.getContractEvents({
  address: loanAddress,
  abi: loanTokenAbi,
  eventName: 'Investment',
  args: {
    investor: myAddress
  },
  fromBlock: 0n
});
```

### Block Range

```typescript
const currentBlock = await client.getBlockNumber();

const recentEvents = await client.getContractEvents({
  address: loanAddress,
  abi: loanTokenAbi,
  eventName: 'Investment',
  fromBlock: currentBlock - 10000n,
  toBlock: currentBlock
});
```

## Event Types

### Investment Event

```typescript
// Event signature
event Investment(address indexed investor, uint256 amount);

// Handling
onLogs: (logs) => {
  logs.forEach(log => {
    const { investor, amount } = log.args;
    // investor: address
    // amount: bigint
  });
}
```

### Distribution Event

```typescript
event Distribution(uint256 amount, uint256 perToken);

onLogs: (logs) => {
  logs.forEach(log => {
    const { amount, perToken } = log.args;
  });
}
```

### Transfer Event

```typescript
event Transfer(address indexed from, address indexed to, uint256 value);

onLogs: (logs) => {
  logs.forEach(log => {
    const { from, to, value } = log.args;
  });
}
```

## Processing Events

### Store in Database

```typescript
const events = await client.getContractEvents({ /* ... */ });

for (const event of events) {
  await db.investment.create({
    data: {
      investor: event.args.investor,
      amount: event.args.amount.toString(),
      txHash: event.transactionHash,
      blockNumber: Number(event.blockNumber)
    }
  });
}
```

### Update UI

```typescript
function InvestmentFeed({ loanAddress }) {
  const [investments, setInvestments] = useState([]);

  useWatchContractEvent({
    address: loanAddress,
    abi: loanTokenAbi,
    eventName: 'Investment',
    onLogs: (logs) => {
      setInvestments(prev => [...logs, ...prev]);
    }
  });

  return (
    <ul>
      {investments.map((inv, i) => (
        <li key={i}>
          {inv.args.investor} invested {formatUnits(inv.args.amount, 6)} USDT
        </li>
      ))}
    </ul>
  );
}
```

## Best Practices

1. **Cleanup**: Always call `unwatch()` when component unmounts
2. **Pagination**: For historical events, use block ranges
3. **Error Handling**: Handle RPC errors gracefully
4. **Debouncing**: Debounce rapid event updates for UI
