# Explorer and Tools

Guide to blockchain explorers and developer tools for Mantle Network.

## Block Explorers

### Mantlescan (Official)

The official block explorer for Mantle Network.

| Network | URL |
|---------|-----|
| Mainnet | [explorer.mantle.xyz](https://explorer.mantle.xyz) |
| Sepolia | [sepolia.mantlescan.xyz](https://sepolia.mantlescan.xyz) |

### Explorer Features

#### View Transactions

1. Go to explorer
2. Enter transaction hash
3. View:
   - Status (success/failed)
   - Block number
   - Gas used
   - Input data
   - Logs/events

#### View Addresses

1. Enter address in search
2. View:
   - Balance
   - Transactions
   - Token holdings
   - Contract code (if applicable)

#### View Contracts

1. Search contract address
2. Click "Contract" tab
3. View:
   - Verified source code
   - Read functions
   - Write functions
   - Events

### Verifying Contracts

1. Deploy contract
2. Go to explorer
3. Navigate to contract address
4. Click "Verify & Publish"
5. Enter:
   - Compiler version
   - Source code
   - Constructor arguments
6. Submit verification

## Development Tools

### Hardhat Configuration

```javascript
// hardhat.config.js
module.exports = {
  networks: {
    mantleSepolia: {
      url: 'https://rpc.sepolia.mantle.xyz',
      chainId: 5003,
      accounts: [process.env.PRIVATE_KEY]
    },
    mantleMainnet: {
      url: 'https://rpc.mantle.xyz',
      chainId: 5000,
      accounts: [process.env.PRIVATE_KEY]
    }
  }
};
```

### Foundry Configuration

```toml
# foundry.toml
[rpc_endpoints]
mantle_sepolia = "https://rpc.sepolia.mantle.xyz"
mantle_mainnet = "https://rpc.mantle.xyz"

[etherscan]
mantle_sepolia = { key = "${MANTLESCAN_API_KEY}", url = "https://api-sepolia.mantlescan.xyz/api" }
```

### Cast Commands

```bash
# Get balance
cast balance <address> --rpc-url https://rpc.sepolia.mantle.xyz

# Send transaction
cast send <to> --value 0.1ether --rpc-url https://rpc.sepolia.mantle.xyz

# Call contract
cast call <contract> "balanceOf(address)" <address> --rpc-url https://rpc.sepolia.mantle.xyz

# Get block
cast block latest --rpc-url https://rpc.sepolia.mantle.xyz
```

## API Access

### Mantlescan API

For programmatic access:

```
https://api-sepolia.mantlescan.xyz/api
```

#### Get Transaction

```
GET /api?module=proxy&action=eth_getTransactionByHash&txhash=0x...
```

#### Get Balance

```
GET /api?module=account&action=balance&address=0x...
```

#### Get Contract ABI

```
GET /api?module=contract&action=getabi&address=0x...
```

### Rate Limits

| Tier | Requests |
|------|----------|
| Free | 5 req/sec |
| Pro | Higher limits |

## Monitoring Tools

### Transaction Tracking

```typescript
// Watch for transaction confirmation
const receipt = await publicClient.waitForTransactionReceipt({
  hash: txHash,
  confirmations: 1
});

if (receipt.status === 'success') {
  console.log('Transaction confirmed!');
}
```

### Event Monitoring

```typescript
// Watch contract events
publicClient.watchContractEvent({
  address: contractAddress,
  abi: contractAbi,
  eventName: 'Transfer',
  onLogs: (logs) => {
    console.log('New transfer:', logs);
  }
});
```

## Useful Links

### Official Resources

| Resource | URL |
|----------|-----|
| Mantle Docs | [docs.mantle.xyz](https://docs.mantle.xyz) |
| Mantle Bridge | [bridge.mantle.xyz](https://bridge.mantle.xyz) |
| Faucet | [faucet.sepolia.mantle.xyz](https://faucet.sepolia.mantle.xyz) |

### Developer Resources

| Resource | URL |
|----------|-----|
| GitHub | [github.com/mantlenetworkio](https://github.com/mantlenetworkio) |
| Discord | Mantle Discord |
| Forum | Mantle Forum |

## Quick Reference

### Contract Addresses (Testnet)

| Contract | Address |
|----------|---------|
| Mock USDT | `0xe01c5464816a544d4d0d6a336032578bd4629F10` |
| Factory P2P | `0xa411df45e20d266500363c76ecbf0b8e483fd408` |
| Factory Manager | `0x4d1a3d97109b2fb7e81023cf0a97aea4277d7235` |

### Explorer Links

View these contracts:
- [Mock USDT](https://sepolia.mantlescan.xyz/address/0xe01c5464816a544d4d0d6a336032578bd4629F10)
- [Factory P2P](https://sepolia.mantlescan.xyz/address/0xa411df45e20d266500363c76ecbf0b8e483fd408)
- [Factory Manager](https://sepolia.mantlescan.xyz/address/0x4d1a3d97109b2fb7e81023cf0a97aea4277d7235)

## Related Pages

- [Network Configuration](network-config.md)
- [Getting Test MNT](getting-test-mnt.md)
- [Contract Interactions](../contracts/interactions.md)
