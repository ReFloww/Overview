# Getting Test MNT

Guide to obtaining test MNT tokens for development and testing on Mantle Sepolia.

## What is Test MNT?

Test MNT is the native token on Mantle Sepolia testnet. You need it to:
- Pay for gas fees
- Test transactions
- Deploy contracts
- Interact with dApps

**Note:** Test MNT has no real value and cannot be converted to mainnet MNT.

## Official Faucet

### Mantle Sepolia Faucet

**URL:** [https://faucet.sepolia.mantle.xyz](https://faucet.sepolia.mantle.xyz)

### Steps

1. **Visit Faucet**
   - Go to faucet.sepolia.mantle.xyz

2. **Connect Wallet**
   - Click "Connect Wallet"
   - Select your wallet
   - Approve connection

3. **Request Tokens**
   - Enter your wallet address (or use connected)
   - Complete any verification (captcha)
   - Click "Request"

4. **Receive Tokens**
   - Tokens arrive within minutes
   - Check wallet balance
   - Ready to use!

### Faucet Limits

| Limit | Value |
|-------|-------|
| Amount per request | ~0.5 MNT |
| Cooldown | 24 hours |
| Max requests | 1 per day |

## Alternative Methods

### Bridge from Sepolia ETH

If you have Sepolia ETH:

1. Get Sepolia ETH from a faucet
2. Use Mantle Bridge
3. Bridge to Mantle Sepolia

### Request from Team

For development purposes:
- Contact ReFlow team
- Provide wallet address
- Receive test tokens

## Checking Balance

### In Wallet

1. Open MetaMask
2. Ensure on Mantle Sepolia
3. View MNT balance

### Via Explorer

1. Go to [sepolia.mantlescan.xyz](https://sepolia.mantlescan.xyz)
2. Enter your address
3. View balance

### Via Code

```typescript
const balance = await publicClient.getBalance({
  address: userAddress
});

console.log('MNT Balance:', formatEther(balance));
```

## Getting Test USDT

For ReFlow testing, you also need test USDT:

### Mock USDT Contract

**Address:** `0xe01c5464816a544d4d0d6a336032578bd4629F10`

### Minting Test USDT

If the contract has a mint function:

```typescript
// Check if mint function available
await writeContract({
  address: '0xe01c5464816a544d4d0d6a336032578bd4629F10',
  abi: mockUsdtAbi,
  functionName: 'mint',
  args: [userAddress, parseUnits('10000', 6)]
});
```

### Request from Team

Contact ReFlow team for test USDT allocation.

## Troubleshooting

### Faucet Not Working

1. Check if address is correct
2. Ensure on Mantle Sepolia network
3. Wait for cooldown if recently used
4. Try different browser

### Tokens Not Appearing

1. Wait a few minutes
2. Check correct network
3. Refresh wallet
4. View on explorer

### Need More Tokens

1. Wait for cooldown reset
2. Use multiple addresses
3. Contact team for development needs

## Best Practices

### For Developers

- Keep test wallet separate from main
- Request tokens before you need them
- Don't waste on unnecessary transactions

### For Testing

- Start with small amounts
- Verify transactions on explorer
- Reset testnet account if needed

## Mainnet MNT

For mainnet:
- Purchase MNT from exchanges
- Bridge from Ethereum
- Check [Mantle documentation](https://docs.mantle.xyz)

## Related Pages

- [Network Configuration](network-config.md)
- [Explorer and Tools](explorer-tools.md)
- [Quick Start Guide](../getting-started/quick-start.md)
