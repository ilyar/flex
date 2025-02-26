# AMM and Liquidity Pools

### Automated Market Maker&#x20;

Instead of a client software like Flex DeBot, an automatic execution could be programmed with a paid subscription method of an $Asset contract. The contract may send a message to other contracts that are subscribed to it once any trade is executed. The subscribed contract can then perform automatic strategies based on this information, to connect Automated Market Maker smart contracts or Liquidity Pools.

Flex AMM uses such subscriptions to support Automated Market Making for Flex. It takes the tokens accrued in its liquidity pools to create a series of orders at different price points and sizes to provide liquidity to the Flex Order Book.

The Flex AMM receives notifications about the Order Book transactions and creates orders around a recent market price using the Fibonacci sequence to provide a specified number of bids and asks at a variety of prices. The relative size of orders depends on the available liquidity of tokens in the pool and the Fibonacci sequence factors for ticks from the market price.

### Liquidity Pools

The Flex AMM creates a Liquidity Pool for each pair of tokens it trades on the Flex Order Book. These Liquidity Pools are used to provide liquidity to the Order Book. They are not used as a source of market price as there are no swaps of tokens within the pools. The market price is derived from the activity on the exchange.

The current implementation of the Liquidity Pools requires Liquidity Providers to contribute both tokens in the pool in proportion to the market price of the pair. It is believed to ensure a better balance of tokens in the Liquidity Pools.

