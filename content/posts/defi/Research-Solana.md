# Solana

What are the differences between EVM and solana
1. Bitcoin is based on UTXO, Ethereum uses the accounts model
- Solana also uses the accounts model

### EVM accounts vs Solana
1. Ethereum/EVM uses Externally Owned Accounts + Smart contract
- nonce, balance, codehash, storage root(256 bit hash of root node of merkle tree that encodes storage)

2. Solana only has 1 type of account
Subtypes are data accounts and program accounts
- Lamports = wei
- owner = program owner, default is owned by system
- executable = whether this account can process instructions(true = program account, false = data account)
- data = raw data byte array stored by account
- rent_epoch = Next epoch that this account will owe rent(If you run out of rent, the account gets purged)
Program accounts only store logic in `data`, not state - which is stored in a separate account owned by this account

### Special type of accounts in Solana: PDAs
A smart contract in solana needs to store state outside of program, they don't want to store this using the same ed2559 elliptic curve to avoid collisions and someone getting the private key to this program in the future, so it uses a separate curve

## Transactions
EVM has two types of transactions, sending ether vs calling smart contract
- can only call single function

Solana allows you to call multiple instructions in the same program
Need to pass all accounts that would be part of the transactions

## Example: how tokens work

### Ethereum
- I have 10 USDC, in the USDC contract, the mapping called _balances 
    - `mapping(address => uint256) private _balances`
    - `_balances[my address] = 10`

### Solana
- Because the state of a program is just some logic, every token can reuse the same token program logic
- image.png