# Troubleshooting

Solutions to common issues on ReFlow.

## Wallet Connection

### Wallet won't connect

**Symptoms:** Click "Connect Wallet" but nothing happens or connection fails.

**Solutions:**

1. **Check browser extension**
   - Ensure wallet extension is installed and unlocked
   - Try refreshing the page

2. **Clear browser cache**
   - Clear cookies and cache for the ReFlow site
   - Try in incognito/private mode

3. **Check network**
   - Ensure wallet is connected to Mantle Network
   - Switch networks and switch back

4. **Try different wallet**
   - Use WalletConnect with mobile wallet
   - Try a different browser

### Wrong network

**Symptoms:** "Wrong network" error or transactions fail.

**Solutions:**

1. Switch to Mantle Network in your wallet
2. Add Mantle Network manually if not available:
   - Network Name: Mantle
   - RPC URL: https://rpc.mantle.xyz
   - Chain ID: 5000
   - Symbol: MNT
   - Explorer: https://mantlescan.xyz

## Transactions

### Transaction stuck pending

**Symptoms:** Transaction shows "pending" for extended time.

**Solutions:**

1. **Wait longer**
   - Network congestion can cause delays
   - Check transaction on block explorer

2. **Speed up transaction**
   - In MetaMask, click "Speed Up"
   - Increase gas price

3. **Cancel and retry**
   - Cancel pending transaction
   - Submit new transaction with higher gas

### Transaction failed

**Symptoms:** Transaction shows "failed" status.

**Solutions:**

1. **Check error message**
   - "Out of gas" - Increase gas limit
   - "Insufficient funds" - Add more USDT or MNT
   - "Reverted" - Check contract conditions

2. **Common fixes**
   - Ensure sufficient MNT for gas fees
   - Verify USDT approval is complete
   - Check loan availability (may be fully funded)

### Insufficient gas

**Symptoms:** "Insufficient funds for gas" error.

**Solutions:**

1. Get MNT tokens for gas fees
2. Bridge MNT from Ethereum using Mantle Bridge
3. Ensure wallet has at least 0.01 MNT

## Investments

### Investment not showing

**Symptoms:** Completed investment but not visible in portfolio.

**Solutions:**

1. **Wait for confirmation**
   - Transactions need block confirmations
   - Refresh page after a few minutes

2. **Check transaction status**
   - Verify transaction succeeded on block explorer
   - Look for token transfer in wallet history

3. **Add token to wallet**
   - Copy loan token contract address
   - Import token in wallet settings

### Cannot invest in loan

**Symptoms:** "Invest" button disabled or error when trying to invest.

**Solutions:**

1. **Check loan status**
   - Loan may be fully funded
   - Loan may have ended

2. **Check your balance**
   - Ensure sufficient USDT balance
   - Verify USDT is on Mantle Network

3. **Check minimum investment**
   - Amount may be below minimum
   - Amount may exceed remaining capacity

### USDT approval issues

**Symptoms:** Prompted for approval repeatedly or approval fails.

**Solutions:**

1. **Complete approval transaction**
   - First transaction approves spending
   - Wait for confirmation before investing

2. **Reset approval**
   - Revoke existing approval
   - Submit new approval transaction

## Secondary Market

### Listing not appearing

**Symptoms:** Created listing but not visible on marketplace.

**Solutions:**

1. Wait for transaction confirmation
2. Refresh the marketplace page
3. Check "My Listings" in portfolio
4. Verify transaction succeeded on explorer

### Cannot buy listing

**Symptoms:** Error when trying to purchase listing.

**Solutions:**

1. **Listing sold**
   - Someone else may have purchased first
   - Try a different listing

2. **Insufficient balance**
   - Check USDT balance covers price + gas
   - Get more USDT if needed

3. **Approval needed**
   - Approve USDT spending first
   - Complete approval transaction

### Cannot cancel listing

**Symptoms:** Unable to remove listing from market.

**Solutions:**

1. Ensure you're connected with the correct wallet
2. Check if listing is still active (not sold)
3. Try refreshing and reconnecting wallet

## Display Issues

### Balances not updating

**Symptoms:** Portfolio shows old balance or wrong values.

**Solutions:**

1. Refresh the page
2. Clear browser cache
3. Check wallet is on correct network
4. Wait for API to sync (up to 1 minute)

### Page not loading

**Symptoms:** Blank page or loading spinner.

**Solutions:**

1. Clear browser cache and cookies
2. Disable browser extensions
3. Try different browser
4. Check internet connection
5. Verify site is operational (check status page)

### Mobile display issues

**Symptoms:** Layout broken on mobile device.

**Solutions:**

1. Use latest version of mobile browser
2. Try requesting desktop site
3. Use mobile wallet's built-in browser

## Account Issues

### KYC verification failed

**Symptoms:** Identity verification not approved.

**Solutions:**

1. Ensure documents are clear and readable
2. Use government-issued ID
3. Make sure face is clearly visible in selfie
4. Contact support for manual review

### Cannot access account

**Symptoms:** Wallet connected but features unavailable.

**Solutions:**

1. Complete KYC verification if pending
2. Check if account is restricted
3. Contact support for account status

## Getting Help

### Before contacting support

1. Note the error message exactly
2. Copy transaction hash if applicable
3. Screenshot the issue
4. Note your wallet address (public)

### Contact support

- Email: support@reflow.io
- Discord: discord.gg/reflow
- Include: wallet address, transaction hash, screenshots

### Emergency

For urgent security issues:
- Email: security@reflow.io
- Do not share private keys
