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
