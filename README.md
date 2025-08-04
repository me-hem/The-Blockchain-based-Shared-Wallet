# **The Blockchain-based Shared Wallet**

A decentralized smart contract system that enables secure shared wallet functionality with allowance-based access control on the Ethereum blockchain.

## Overview

The Blockchain-based Shared Wallet is a smart contract solution that allows multiple users to share funds while maintaining strict access controls. The system implements an allowance mechanism where the wallet owner can grant specific spending limits to other addresses, enabling controlled access to shared funds.

## Features

- **Ownership Control**: Single owner with administrative privileges
- **Allowance System**: Grant spending permissions to specific addresses
- **Secure Withdrawals**: Users can only withdraw within their allocated allowance
- **Event Logging**: Comprehensive event tracking for all transactions
- **Fund Reception**: Easy deposit functionality through direct transfers
- **Access Control**: Robust permission system preventing unauthorized access

## Smart Contracts

### 1. Allowance.sol
Base contract that manages the allowance system and ownership controls.

**Key Functions:**
- `add_allowance(address _who, uint _amount)`: Owner sets spending allowance for an address
- `reduce_allowance(address _who, uint _amount)`: Internal function to decrease allowance after spending
- Custom ownership management with renouncement prevention

### 2. SharedWallet.sol (SimpleWallet.sol)
Main wallet contract that inherits from Allowance and implements core wallet functionality.

**Key Functions:**
- `withdraw_money(address payable _to, uint _amount)`: Withdraw funds with allowance checking
- `receive()`: Accept incoming Ether deposits

## Technology Stack

- **Solidity**: ^0.8.20
- **OpenZeppelin**: Access control and safe math utilities
- **Ethereum**: Blockchain platform

## Dependencies

```json
{
  "@openzeppelin/contracts": "^5.x"
}
```

## Getting Started

### Prerequisites

- Node.js (v14 or higher)
- npm or yarn
- Hardhat or Truffle development environment
- MetaMask or similar Web3 wallet

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd SmartWallet
```

2. Install dependencies:
```bash
npm install
```

3. Install OpenZeppelin contracts:
```bash
npm install @openzeppelin/contracts
```

### Compilation

```bash
npx hardhat compile
```

### Deployment

1. Configure your network settings in `hardhat.config.js`
2. Deploy the contracts:

```bash
npx hardhat run scripts/deploy.js --network <network-name>
```

## Usage

### For Wallet Owner

1. **Deploy the Contract**: Deploy SharedWallet with your address as the initial owner
2. **Add Funds**: Send Ether directly to the contract address
3. **Set Allowances**: Use `add_allowance()` to grant spending permissions
4. **Withdraw**: Withdraw any amount without restrictions

### For Allowance Holders

1. **Check Allowance**: View your spending limit using the `allowance` mapping
2. **Withdraw Funds**: Use `withdraw_money()` within your allowance limit
3. **Monitor Balance**: Your allowance decreases with each withdrawal

## Contract Interactions

### Setting an Allowance
```solidity
// Owner grants 1 ETH allowance to user
wallet.add_allowance(userAddress, 1000000000000000000); // 1 ETH in wei
```

### Withdrawing Funds
```solidity
// User withdraws 0.5 ETH
wallet.withdraw_money(recipientAddress, 500000000000000000); // 0.5 ETH in wei
```

### Depositing Funds
```solidity
// Simply send Ether to the contract address
// The receive() function will handle the deposit
```

## Security Features

- **Access Modifiers**: `onlyOwner` and `ownerOrAllowed` ensure proper authorization
- **SafeMath**: Prevents integer overflow/underflow vulnerabilities (though less critical in Solidity ^0.8.0)
- **Event Emission**: All transactions are logged for transparency
- **Ownership Protection**: Prevents accidental ownership renouncement

## Events

The contract emits the following events:

- `AllowanceChanged(address indexed _from, address indexed _to, uint _oldAmount, uint _newAmount)`
- `moneySend(address indexed _to, uint _amount)`
- `moneyReceived(address indexed _from, uint _amount)`

## Important Notes

1. **Owner Privileges**: The contract owner has unlimited withdrawal access
2. **Allowance Management**: Allowances are automatically reduced after withdrawals
3. **Gas Costs**: Consider gas fees for all transactions
4. **Network Security**: Always verify contract addresses before interacting

## Testing

Run the test suite:
```bash
npx hardhat test
```

Create comprehensive tests for:
- Allowance setting and reduction
- Withdrawal functionality
- Access control mechanisms
- Event emission
- Error handling

## Development

### Project Structure
```
SmartWallet/
├── .deps/                     # Dependencies cache
├── .states/                   # Compilation states
├── artifacts/                 # Compiled contract artifacts
│   └── build-info/           # Build information
│       ├── Allowance.json    # Allowance contract ABI and bytecode
│       ├── Allowance_metadata.json
│       ├── SimpleWallet.json  # SimpleWallet contract ABI and bytecode
│       └── SimpleWallet_metadata.json
├── Allowance.sol             # Base allowance management contract
├── SimpleWallet.sol          # Main shared wallet contract
└── README.md                 # Project documentation
```


## 📄 License

This project is licensed under the MIT License - see the contract headers for details.

