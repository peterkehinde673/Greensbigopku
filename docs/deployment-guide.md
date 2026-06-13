# Greensbigopku (GBPK) Deployment Guide

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Environment Setup](#environment-setup)
- [Local Development](#local-development)
- [Canopy Testnet Deployment](#canopy-testnet-deployment)
- [Verification](#verification)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before deploying, ensure you have:

- **Node.js** (v14.0.0 or higher)
- **npm** or **yarn** package manager
- **Git** version control
- **Hardhat** for smart contract development
- **MetaMask** or similar Web3 wallet
- **Test funds** on Canopy Testnet (for gas fees)

### Installation

```bash
# Install Node.js from https://nodejs.org/
# Verify installation
node --version
npm --version

# Install Hardhat globally (optional)
npm install -g hardhat
```

## Environment Setup

### 1. Clone the Repository

```bash
git clone https://github.com/peterkehinde673/Greensbigopku.git
cd Greensbigopku
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Configure Environment Variables

Create a `.env` file in the project root:

```bash
cp .env.example .env
```

Edit `.env` with your configuration:

```env
# Private key of your deployment account
PRIVATE_KEY=your_private_key_here

# Canopy Testnet RPC endpoint
CANOPY_TESTNET_RPC=https://rpc.canopy.testnet.io

# Optional: Etherscan-like API key for verification
ETHERSCAN_API_KEY=your_etherscan_key

# Network IDs
CANOPY_TESTNET_CHAIN_ID=12345
```

⚠️ **Security Warning**: Never commit `.env` to version control. It's listed in `.gitignore` for protection.

## Local Development

### 1. Compile Smart Contracts

```bash
npm run compile
```

This will:
- Compile all Solidity contracts
- Generate ABI files
- Create contract artifacts

### 2. Run Tests

```bash
# Run all tests
npm run test

# Run tests with gas reporting
npm run test:gas

# Run tests with coverage
npm run test:coverage
```

Expected output:
```
GreensbigopkuToken
  ✓ should deploy successfully
  ✓ should have correct initial supply
  ✓ should transfer tokens correctly
  ✓ should burn tokens correctly
  ✓ should handle approvals
```

### 3. Local Hardhat Node

```bash
# Start local node
npx hardhat node

# In another terminal, deploy to local node
npx hardhat run scripts/deploy.js --network localhost
```

## Canopy Testnet Deployment

### 1. Prepare for Deployment

Ensure you have:

- ✓ Tested contracts locally
- ✓ `.env` configured correctly
- ✓ Sufficient testnet funds for gas
- ✓ Private key safely stored

### 2. Deploy to Canopy Testnet

```bash
# Deploy contract
npx hardhat run scripts/deploy.js --network canopy-testnet
```

Expected output:
```
Deploying Greensbigopku token...
Deployment transaction hash: 0x1234...
Contract deployed at: 0xabcd...
Token Name: Greensbigopku
Token Symbol: GBPK
Total Supply: 1000000 GBPK
```

### 3. Save Deployment Information

Record the deployment details:

```
Network: Canopy Testnet
Contract Address: 0x1234567890123456789012345678901234567890
Deployer: 0xYourAddress
Block Number: 12345
Timestamp: 2026-06-13 12:34:56 UTC
```

### 4. Fund Your Contract (if needed)

```bash
npx hardhat run scripts/fund.js --network canopy-testnet
```

## Verification

### 1. Check Contract Deployment

```bash
# Query contract details
npx hardhat run scripts/verify.js --network canopy-testnet
```

### 2. Verify on Block Explorer

1. Go to [Canopy Testnet Block Explorer](https://testnet.canopy.io)
2. Paste your contract address
3. Verify contract information is displayed correctly

### 3. Test Token Functionality

```bash
# Check token supply
npx hardhat run scripts/checkSupply.js --network canopy-testnet

# Check account balance
npx hardhat run scripts/balance.js --network canopy-testnet

# Transfer tokens
npx hardhat run scripts/transfer.js --network canopy-testnet
```

### 4. Add Token to MetaMask

1. Open MetaMask
2. Switch to Canopy Testnet network
3. Click "Import Tokens"
4. Enter contract address: `0x...`
5. Token should appear in your wallet

## Troubleshooting

### Common Issues

#### Issue: "Insufficient funds for gas"

```
Error: sender doesn't have enough funds to send tx
```

**Solution**: 
- Get testnet funds from [Canopy Testnet Faucet](https://faucet.canopy.io)
- Wait for confirmation
- Retry deployment

#### Issue: "Invalid private key"

```
Error: invalid private key
```

**Solution**:
- Check `.env` file for correct private key format
- Ensure private key doesn't have `0x` prefix in `.env`
- Verify it's a valid 64-character hex string

#### Issue: "Network connection failed"

```
Error: failed to fetch from RPC endpoint
```

**Solution**:
- Check RPC endpoint in `.env`
- Verify internet connection
- Try alternative RPC endpoint
- Check Canopy network status

#### Issue: "Contract already deployed"

```
Error: Contract already exists at this address
```

**Solution**:
- Use a different deployment account
- Deploy to different chain/testnet
- Increment contract version

### Debug Mode

Run with verbose output:

```bash
npx hardhat run scripts/deploy.js --network canopy-testnet --verbose
```

Enable debugging:

```bash
DEBUG=* npx hardhat run scripts/deploy.js --network canopy-testnet
```

## Post-Deployment

### 1. Update Documentation

Update `token-information.md` with:
- Contract address
- Deployment block number
- Token supply details
- Holder information

### 2. Announce Deployment

- Update README.md with contract address
- Create GitHub release
- Post in community channels

### 3. Monitor Contract

```bash
# Check transaction history
npx hardhat run scripts/getTxHistory.js --network canopy-testnet

# Monitor token transfers
npx hardhat run scripts/watchTransfers.js --network canopy-testnet
```

## Next Steps

- [ ] Verify contract on block explorer
- [ ] Add token to community platforms
- [ ] Conduct security audit
- [ ] Plan mainnet deployment
- [ ] Establish governance structure

---

## Additional Resources

- [Hardhat Documentation](https://hardhat.org/docs)
- [OpenZeppelin Contracts](https://docs.openzeppelin.com/contracts/)
- [Canopy Documentation](https://docs.canopy.io)
- [Solidity Documentation](https://docs.soliditylang.org/)

**Last Updated**: June 13, 2026
**Version**: 1.0.0