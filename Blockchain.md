# Blockchain

A blockchain is a distributed ledger that is completely open to anyone. Once a some data has been recorded inside of a blockchain, it becomes very difficult to change it.

In summary, it is like a distributed database that everyone can have a copy of that, but they can not change the records.

## Block

Each block in the blockchain contains the data, hash of the block, and the hash of the previous block. For instance, the data in the Bitcoin blockchain includes details of a transaction.

### Genesis Block

Genesis block is the parent of all of blocks. It is a special block, because it does not have a parent.

## Proof of work

Changing a block in blockchain make it invalid. Using hashes is not enough to prevent tempering, because nowadays computers are fast enough to calculate hundreds of thousands of hashes per second. It is possible to to tamper a block and recalculate all of hashes of other blocks to manipulate.

Proof of work is a mechanism to slow down the creation of new blocks. For instance, proof of work of Bitcoin takes about 10 minutes to be calculated. By use of proof of work, It is much harder to manipulate, because you need to calculate the proof of work for each one of the blocks.

## P2P Network

In addition to the proof of work, Blockchains also use the P2P networks to secure themselves. 

### Joining to the network

When someone joins the network, they gets the full copy of the blockchain.

### Creating a new Block

The created block is sent to everyone on the network. Each node verifies the block to make sure it hasn't been tampered with. Then, each node will add this block to their blockchain. 

## Difficulty Adjustment

Day by day, the power of computing can be stronger than before. Maybe a proof of work in future could be done in less than 5 minutes. To resolve this issue, each two weeks, blockchain adjust a difficulty to make it easier or harder to perform the proof of work. In summary the reason for this mechanism is to

1. Adjust the difficulty of creating new block over time while computers gain more powerful capabilities
2. Adjust the number of miners in the network. More number of miners means that the problem can be solved in a shorter amount of time, while having less miners cause a longer amount of time.

> More difficulty means that the operation consumes more electricity than it generates in rewards. This is kind of against of being distributed, because they would be centralized for big miners.

## Tamper with the Blockchain

So to successfully tamper with a blockchain, you will need to

1. Tamper with all blocks in the chain
2. Redo the proof of work for each block
3. Take control of 50% of the P2P network

## Smart Contracts

These contracts are simple programs that are stored on the blockchain and can be used to automatically exchange coins based on certain conditions.

# Applications

- Cryptocurrencies
- Odometer Fraud
- Intellectual Property
- Patents
- Notary
- Voting
- Food Industry
- Medical Records

## Find the valid Blockchain

When you want to join/rejoin to the P2P network, you need to find the valid blockchain, trust it, and then clone it. The question is how to find this valid blockchain.

# Resources

- [How does a blockchain work - Simply Explained](https://www.youtube.com/watch?v=SSo_EIwHSd4)
- [Blockchains: how can they be used? (Use cases for Blockchains)](https://www.youtube.com/watch?v=aQWflNQuP_o)