# ethernautCoinFlip

Title:
Ethernaut Lvl 3: Coin Flip Puzzle - Exploiting Pseudo-Randomness in Smart Contracts

Body:
🔒💻 In this in-depth series on the Zeppelin team’s Ethernaut smart contract security puzzles, I'm excited to share my latest walkthrough for Level 3: Coin Flip. This level is a fascinating exploration of pseudo-randomness in Ethereum smart contracts and how it can be exploited.

🤔 How Does Ethereum Generate "Randomness"?
Ethereum lacks true randomness, relying instead on "good enough" pseudo-random generators. These are typically created by hashing variables like transaction timestamps, sender addresses, and block heights, using cryptographic hashing functions like SHA-3 or KECCAK256.

🎲 The Coin Flip Challenge:
The challenge requires correctly guessing the outcome of a coin flip 10 times in a row. But here's the twist: the input variables that determine the flip are publicly available, making the outcome predictable!

👨‍💻 Crafting a Malicious Contract:
I've created a malicious contract that replicates the logic of the original CoinFlip contract. By predicting the outcome using the same input variables, the contract can consistently make correct guesses.

🔑 Key Security Takeaway:
This exercise underscores the vulnerability of pseudo-randomness in smart contracts. When randomness determines something as critical as contest winners, it's vital to understand how easily it can be predicted and manipulated.

📝 Solution:
Here's a snippet of the solution (see full code in CoinFlip.sol):}

🔁 Execute this function 10 times and watch the consecutiveWins counter on Ethernaut increase!



Key Takeaways

pseudo randomness can be exploited
randomness based on hashing block numbers and other known data can be faked
if the hashing logic is known by the attacker
if the data to be hashed is known by the attacker
in this case both of which are easily obtainable, thus the problem to solve is deterministic
to achieve real randomness we should utilise an external source like chainlink
smart contracts can call functions of other smart contracts
this way we can facilitate a proxy contract to crack the coin flip
remix can be utilised to write and deploy such contracts on the fly
a) create a file with the original contract
to do so, copy paste the code from ethernaut into a new contract file
b) create and deploy a custom contract containing the “hack logic”
in here we reference the original contract by importing and initialising it with the instance address from ethernaut
this way we are able to call the flip() function of the original contract
Solution

first of all we move over to the web-IDE Remix: https://remix.ethereum.org/
as your environment, select injected provider with your rinkeby account to sign transactions
create a new file named CoinFlip.sol
in here copy paste the whole code from the ethernaut challenge that defines the CoinFlip contract.
to get it running, we have to fix some errors by adjusting the following:
solidity version at the top must be ^0.8.0 instead of ^0.6.0 (because of the newer versioned imports)
the import statement needs to have the following updated path 'github.com/OpenZeppelin/openzeppelin-contracts/contracts/utils/math/SafeMath.sol'
create a new file named Hack.sol which needs to contain the following code:
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import './CoinFlip.sol';

contract Hack {
    CoinFlip public originalContract = CoinFlip(copy paste your instance address here); 
    uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

    function hackFlip(bool _guess) public {
        uint256 blockValue = uint256(blockhash(block.number - 1));
        uint256 coinFlip = blockValue/FACTOR;
        bool side = coinFlip == 1 ? true : false;

        if (side == _guess) {
            originalContract.flip(_guess);
        } else {
            originalContract.flip(!_guess);
        }
    }
}
as you can see, at the top we import the original contract which is then referenced as the public originalContract variable, by initialising it with the instance address we received on ethernaut (this allows us to call its flip() function later on)
we also copy the division factor, since we need it for the calculation
then we implement the function hackFlip()
which takes a bool as argument, same as the original flip function
the first three lines of this function mimic the block number and division factor based calculation, exactly the same as in the original function
then we simply use that result (side) to compare it with our initial guess (that we passed to hackFlip()) and if it is correct, we pass it on to the original flip() call, if not we pass the opposite -> we always win
after calling hackFlip() 10 times, we have 10 consecutive wins and have solved the challenge
(optional) back in ethernaut we can check this
command: await contract.consecutiveWins()
inside the result array under word our wins are represented -> 0:10
