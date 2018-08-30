# Hydro Swap Contract

The Smart Contract used by Hydro Swap to exchange ERC20 tokens for ETH at a given price point.

## What is Hydro Swap?

Hydro Swap allows for easy conversion between ETH and ERC20 tokens. The Hydro Swap API will calculate a current market price for an ERC20 token and allow you to exchange ETH/tokens at that price point. The price point is fluid and the offer is only valid for a set period of time. Because of that, you must interact with the Hydro Swap API to create and sign an order validating the amount of token and price. Once you have the signed transaction data it can be passed to this contract to complete the transaction.

## How does the swap work?

The Hydro Swap widget allows users to quickly and easily exchange tokens for ETH. Swap is built on top of the 0x project's settlement layer, which performs token exchanges atomically within their smart contract. However, the 0x settlement structure seen on various decentralized exchanges (or 0x relayers) typically comes with a few unique challenges. Specifically, the necessity of trading using WETH instead of ETH (since 0x can only handle trading of ERC20 tokens) and the need to authorize 0x's smart contract to withdraw ERC20 tokens on the users' behalf. The Swap contract, beyond facilitating the actual trade, is intended to abstract away some of these challenges to provide a greatly simplified process for buying or selling tokens.

The swap process begins by connecting a digital wallet to the Hydro Swap API. Once a wallet is connected, users can request to buy or sell a certain number of tokens. The API will calculate an ETH price based on a variety of factors, and returns transaction data to be sent to the swap contract. Included in this data is a signed 0x order which will be used to finalize the transaction.

If the user is buying tokens, they will create a transaction with the data returned from the API call, and send the required amount of ETH along with it. The contract will convert that ETH to WETH, complete the 0x transaction, and then transfer the requested amount of token to the users wallet. This completely abstracts away the idea of converting ETH to WETH, and enabling the WETH contract to perform transfers with the 0x contract.

If the user is selling tokens, there is a required extra step. Since the tokens will be taken directly from their wallet, they must still approve the swap contract to transfer tokens on their behalf. Once that is completed, they will create a transaction with the data returned from the API call, as before. The contract will then transfer the required number of tokens from their wallet to the contract, and complete the 0x transaction. The contract converts WETH back to ETH, and then transfers the ETH back to the users wallet, again abstracting WETH away from the user.

## Why use Hydro Swap?

Projects often need a way to distribute their token to users. The current best way to do this is to list their token on an exchange and let people trade for it on the exchange's platform. This standard process often proves to be a massive barrier to community token acquisition and trading. Many users simply want to purchase token directly from the project at a fair market value, rather than learn the nuances of trading on complex exchanges.

Additionally, Hydro Swap includes an additional liquidity layer for projects. While we will check multiple sources for fair market value, the project will also be able to inject liquidity into the system by providing a source of tokens available for purchase by users. We let the project choose among multiple liquidity models to ensure they are getting the best value for tokens sold given their risk tolerance and desired spread.
