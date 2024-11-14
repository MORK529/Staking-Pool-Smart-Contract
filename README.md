// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract StakingPool {
    struct Staker {
        uint256 balance;
        uint256 rewardDueDate;
    }
    
    mapping(address => Staker) public stakers;
    uint256 public constant REWARD_RATE = 5; // 5%
    uint256 public constant REWARD_PERIOD = 7 days;
    
    event Deposit(address indexed staker, uint256 amount);
    event Withdraw(address indexed staker, uint256 amount);
    event ClaimRewards(address indexed staker, uint256 rewardAmount);

    // Function to deposit Ether and participate in staking
    function deposit() external payable {
        require(msg.value > 0, "Deposit amount must be greater than zero");

        Staker storage staker = stakers[msg.sender];
        staker.balance += msg.value;

        // Set or reset the reward due date
        if (staker.rewardDueDate == 0) {
            staker.rewardDueDate = block.timestamp + REWARD_PERIOD;
        }

        emit Deposit(msg.sender, msg.value);
    }

    // Function to withdraw Ether from the staking pool
    function withdraw(uint256 amount) external {
        Staker storage staker = stakers[msg.sender];
        require(staker.balance >= amount, "Insufficient balance");

        staker.balance -= amount;
        payable(msg.sender).transfer(amount);

        emit Withdraw(msg.sender, amount);
    }

    // Function to claim staking rewards
    function claimRewards() external {
        Staker storage staker = stakers[msg.sender];
        require(block.timestamp >= staker.rewardDueDate, "Reward period has not yet passed");

        uint256 reward = (staker.balance * REWARD_RATE) / 100;
        staker.balance += reward;

        // Reset the reward due date for the next period
        staker.rewardDueDate = block.timestamp + REWARD_PERIOD;

        emit ClaimRewards(msg.sender, reward);
    }

    // Function to get the balance of a staker
    function getBalance(address stakerAddress) external view returns (uint256) {
        return stakers[stakerAddress].balance;
    }

    // Function to get the reward due date of a staker
    function getRewardDueDate(address stakerAddress) external view returns (uint256) {
        return stakers[stakerAddress].rewardDueDate;
    }
}
