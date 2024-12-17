# SimpleWallet
Blockchain SimpleWallet
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleWallet {
    // Owner of the wallet
    address public owner;

    // Events to log transactions
    event Deposit(address indexed sender, uint256 amount);
    event Withdraw(address indexed receiver, uint256 amount);

    // Modifier to restrict access to the owner
    modifier onlyOwner() {
        require(msg.sender == owner, "Not the owner of the wallet");
        _;
    }

    // Constructor to set the wallet owner
    constructor() {
        owner = msg.sender;
    }

    /**
     * @dev Deposit funds into the wallet.
     * Allows users to send ETH to the contract.
     */
    function deposit() external payable {
        require(msg.value > 0, "Deposit amount must be greater than 0");
        emit Deposit(msg.sender, msg.value);
    }

    /**
     * @dev Withdraw funds from the wallet (onlyOwner).
     * @param amount The amount of Ether to withdraw.
     */
    function withdraw(uint256 amount) external onlyOwner {
        require(amount <= address(this).balance, "Insufficient balance");
        payable(owner).transfer(amount);
        emit Withdraw(owner, amount);
    }

    /**
     * @dev Get the current balance of the wallet.
     * @return The wallet's balance in wei.
     */
    function getBalance() external view returns (uint256) {
        return address(this).balance;
    }
}
