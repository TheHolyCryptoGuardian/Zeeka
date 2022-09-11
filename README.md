# Instruction to install your own Zeeka node

First you need to update necessary packages — `sudo apt update && sudo apt upgrade -y`

Then install necessary packages — `sudo apt install wget jq git libssl-dev cmake -y`

Install Rust — `. <(wget -qO- https://raw.githubusercontent.com/letsnode/Utils/main/installers/rust.sh)`

Clone repository — `git clone https://github.com/zeeka-network/bazuka`

Get in *Bazuka* directory — `cd bazuka`

Install this — `cargo build`

Initialize node — `bazuka init --seed [your seed phrase] --network debug --node 127.0.0.1:8765`

**IMPORTANT**

Change *your seed phrase* to your new wallet that you should create specially for Zeeka. Just create a new wallet in Metamask (for example) and past the seed phrase to the command in ''. Example `bazuka init --seed 'avtogen is the best crypto user ever for sure i promise you' --network debug --node 127.0.0.1:8765`

Create a system file and don't forget to change [your ip] to your ip — 

`sudo tee <<EOF >/dev/null /etc/systemd/system/zeeka.service`  
`[Unit]`  
`Description=Zeeka node`  
`After=network.target`  
  
`[Service]`  
`User=$USER`  
`ExecStart=`RUST_LOG=info which bazuka` node --listen 0.0.0.0:8765 --external [your ip]:8765 --network debug --db ~/.bazuka-debug --bootstrap 5.161.152.123:8765 --bootstrap 65.108.201.41:8765 --bootstrap 185.213.25.229:8765 --bootstrap 45.88.106.199:8765 --bootstrap 148.251.1.124:8765 --bootstrap 195.54.41.115:8765 --bootstrap 195.54.41.130:8765`  
`Restart=on-failure`  
`RestartSec=3`  
`LimitNOFILE=65535`  
  
`[Install]`  
`WantedBy=multi-user.target`  
`EOF`

Run the service — `sudo systemctl daemon-reload` 
                  `sudo systemctl enable zeeka`
                  `sudo systemctl restart zeeka`

To add a command to check node logs type this — `. <(wget -qO- https://raw.githubusercontent.com/AlexM-dev/Utils/main/commands/insert_variable.sh) -n zeeka_log -v "sudo journalctl -fn 100 -u zeeka" -a`

Check logs — `zeeka_log`

If your current height is 1 that's ok.

# What is Zeeka?
In simplest words, Zeeka (ℤ) is a cryptocurrency which aims to provide a light and scalable blockchain by extensively using the help of Zero-Knowledge proof technology.

For detailed technical information, read the [Zeeka Whitepaper](https://hackmd.io/_Sw5u2lUR9GfBV5vwtoMSQ)! Also, we are actively developing the Zeeka project. Follow us on [GitHub](https://github.com/zeeka-network)!

## Huh? Zero-Knowledge proofs? 🤔
A Zero-Knowledge protocol is a crytographic method by which someone can prove that they know the answer of a problem without actually revealing it. A very good example of an interactive Zero-Knowledge proof is provided below:

Suppose Alice is blindfolded and has two balls in her hands. Bob, who is able to see the balls, claims that the balls are different in colors. Alice doesn't trust Bob. How can Bob convince Alice that the balls have different colors (The problem), without uncovering Alice's eyes (Revealing the answer)?

![colorblind](https://user-images.githubusercontent.com/96244917/189488116-2ee7ab0a-1b05-40a3-b73b-3679ea649f2a.png)

Here is what Alice does:

- She first hides the balls behind her back.
- She shuffles the balls with a 50% chance.
- She shows back the balls to Bob, and ask him: - Did I shuffle the balls?

![shuffle](https://user-images.githubusercontent.com/96244917/189488151-538d5a26-7099-41b6-8dbe-bdbd402effb1.png)

If the balls are really different in colors, Bob would give Alice the correct answer. If he can't distinguish their colors, he still can give Alice a random answer, and his answer can still be correct. But the chances of giving a correct answer is 50%.

Alice repeats the procedure for 20 times. If the balls have same colors, the chances of Bob giving the correct answer all the 20 times is (1/2)^20 (Around 0.000001%). The probability is so tiny that Alice can conclude that Bob is really able to distinguish between the balls, leading to the conclusion that they really have different colors.

## What are you trying to prove? 😐
Suppose there is a novel payment system that consists of a merkle tree in which every leaf represents an account (A public key and a balance). We define the state of the system as the merkle root of this tree.

![Hash_Tree svg](https://user-images.githubusercontent.com/96244917/189488169-8faae011-20d5-4fe5-9608-538fe838d9da.png)

We want to prove a big set of transactions have happened, changing the state of the system from A to B (The problem), without showing the transactions (The answer).

Now, here is the mind blowing fact:

The proof that you provide is constant in size, no matter how big the answer is. E.g the answer can be millions of transactions, but you don't need to show them for the state transition to happen. A constant sized proof is enough to convince everyone that the state transition is valid! 🤯

## Ehmmm, Ethereum already has this with zkRollups? 🙄
You got us! But hey, there is a big difference. In zkRollups, there is a centeralized operator, constantly publishing Zero-Knowledge proofs on the main chain, and in case the operator stops doing this (i.e gets unavailable), the chain will roll back to a previous state, from which some other operator is able to build on. This is a huge amount of complexity.

Blockchains like Ethereum/Bitcoin, enforce data-availability of the chain history, and not a SNARK state. Zeeka nodes and validators are implemented in a way to only accept forks that reveal the chain state of their last block, meaning that they will check if the hash of the provided state results in the state-hash submitted on the last block. This mechanism makes sure that the compressed state of the last block is always available. A longer subchain whose tip state is not available is worth nothing and is not accepted by the network. This removes the mentioned complexities by a great deal and creates room for some creativity as well.

## Okay, how are you going to handle smart-contracts? 😉
The equivalent of a Smart Contract in Zeeka blockchain is a Zero Contract. The contracts in Zeeka blockchain are not written for a specific virtual-machine (Like EVM), but written in R1CS (Which is the building block of zkSNARK circuits).

![r1cs](https://user-images.githubusercontent.com/96244917/189489478-201e59c9-ce3d-4e9b-b6ae-a15412d53bbd.png)

In this scheme, the programmer uploads the verifying keys of his R1CS contract (Which can consist of multiple circuits) on the blockchain and people could easily invoke these circuits and move from one state into another with a single small transaction (Which could be a compressed version of thousands of transactions).

## How can I be a part of this? ❤️
Follow us on GitHub! Contribute to the project either by coding or promoting it. 💸 Investments are also welcome. Keep in touch with me on Telegram :)  [@keyvankambakhsh](https://t.me/keyvankambakhsh)
