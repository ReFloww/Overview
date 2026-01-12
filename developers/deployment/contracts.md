# Smart Contract Deployment

Deploy ReFlow contracts to Mantle Network.

## Prerequisites

- Funded deployer wallet
- Hardhat or Foundry configured
- Verified contract source code

## Foundry Deployment

### Configure

```toml
# foundry.toml
[rpc_endpoints]
mantle = "https://rpc.mantle.xyz"
mantle_sepolia = "https://rpc.sepolia.mantle.xyz"
```

### Deploy

```bash
# Testnet
forge script script/Deploy.s.sol --rpc-url mantle_sepolia --broadcast

# Mainnet
forge script script/Deploy.s.sol --rpc-url mantle --broadcast
```

### Verify

```bash
forge verify-contract <address> src/LoanToken.sol:LoanToken \
  --chain-id 5000 \
  --etherscan-api-key $MANTLESCAN_API_KEY
```

## Hardhat Deployment

### Configure

```javascript
// hardhat.config.js
module.exports = {
  networks: {
    mantle: {
      url: 'https://rpc.mantle.xyz',
      chainId: 5000,
      accounts: [process.env.PRIVATE_KEY]
    }
  }
};
```

### Deploy

```bash
npx hardhat run scripts/deploy.js --network mantle
```

### Verify

```bash
npx hardhat verify --network mantle <address> <constructor-args>
```

## Deployment Order

1. Deploy Factory P2P
2. Deploy Factory Manager
3. Configure with USDT address
4. Verify all contracts

## Post-Deployment

1. Verify on Mantlescan
2. Update contract addresses in config
3. Test all functions
4. Transfer ownership if needed

## Contract Addresses

Update after deployment:

| Contract | Mainnet | Testnet |
|----------|---------|---------|
| USDT | TBD | 0xe01c... |
| Factory P2P | TBD | 0xa411... |
| Factory Manager | TBD | 0x4d1a... |
