# Subgraphs

We support the development of efficient and responsive applications by providing real-time and accurate blockchain data access through the use of The Graph's indexing protocol. Subgraphs are a powerful tool that allows us to index data stored on Ethereum and query this data via GraphQL.

Here is how you can utilize custom subgraphs to query related data within the protocol.

| Subgraph                | Query URL                                  |
|-------------------------|--------------------------------------------|
| SOFA Mainnet            | https://api.studio.thegraph.com/query/77961/sofa-mainnet/version/latest |
| SOFA Arbitrum           | https://api.studio.thegraph.com/query/77961/sofa-arbitrum/version/latest |
| SOFA BSC                | https://api.studio.thegraph.com/query/77961/sofa-bsc/version/latest |
| SOFA Polygon            | https://api.studio.thegraph.com/query/77961/sofa-polygon/version/latest |
| SOFA Sei                | https://api.studio.thegraph.com/query/77961/sofa-sei/version/latest |
| SOFA Automator Mainnet  | https://api.studio.thegraph.com/query/77961/sofa-automator-mainnet/version/latest |
| SOFA Automator Arbitrum | https://api.studio.thegraph.com/query/77961/sofa-automator-arbitrum/version/latest |

## Accessing Our Subgraph

We have created specific subgraphs for the protocol's data. These subgraphs contain various entities indexed from the blockchain, such as transactions, accounts, and specific events related to our DeFi products.  You can query data in the subgraph via the following steps:

### Step 1: Find the Endpoint

Locate the GraphQL endpoint URL corresponding to the subgraph you wish to interact with. This typically appears as a web address link. Please ensure you are using the most up-to-date endpoint.

### Step 2: Understand the Schema

The schema describes the entities within the subgraph and the types of queries that can be performed. Understanding the schema is crucial for writing efficient queries.

### Step 3: Write a GraphQL Query

Based on the schema, you can start writing GraphQL queries to query data from the subgraph. GraphQL allows for very specific queries, requesting only the fields you are interested in.  For example, if you want to query detailed information for each transaction, you might write a query like this:

```
{
  transactions(first: 5) {
    vault
    tradingFeeRate
    totalCollateral
    timestamp
    term
    spreadAPR
    referral
    minter
    makerCollateral
    maker
    id
    hash
    expiry
    collateralAtRiskPercentage
    borrowAPR
    anchorPrices
  }
}
```

This will return transactions and their related details for the first five records on the blockchain.

### Step 4: Send the Query Request

Send your query request using any HTTP client that supports GraphQL. You can use a command-line tool such as curl, or a library integrated into your application, like apollo-client.  Typically, the request is sent as a POST request to the endpoint URL.

An example code using curl is as follows:

```
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ transactions(first: 5) { id vault totalCollateral minter timestamp } }"}'  https://api.studio.thegraph.com/query/77961/sofa-mainnet/version/latest
```

Replace the the URL based on the actual situation.

### Step 5: Analyze and Use the Response

After your query request is sent, the GraphQL service will return the response in JSON format. You can process this data in your application or directly display it to users on the front end.

## Best Practices

- **Use Query Variables**:  If you frequently need to perform queries of the same structure but with different parameters, using query variables can make it easier to reuse your queries.
- **Handle Errors**:  In addition to normal responses, GraphQL can also return error information. Make sure your client code can gracefully handle these situations.

By following the above steps, you can effectively use The Graph to query data on the SOFA protocol to support your development projects.   Please refer to the rest of our developer documentation and Subgraph related resources for further information.

