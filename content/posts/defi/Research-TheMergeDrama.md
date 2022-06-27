# The Merge
Transition from Proof of Work to Proof of stake

## Proof of Work
- Looking for some amount of leading zeroes, aka your hash should be less than some value

## Proof of Stake
- Blocks constructed by leaders proposes block to committee, committee confirms the block
- Beacon chain: currently running in parallel to main POW chain and holds staking balances for ETH POS
- Will be merged into the main chain 

## Sharding
- Sharding splits a database into separate shards so nodes don't have to store full database
- Shards initially will only be a data layer, doesn't support execution
- Sharding can be used for execution via ZK rollups bc only proof needs to be stored

## Difficulty Bomb
- Hardcoded in eth, increases difficulty for block production, making it virtually impossible for miners to produce blocks
- Will hit when the merge takes place via the bellatrix hard fork and POS validators take over
- Shards would launch later

### Ropsten Drama
- Difficulty Bomb hits when the cumulative difficulty reaches a certain point. On ropsten, someone brought a ton of hash power to increase cumulative difficulty which made the difficulty bomb hit early
- Beacon chain wasn't yet there, normal chain can't produce blocks bc difficulty bomb
- On May 26th, there was a 7 block reorg. Reorgs are bad because they break many security assumptions on dapps, like oracles. Crypto Twitter suspects that this is due to people using different clients, some outdated, some current

## Consecutive Block Production By Leaders
- In POW, it's extremely unlikely for two miners to form more than one block in a row without a significant portion of the network hash power(selfish mining)
- Leaders in proof of stake would know that they are going to form a block in advance, so they would know when they could form two blocks in a row, and thus cause a reorg