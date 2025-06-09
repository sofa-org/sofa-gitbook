# Oracle Construction

In the world of cryptocurrencies and DeFi, **obtaining off-chain data in a timely and accurate manner is crucial.**  As smart contracts cannot directly access external systems or data sources, oracles serve to bridge this gap by bringing outside information onto the blockchain.  The trustworthy data service must be provided to ensure full settlement transparency and traceability in settlement, while minimizing possible disputes.

The **ChainLink** oracle service, known for its high degree of decentralization, reliability, and adaptability, has been chosen as SOFA's primary means of accessing off-chain data.  Furthermore, the use of ChainLink price feeds has the **additional benefit of protecting the protocol against 'flash loan attacks'**, while the **source of data feeds will be from reputable trading platforms such as Coinbase** to ensure sanctity of data.

![](../../static/chainlink.png)

SOFA utilizes [APRO Oracle](https://www.apro.com) to protect our assets on multiple chains. The support and protection from **APRO Oracle** allow us to maintain the stability and security of our on-chain assets. For more information, please refer to the [APRO Oracle documentation](https://docs.apro.com/en).

![](../../static/apro.jpg)

## Spot Price / Price at Expiry

Obtaining the current price of an underlying asset on-chain through an oracle is typically done in one of two ways:

- calling contracts from DEXs like Uniswap to get the Price or TWAP Price of the underlying asset
- obtaining the asset price through data feeds provided by an oracle service like ChainLink

Considering the need for the Settlement Price to be as close as possible to CeFi exchange prices (for ease of hedging by users or market makers) and for security reasons (to avoid manipulation and attacks), we have **chosen ChainLink's Data Feeds as the oracle source for our Price at Expiry**.

With ChainLink's decentralized data sources that aggregate pricing information from different exchanges, we ensure that our smart contract executions are based on up-to-date, reliable, and fairest sets of pricing data publicly available.

## Calculated Price Derived from Historical Closes

For products like Rangebound, it is necessary to know the highest and lowest prices of the underlying token from mint inception to expiry in order to calculate the final payout.  Common on-chain typically cannot provide this data, given that on-chain oracles do not provide continuous time-series data.

Instead of deploying our own oracle contracts as a (common) centralization compromise, we have found a solution in **ChainLink's Functions product**.  Thanks to the latest innovations, we are able to call off-chain APIs in a decentralized manner, and publish the obtained data on-chain through decentralized nodes like a typical data feed.

![](../../static/KxYlbnS0IoEtX6xAxV1uB0WKsLg.png)

## Automation Regular Update Service

SOFA utilizes ChainLink Automation to guarantee that prices are automatically updated and pushed to the blockchain on a regular basis. ChainLink Automation offers a decentralized network where smart contracts can schedule and automatically execute complex tasks, including interval data updates, trigger events, and even critical contract adjustments.  This **ensures that on-chain prices remain constantly up to date, and offers data confidence to SOFA's network of connected dApps**.

![](../../static/FESNbrjpEobC0DxtBz5u6Og0sgf.png)

## Decentralization and Traceability

Staying true to our core DeFi spirit, **SOFA's price data retrieval process will always be fully decentralized**, offering full audit traceability at the contract level.  Our advocacy for transparent processing ensures that every step within the system is openly verifiable by users, with pricing data details such as provider source and aggregation logic fully observable at all times.

## Continuous Innovation in Oracle Services

While ChainLink remains a gold standard for DeFi data reliability, SOFA is continuously exploring and staying informed of the latest innovations in oracle services.  We are cognizant of over-reliance on limited provider sources and will** strive to diversify our protocol inputs as much as technologically possible**.

In the long-run, SOFA strives to offer users a robust, transparent, decentralized, and sustainable data acquisition system that can safeguard user interests while ensuring operational longevity.

