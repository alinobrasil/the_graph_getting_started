# The Graph 

Getting historical data on a smart contract can be frustrating when building a dapp. [The Graph](https://thegraph.com/) provides an easy way to query smart contract data through APIs known as subgraphs. The Graph’s infrastructure relies on a network of indexers, enabling your dapp to become truly decentralized.

Chiliz mainnet and testnet are both supported by The Graph.

## Quick Start: Indexing the PSG Token on Chiliz

These subgraphs only take a few minutes to set up. To get started, follow these three steps:

1. Initialize your subgraph project
2. Deploy & Publish
3. Query from your dapp

Pricing: 
 - The rate-limited test endpoints in Studio are free. 
 - API calls for the decentralized network are pay-per-use at $4 per 100K queries. **The first 100K queries are free!**


Here’s a step by step walk through:

## 1. Initialize your subgraph project

### Create a subgraph on Subgraph Studio⁠

Go to the [Subgraph Studio](https://thegraph.com/studio/) and connect your wallet. Once your wallet is connected, you can begin by clicking “Create a Subgraph”. When choosing a name, it is recommended to use Title Case: “Subgraph Name Chain Name.”

![Create a Subgraph](https://lh7-us.googleusercontent.com/docsz/AD_4nXf8OTdwMxlKQGKzIF_kYR7NPKeh9TmWnZBYxb7ft_YbdOdx_VVtbp6PslN7N1KGUzNpIDCmaXppdrllM1cw_J4L8Na03BXOWzJTK1POCve0nkRjQYgWJ60QHAdtQ4Niy83SMM8m0F0f-N-AJj4PDqDPlA5M?key=fnI6SyFgXU9SZRNX5C5vPQ)

<br>
You will then land on your subgraph’s page. 
All the CLI commands you need will be visible on the right side of the page:

![CLI commands](https://raw.githubusercontent.com/alinobrasil/the_graph_getting_started/refs/heads/chiliz/img/studio-cli-commands.webp)


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

The `--studio` tag is optional. In this example, I ran: 
```
graph init psg-token
```

You’ll be prompted to provide some info on your subgraph like this:

![cli sample](https://github.com/alinobrasil/the_graph_getting_started/raw/refs/heads/chiliz/img/cli-sample-output.webp)


Simply have your contract verified on the block explorer and the CLI will automatically obtain the ABI and set up your subgraph. The default settings will generate an entity for each event. 

Note:
- If the contract uses a proxy like PSG Token, then use the implementation contract's address instead. You'll see it in the "read contract" tab on the block explorer page of the contract.
- If the Start Block isn't automatically obtained, you can manually enter the block number where the contract was created. You can obtain this from the block explorer:
![Contract creation block number](https://github.com/alinobrasil/the_graph_getting_started/raw/refs/heads/chiliz/img/chiliz-scan.webp)

If you had to enter a proxy's implementation contract address like above, then once the project is set up, go to the manifeset file (subgraph.yaml) and change the contract address to the proxy's address
![Contract address](https://github.com/alinobrasil/the_graph_getting_started/raw/refs/heads/chiliz/img/update-manifest-address.webp)



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

You will be asked for a version label. You can enter something like v0.0.1, but you’re free to choose the format. Once that's done, you'll see the subgraph start to sync in the Studio page:

![Subgraph syncing in studio](https://github.com/alinobrasil/the_graph_getting_started/raw/refs/heads/chiliz/img/subgraph-deployed.webp)

### Test your subgraph⁠

You can test your subgraph by making a sample query in the playground section. The Details tab will show you an API endpoint. You can use that endpoint to test from your dapp.

![Playground](https://github.com/alinobrasil/the_graph_getting_started/raw/refs/heads/chiliz/img/playground.webp)


### Publish Your Subgraph to The Graph’s Decentralized Network

Once your subgraph is ready to be put into production, you can publish it to the decentralized network. On your subgraph’s page in Subgraph Studio, click on the Publish button:

![publish button](https://github.com/alinobrasil/the_graph_getting_started/raw/refs/heads/chiliz/img/publish-button.webp)
It'll trigger a transaction through your wallet to publish your subgraph as an NFT on the Arbitrum One network. 


> **Note:** The Graph's smart contracts are all on Arbitrum One, even though your subgraph is indexing data from Chiliz, Ethereum any other [supported chain](https://thegraph.com/docs/en/developing/supported-networks/). 

This subgraph can be found here [on the network](https://thegraph.com/explorer/subgraphs/7x2xzF58Mr694MiAQxCK6fiXux1vaBQm5SdYhbv1LNGE?view=Query&chain=arbitrum-one). 

## 3. Query your Subgraph

Congratulations! You can now query your subgraph on the decentralized network!

For any subgraph on the decentralized network, you can start querying it by passing a GraphQL query into the subgraph’s query URL which can be found at the top of its Explorer page.

The PSG Token subgraph above was [published here](https://thegraph.com/explorer/subgraphs/7x2xzF58Mr694MiAQxCK6fiXux1vaBQm5SdYhbv1LNGE?view=Query&chain=arbitrum-one):

![Query URL](https://github.com/alinobrasil/the_graph_getting_started/raw/refs/heads/chiliz/img/explorer-query-url.webp)


The query URL for this subgraph is:

`https://gateway-arbitrum.network.thegraph.com/api/`**[api-key]**`/subgraphs/id/7x2xzF58Mr694MiAQxCK6fiXux1vaBQm5SdYhbv1LNGE`

Now, you simply need to  fill in your own API Key to start sending GraphQL queries to this endpoint.

### Getting your own API Key

![API keys](https://lh7-us.googleusercontent.com/docsz/AD_4nXdz7H8hSRf2XqrU0jN3p3KbmuptHvQJbhRHOJh67nBfwh8RVnhTsCFDGA_JQUFizyMn7psQO0Vgk6Vy7cKYH47OyTq5PqycB0xxLyF4kSPsT7hYdMv2MEzAo433sJT6VlQbUAzgPnSxKI9a5Tn3ShSzaxI?key=fnI6SyFgXU9SZRNX5C5vPQ)


In Subgraph Studio, you’ll see the “API Keys” menu at the top of the page. Here you can create API Keys.

## Appendix

### Sample Query

This query shows all transactions of PSG token.

```graphql
{
  transfers {
    from
    to
    value
    transactionHash
  }
}
```

Passing this into the query URL returns this result:

```
{
  "data": {
    "transfers": [
      {
        "from": "0x26a3e78fa4d2cbebf6b59b2f84b8fb7c61b52d28",
        "to": "0xdca23d02923d01779fb22959bd2575d64eab4535",
        "value": "1500",
        "transactionHash": "0x000309e9cd3f550e8965381bbd83a35c5cee18f26c33a357f9dbb57450d594ea"
      },
//      ...
  }
}
```

### Sample code

```jsx
const axios = require('axios');

//the graphql query
const graphqlQuery = `{
  transfers {
    from
    to
    value
    transactionHash
  }
}`;
const queryUrl = 'https://gateway-arbitrum.network.thegraph.com/api/[api-key]/subgraphs/id/7x2xzF58Mr694MiAQxCK6fiXux1vaBQm5SdYhbv1LNGE'

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
    // Handle any errors
    console.error(error);
  });
```

### Additional resources:

- To explore all the ways you can optimize & customize your subgraph for a better performance, read more about [creating a subgraph here](https://thegraph.com/docs/en/developing/creating-a-subgraph/).
- For more information about querying data from your subgraph, read more [here](https://thegraph.com/docs/en/querying/querying-the-graph/).