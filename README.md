Explanation of the Contract
Deposit Function: The deposit() function lets users deposit Ether. Their balance is updated, and an initial reward due date is set if they are staking for the first time.

Withdraw Function: The withdraw() function allows users to withdraw their staked Ether. It ensures that the user can only withdraw an amount within their balance.

Reward Calculation: Rewards are calculated at a 5% rate of the staked balance, and they are claimable every 7 days.

Claim Rewards Function: The claimRewards() function lets users claim their rewards if at least 7 days have passed since their last reward claim. The reward is added to their balance, and the timer is reset for the next claim period.

Event Logging: Events Deposit, Withdraw, and ClaimRewards are emitted to log each corresponding action.

This contract covers the basic functionality of a staking pool, enabling users to deposit, withdraw, and claim rewards after a specified period.
