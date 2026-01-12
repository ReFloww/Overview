# Connecting Your Wallet

Your crypto wallet is your gateway to ReFlow. This guide covers wallet setup and connection in detail.

## What You Need

### A Compatible Wallet

ReFlow supports wallets compatible with WalletConnect and Web3:

| Wallet | Best For | Get It |
|--------|----------|--------|
| MetaMask | Browser users | metamask.io |
| Rainbow | Mobile users | rainbow.me |
| Coinbase Wallet | Coinbase ecosystem | coinbase.com/wallet |
| Trust Wallet | Multi-chain mobile | trustwallet.com |

### Mantle Network Configuration

Add Mantle Network to your wallet:

| Parameter | Value |
|-----------|-------|
| Network Name | Mantle Sepolia (Testnet) |
| Chain ID | 5003 |
| RPC URL | https://rpc.sepolia.mantle.xyz |
| Currency Symbol | MNT |
| Block Explorer | https://sepolia.mantlescan.xyz |

## Setting Up MetaMask

### Step 1: Install MetaMask

1. Visit [metamask.io](https://metamask.io)
2. Download for your browser
3. Follow installation prompts
4. Create or import a wallet

### Step 2: Add Mantle Network

**Option A: Automatic (Recommended)**
1. Visit ReFlow
2. Click "Connect Wallet"
3. Approve network addition when prompted

**Option B: Manual**
1. Open MetaMask
2. Click network dropdown
3. Select "Add Network"
4. Enter Mantle Network details
5. Save

### Step 3: Get Tokens

**For Testnet:**
1. Visit [Mantle Faucet](https://faucet.sepolia.mantle.xyz/)
2. Enter your wallet address
3. Request test MNT

**For Mainnet:**
1. Purchase MNT from exchanges
2. Bridge USDT to Mantle Network
3. Ensure sufficient gas

## Connecting to ReFlow

### Step 1: Navigate to ReFlow

1. Open ReFlow in your browser
2. Ensure you're on the correct URL

### Step 2: Initiate Connection

1. Click **"Connect Wallet"** button
2. Select your wallet type
3. Choose your specific wallet

### Step 3: Approve in Wallet

1. **Connection Request**: Review and approve
2. **Network Switch**: Accept Mantle Network
3. **Signature Request**: Sign to authenticate

### Step 4: Confirm Connection

Once connected, you'll see:
- Wallet address in header
- Network indicator (Mantle)
- Account menu accessible

## Understanding Wallet Interactions

### Transaction Types

| Type | Purpose | Gas Required |
|------|---------|--------------|
| **Sign Message** | Authentication | No |
| **Approve Token** | Allow spending | Yes |
| **Transfer** | Move tokens | Yes |
| **Contract Call** | Execute functions | Yes |

### Common Transactions on ReFlow

1. **Login Signature** - Prove wallet ownership (free)
2. **USDT Approval** - Allow platform to use USDT (one-time gas)
3. **Investment** - Purchase loan tokens (gas required)
4. **Trade** - Buy/sell on secondary market (gas required)

## Security Best Practices

### Before Connecting

```
✓ Verify you're on the official ReFlow site
✓ Check for HTTPS and valid certificate
✓ Never enter seed phrase anywhere
✓ Beware of phishing attempts
```

### While Connected

```
✓ Review every transaction carefully
✓ Check amounts before approving
✓ Verify contract addresses
✓ Don't approve unlimited spending
```

### Account Management

```
✓ Use a dedicated investment wallet
✓ Consider hardware wallet for large amounts
✓ Regularly review connected sites
✓ Revoke unused permissions
```

## Troubleshooting

### "Wallet Not Detected"

1. Ensure wallet extension is installed
2. Refresh the browser page
3. Check if wallet is unlocked
4. Disable conflicting extensions

### "Wrong Network"

1. Open your wallet
2. Switch to Mantle Network
3. Refresh ReFlow page
4. Reconnect if necessary

### "Connection Failed"

1. Check internet connection
2. Clear browser cache
3. Try incognito mode
4. Use a different browser

### "Transaction Failed"

1. Check MNT balance for gas
2. Verify token balances
3. Increase gas limit if needed
4. Wait and retry if network busy

## Managing Connected Sites

### In MetaMask

1. Click three dots menu
2. Select "Connected sites"
3. Review connections
4. Disconnect if needed

### Security Audit

Periodically:
1. Review all connected dApps
2. Remove unused connections
3. Check token approvals
4. Revoke excessive permissions

## Multiple Wallets

### Using Different Wallets

- Each wallet = separate account
- Switch wallets in ReFlow to change accounts
- KYC required per wallet

### Hardware Wallet Integration

For enhanced security:
1. Connect hardware wallet to MetaMask
2. Use hardware wallet for transactions
3. Approve on device for each action

## Wallet Backup

### Essential Steps

1. **Seed Phrase**: Write down and store securely offline
2. **Private Keys**: Never share, never store digitally
3. **Recovery Test**: Verify you can restore wallet

### Storage Recommendations

| Do | Don't |
|----|-------|
| Metal backup plates | Photos on phone |
| Safe deposit box | Cloud storage |
| Multiple secure locations | Single copy |
| Memorize if possible | Share with anyone |

## Next Steps

After connecting your wallet:

1. [Complete KYC](kyc-verification.md) if not done
2. [Learn Investment Modes](investment-modes/README.md)
3. [Start with Quick Start Guide](../getting-started/quick-start.md)
