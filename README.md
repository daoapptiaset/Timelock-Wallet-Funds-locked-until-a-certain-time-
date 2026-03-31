# Timelock-Wallet-Funds-locked-until-a-certain-time-
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract TimeLockWallet {
    address public owner;
    uint256 public unlockTime;
    uint256 public lockedAmount;

    constructor(uint256 _unlockTime) payable {
        owner = msg.sender;
        unlockTime = _unlockTime;
        lockedAmount = msg.value;
    }

    function withdraw() public {
        require(msg.sender == owner, "Only owner can withdraw");
        require(block.timestamp >= unlockTime, "Funds are still locked");
        payable(owner).transfer(address(this).balance);
    }

    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }
}
