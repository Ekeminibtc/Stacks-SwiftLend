# Stacks-SwiftLend

This is a **Clarity** smart contract for a **Flash Loan Protocol** with **Governance** features, designed for the Stacks blockchain.  
It enables users to **deposit**, **borrow via flash loans**, **govern protocol settings**, and **earn fees** while maintaining strict **user validation**, **contract whitelisting**, and **security checks**.

---

## Features

- 🏦 **Flash Loans** (Instant borrowing with fee repayment within one transaction)
- 🛡️ **Governance Mechanism** (Create proposals, vote, execute changes)
- 🔐 **User and Contract Validation** (Only validated users/contracts can interact)
- 🔥 **Whitelisting and Borrowing Limits** (Tightly control access and risk)
- 💰 **Deposits and Withdrawals** (Deposit and withdraw supported tokens)
- 📈 **Fee Revenue Accumulation** (Protocol earns flash loan fees)

---

## Contract Constants

| Constant | Description |
|:---------|:------------|
| `CONTRACT-ADMINISTRATOR` | Address of admin set at deployment (tx-sender) |
| `flash-loan-fee-basis-points` | 5 basis points (0.05%) flash loan fee |
| `governance-timelock-duration` | 24 hours (in blocks) for executing proposals |
| Various `ERROR-*` constants | Meaningful error codes for easier debugging and audits |

---

## Tokens

- **Governance Token (`governance-token`)**  
Used for voting, proposal creation, and governance decisions.

- **Flash Loan Token (`flash-loan-token`)**  
The token available for flash loans and deposit operations.

---

## Data Storage

- `user-token-balances`: Maps a user's balance for each supported token.
- `whitelisted-addresses`: Tracks addresses eligible for flash loans.
- `governance-proposals`: Stores proposal metadata and voting counts.
- `user-proposal-votes`: Prevents double voting per proposal.
- `user-borrowing-limits`: Limits maximum borrowing per user.
- `supported-token-contracts`: Whitelisted tokens usable in the protocol.
- `validated-users` & `validated-contracts`: Control user/contract access.

---

## Public Functions

### 1. Token Operations
- **`mint-governance-tokens`**: Mint governance tokens (admin controlled).
- **`mint-flash-loan-tokens`**: Mint flash loan tokens.
- **`deposit-tokens`**: Deposit tokens into the protocol.
- **`withdraw-tokens`**: Withdraw tokens from the protocol.

### 2. Flash Loans
- **`execute-flash-loan`**: Borrow tokens instantly, repay with fee within the same transaction.
- **`repay-flash-loan`**: Repay borrowed amount + fee.

### 3. Governance
- **`create-governance-proposal`**: Propose a protocol change (requires 100M governance tokens).
- **`vote-on-governance-proposal`**: Vote for or against a proposal.
- **`execute-governance-proposal`**: Execute a successful proposal after timelock expiry.

### 4. Administration (Admin Only)
- **`set-user-borrowing-limit`**: Set the borrowing cap for a user.
- **`add-supported-token-contract`**: Whitelist a new token for deposits/loans.

---

## Read-Only Functions

- **`get-user-token-balance`**: View a user's token balance.
- **`get-total-protocol-liquidity`**: Total liquidity in the protocol.
- **`get-current-flash-loan-fee`**: Current fee basis points.
- **`is-address-whitelisted`**: Check if address is flash-loan eligible.
- **`get-governance-proposal-details`**: Retrieve proposal info.
- **`get-user-borrowing-limit`**: View user's borrowing limit.
- **`get-contract-token-balance`**: Contract's balance of a specific token.

---

## Governance Flow

1. **Mint Governance Tokens** (for initial users if needed)
2. **Create Proposal** (Requires minimum 100M governance tokens)
3. **Vote** (before timelock expiry)
4. **After Timelock Expiry**, if votes in favor > votes against:
   - Execute the proposal

---

## Flash Loan Flow

1. **Deposit Tokens** into the protocol.
2. **Whitelist** your address (admin action).
3. **Set Borrowing Limit** (admin action).
4. **Execute Flash Loan** — must repay **loan amount + fee** within the same transaction.
5. **Repay Flash Loan**.

---

## Events (for easier tracking)

- `token-deposit`
- `token-withdrawal`
- `flash-loan-executed`
- `flash-loan-repaid`
- `governance-proposal-created`
- `governance-vote-cast`
- `governance-proposal-executed`
- `user-borrowing-limit-set`
- `token-contract-supported`

---

## Security Checks & Safeguards

✅ Flash loan repayments must happen atomically.  
✅ Only whitelisted users with limits can borrow.  
✅ Governance changes require majority voting and a timelock.  
✅ Pausing functionality (via `is-protocol-paused`) for emergencies.  
✅ Borrowing amounts cannot exceed user-specific limits or available liquidity.  
✅ Flash loan fees increase protocol liquidity, benefitting stakers/depositors indirectly.

---

## Potential Future Enhancements

- **Slashing mechanism** for governance voters.
- **Dynamic fee model** based on utilization.
- **Multiple token support for flash loans** beyond initial token.
- **Insurance pool** for depositor protection.
- **Protocol pause/resume proposals** via governance instead of admin direct control.

---

## Requirements

- Clarity Language (Stacks 2.0+)
- Clarinet (for local testing and development)

---

## Quick Start

```bash
# Install Clarinet
npm install -g @hirosystems/clarinet

# Initialize and clone this contract
clarinet new flash-loan-protocol
cd flash-loan-protocol

# Add this contract to `contracts/`
# Then test
clarinet test
```

---
