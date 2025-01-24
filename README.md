# DECENTRALIZED-ESCROW
A decentralized escrow uses smart contracts on a blockchain to securely hold funds until agreed conditions are met, without the need for a trusted intermediary. The process is automated and trustless, with funds released once conditions (like delivery or service completion) are verified.
# Escrow Contract

This is a smart contract for decentralized escrow services on the Ethereum blockchain. The contract allows a buyer and a worker to securely manage funds during a transaction. The buyer deposits funds, and the worker can complete the task. Once the task is completed, the buyer can release the funds. If the task isn't completed in time, the buyer can request a refund.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Contract Structure](#contract-structure)
- [Functions](#functions)
- [How to Use](#how-to-use)
- [License](#license)
- [Fork](#fork)

## Overview

This escrow contract ensures that funds are securely held until the buyer confirms that a task has been completed. If the task is not completed within a set time, the buyer can request a refund. The contract also protects against reentrancy attacks by using OpenZeppelin's `ReentrancyGuard`.

## Features

- **Deposit Funds**: The buyer deposits funds into the contract, locking the amount until the task is completed or refunded.
- **Task Completion**: The worker can mark the task as completed, starting a 3-day period for the buyer to confirm or request a refund.
- **Refund Mechanism**: If the task isn't completed within the specified time frame, the buyer can request a refund.
- **Reentrancy Guard**: Prevents reentrancy attacks by using OpenZeppelin's `ReentrancyGuard`.
- **Events**: Tracks actions like deposits, task completion, work confirmation, and refunds.

## Contract Structure

### State Variables:
- `buyer`: The address of the buyer (initiator of the escrow).
- `worker`: The address of the worker (recipient of the escrow payment).
- `amount`: The amount of funds held in escrow.
- `taskCompleted`: A boolean indicating whether the task has been completed.
- `taskCompletionTime`: The timestamp when the task was marked as complete.
- `refundDeadline`: The deadline by which the buyer can request a refund.

### Events:
- `addMoney(address indexed buyer, uint amount)`: Emitted when the buyer deposits funds into the contract.
- `taskComplete(address indexed worker, uint completionTime)`: Emitted when the worker marks the task as complete.
- `WorkConfirmed(address indexed buyer, address indexed worker, uint256 confirmationTime)`: Emitted when the buyer confirms the work and funds are sent to the worker.
- `refunding(address indexed buyer, address indexed worker, uint amount)`: Emitted when the buyer requests a refund.

### Modifiers:
- `onlyBuyer`: Restricts access to functions to the contract's buyer.
- `onlyWorker`: Restricts access to functions to the contract's worker.

## Functions

### `constructor(address _worker, uint256 _amount)`
- Initializes the contract with the worker's address and the escrow amount.
- The contract creator (the buyer) is set as the buyer.

### `amountAdd()`
- Allows the buyer to deposit the agreed amount into the contract.
- Ensures that the buyer sends exactly the specified amount.

### `completetask()`
- Allows the worker to mark the task as completed.
- Sets the task completion time and the refund deadline (3 days after completion).

### `confirmWork()`
- Allows the buyer to confirm that the work has been completed.
- Releases the funds to the worker, confirming the transaction.

### `refund()`
- Allows the buyer to request a refund if the task hasn't been completed.
- Ensures that the refund window is still open.

### `contractBalance()`
- Returns the balance of the contract, showing the current escrow amount.

## How to Use

1. **Deploy the Contract**: When deploying, pass the worker's address and the amount to be held in escrow.
2. **Deposit Funds**: The buyer deposits the agreed amount using the `amountAdd()` function.
3. **Complete the Task**: The worker calls `completetask()` to mark the task as completed.
4. **Confirm or Refund**: After the task is completed, the buyer can call `confirmWork()` to release the funds or `refund()` to get their money back if the task wasn't completed on time.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Fork

Feel free to fork this repository and modify it for your own purposes.
