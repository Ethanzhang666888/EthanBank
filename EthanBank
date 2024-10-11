// SPDX-License-Identifier: MIT
pragma solidity ^0.8.28;

contract EthanBank {
    // Mapping to track deposits by each address
    mapping(address => uint256) public deposits;
    // Array to keep track of top 3 users by deposit amount
    address[3] public topUsers;
    // Array to store the corresponding deposit amounts
    uint256[3] public topAmounts;
    // Admin address
    address public admin;

    // Event to log deposits
    event Deposited(address indexed user, uint256 amount);

    // Constructor to set the admin
    constructor() {
        admin = msg.sender;
    }

    // Function to receive deposits
    receive() external payable {
        require(msg.value > 0, "Deposit amount must be greater than zero.");
        
        // Record the deposit amount for the sender
        deposits[msg.sender] += msg.value;

        // Log the deposit event
        emit Deposited(msg.sender, msg.value);

        // Update the top 3 users if necessary
        updateTopUsers(msg.sender, deposits[msg.sender]);
    }

    // Internal function to update top users
    function updateTopUsers(address user, uint256 amount) internal {
        for (uint256 i = 0; i < 3; i++) {
            if (amount > topAmounts[i]) {
                // Shift down the array if necessary
                for (uint256 j = 2; j > i; j--) {
                    topAmounts[j] = topAmounts[j - 1];
                    topUsers[j] = topUsers[j - 1];
                }
                // Update the top user and amount
                topUsers[i] = user;
                topAmounts[i] = amount;
                break;
            }
        }
    }

    // Withdraw function for the admin only
    function withdraw(uint256 amount) external {
        require(msg.sender == admin, "Only admin can withdraw.");
        require(address(this).balance >= amount, "Insufficient funds in contract.");
        
        // Transfer the requested amount to the admin
        payable(admin).transfer(amount);
    }

    // Function to get contract balance (for admin use)
    function getContractBalance() external view returns (uint256) {
        return address(this).balance;
    }
}
