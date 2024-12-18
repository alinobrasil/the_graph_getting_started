# The Graph 

Getting historical data on a smart contract can be frustrating when building a dapp. [The Graph](https://thegraph.com/) provides an easy way to query smart contract data through APIs known as subgraphs. The Graph’s infrastructure relies on a public network of indexers, enabling your dapp to become truly decentralized.

The Graph supports indexing contracts on Lens Network.

## Quick Start

These subgraphs only take a few minutes to set up. To get started, follow these three steps:

1. Initialize your subgraph project
2. Deploy & Publish
3. Query from your dapp

Here’s a step by step walk through:

## 1. Initialize your subgraph project

### Create a subgraph on Subgraph Studio⁠

Go to the [Subgraph Studio](https://thegraph.com/studio/) and connect your wallet. Once your wallet is connected, you can begin by clicking “Create a Subgraph”. When choosing a name, it is recommended to use Title Case: “Subgraph Name Chain Name.”

![Create a Subgraph](https://raw.githubusercontent.com/alinobrasil/the_graph_getting_started/refs/heads/lens/img/studio-create-subgraph.png)


You will then land on your subgraph’s page. All the CLI commands you need will be visible on the right side of the page:

![CLI commands](https://raw.githubusercontent.com/alinobrasil/the_graph_getting_started/refs/heads/lens/img/studio-graphcli-commands.png)


### Install the Graph CLI⁠

On your local machine run the following:
```
npm install -g @graphprotocol/graph-cli
```

### Initialize your Subgraph⁠

You can copy this directly from your subgraph page to include your specific subgraph slug:
```
graph init --studio <SUBGRAPH_SLUG>
```
You’ll be prompted to provide some info on your subgraph like this:

![cli sample](https://raw.githubusercontent.com/alinobrasil/the_graph_getting_started/refs/heads/lens/img/cli-sample.webp)


After entering the contract info, the graph-cli will attempt to fetch ABI, StartBLock & Contract name from the blockexplorer API. 

If these aren't imported automatically, choose "n" when asked to retry fetching that data. Here's how to prepare them manually:

![Specify ABI in CLI](https://raw.githubusercontent.com/alinobrasil/the_graph_getting_started/refs/heads/lens/img/cli-specify-abi.png)

1. ABI: save the contract's ABI as a json file in the same folder where you're running graph init. Then simply input the filename in the cli. In the screenshot above, it was named as `stk.json`.
2. Start Block: Enter the block where your contract was created. You can look this up in the block explorer in the transaction where the contract was created. 
3. Contract Name: If you're only indexing 1 smart contract, it's safe to go with the default name, `Contract`. Otherwise input the name of the contract as defined in its solidity source code.


## 2. Deploy & Publish

### Deploy to Subgraph Studio⁠

First run these commands:

```bash
$ graph codegen
$ graph build
```

Then run these to authenticate and deploy your subgraph. You can copy these commands directly from your subgraph’s page in Studio to include your specific deploy key and subgraph slug:

```bash
$ graph auth --studio <DEPLOY_KEY>
$ graph deploy --studio <SUBGRAPH_SLUG>
```

You will be asked for a version label. You can enter something like v0.0.1, but you’re free to choose the format.

### Test your subgraph⁠

You can test your subgraph by making a sample query in the playground section. The Details tab will show you an API endpoint. You can use that endpoint to test from your dapp.

![Playground](https://raw.githubusercontent.com/alinobrasil/the_graph_getting_started/refs/heads/lens/img/studio-playground.png)


### Publish Your Subgraph to The Graph’s Decentralized Network

Once your subgraph is ready to be put into production, you can publish it to the decentralized network. On your subgraph’s page in Subgraph Studio, click on the Publish button:

![publish button](https://raw.githubusercontent.com/alinobrasil/the_graph_getting_started/refs/heads/lens/img/studio-publish-button.png)


You'll need some ETH on Arbitrum One to create an on-chain transaction. The Graph's smart contracts are all on Arbitrum One, even though your subgraph is indexing data from Lens. 

![Publish Modal](https://raw.githubusercontent.com/alinobrasil/the_graph_getting_started/refs/heads/lens/img/studio-publish-modal.png)

Note: When publishing, you might see a "Partial Indexer Support" alert. This means the network's subgraphs will be indexed by The Graph's default indexer, but won’t be served by independent indexers on The Graph Network. Testnets will always have this limitation. Lens Mainnet subgraphs will soon be able to attract additional indexers once a voting process on The Graph Network is completed. This will unlock indexer rewards, incentivizing independent indexers to support the subgraphs. At that point, the "Partial Indexer Support" warning will no longer appear.



## 3. Query your Subgraph

Congratulations! You can now query your subgraph on the decentralized network! Here's the published subgraph from the example above: https://thegraph.com/explorer/subgraphs/8BpaJFWEBQYjUmi8ToJA9RChPPut2A4GCXpBgSJRtdBv?view=Query&chain=arbitrum-one

For any subgraph on the decentralized network, you can start querying it by passing a GraphQL query into the subgraph’s query URL which can be found at the top of its Explorer page:

![Query URL](https://raw.githubusercontent.com/alinobrasil/the_graph_getting_started/refs/heads/lens/img/explorer-lensMytoken-query-url.png)


`https://gateway.thegraph.com/api/`**[api-key]**`/subgraphs/id/8BpaJFWEBQYjUmi8ToJA9RChPPut2A4GCXpBgSJRtdBv`

Now, you simply need to  fill in your own API Key to start sending GraphQL queries to this endpoint.

### Getting your own API Key

![API keys](https://raw.githubusercontent.com/alinobrasil/the_graph_getting_started/refs/heads/main/img/getting-api-key.png)


In Subgraph Studio, you’ll see the “API Keys” menu at the top of the page. Here you can create API Keys.

## Appendix

### Example from an Ethereum Subgraph on the Decentralized Network

Here’s an example from the [CryptoPunks Ethereum subgraph](https://thegraph.com/explorer/subgraphs/HdVdERFUe8h61vm2fDyycHgxjsde5PbB832NHgJfZNqK) by Messari:

![Query URL](https://raw.githubusercontent.com/alinobrasil/the_graph_getting_started/refs/heads/main/img/explorer-query-url.png)


### Sample Query

This query shows the most expensive CryptoPunks sold.

```graphql
{
  trades(orderBy: priceETH, orderDirection: desc) {
    priceETH
    tokenId
  }
}

```

Passing this into the query URL returns this result:

```
{
  "data": {
    "trades": [
      {
        "priceETH": "124457.067524886018255505",
        "tokenId": "9998"
      },
      {
        "priceETH": "8000",
        "tokenId": "5822"
      },
//      ...
    ]
  }
}
```

<aside>
💡 Trivia: Looking at the top sales on [CryptoPunks website](https://cryptopunks.app/cryptopunks/topsales) it looks like the top sale is Punk #5822, not #9998. Why? Because they censor the flash-loan sale that happened.
</aside>

### Sample code

```jsx
const axios = require('axios');

//your graphql query
const graphqlQuery = `{
  trades(orderBy: priceETH, orderDirection: desc) {
    priceETH
    tokenId
  }
}`;

//query URL obtained from subgraph's explorer page
const queryUrl = 'https://gateway.network.thegraph.com/api/[api-key]/subgraphs/id/HdVdERFUe8h61vm2fDyycHgxjsde5PbB832NHgJfZNqK'

const graphQLRequest = {
  method: 'post',
  url: queryUrl,
  data: {
    query: graphqlQuery,
  },
};

// Send the GraphQL query
axios(graphQLRequest)
  .then((response) => {
    // Handle the response here
    const data = response.data.data
    console.log(data)

  })
  .catch((error) => {
    console.error(error);
  });
```

### Additional resources:

- To explore all the ways you can optimize & customize your subgraph for a better performance, read more about [creating a subgraph here](https://thegraph.com/docs/en/developing/creating-a-subgraph/).
- For more information about querying data from your subgraph, read more [here](https://thegraph.com/docs/en/querying/querying-the-graph/).