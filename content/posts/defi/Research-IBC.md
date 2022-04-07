## IBC Research

Quick notes
- Bc of the blockchain trilemma{decentralization, scalability, security}, applications will use multiple blockchains to achieve the trilemma
- To communicate between blockchains
1. Deploy contract on Ethereum: meant to communicate with validators
2. Send package to validators
3. Validators communicate with solana blockchain
Eg If transferring USDT, you lock USDT on ethereum contract and mint synthetic USDT on solana blockchain
**Validator:** Who runs the validators between chains?

## IBC
- Handle Authentication, ordering data packets between blockchains, can use 
- Recall above, the package needs to be validated
- Do you need to use IBC + a bridge?
Mental model: IBC is a validator. (Who runs the validator?)
Goal: Manage communication between two modules. This involves sending/receiving packets with establish connection

## How IBC works
Properties of IBC:
Data confidentiality + Legibility: Send as little data to validator as possible + validator needs to understand the data
Reliability: Relayer + blockchain works independently, there is a chance of dropping packets. This is managed by a sequence number per packet.
Flow Control: Depends on ledger, IBC does not have overriding rule
Authentication: Whenever IBC is to be used, data must be first on the originating blockchain. IBC handler(validator?) verifies the data
Statefulness: Three parts - clients, connection, channel
Clients: info about state of both blockchains
connection: identifiers/data structures agreed by both blockchains
channel - info about encoding info, state, sequence numbers
Multiplexing: smart contracts can use single IBC connection. Generally one to one but IBC supports one to many and many to one

Relayer - keeps watch of state chain from origin chain(ie Ethereum in example above)

## Use Case
Why switch networks when purchasing insurance on other chain?
- If vault and pool are connected with IBC, user can buy insurance from any chain while spending from mainnet wallet.

## Questions?
Confirmation time is 15 seconds on ETH, Bitcoin is 10 minutes, can we still use IBC?
Need to wait 10 minutes?

What about reorgs? There's never true finality on proof of work chains right?
Solved via validators?
