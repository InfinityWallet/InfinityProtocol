# Infinity Protocol

The core contract is a factory which can create pair contracts as required for any token pair. Liquidity can be provided for these token pairs, allowing users to swap between them. To swap and add/remove liquidity we use a router contract to execute the desired transactions and perform safety checks. You must first approve the router address for any tokens you want to send.

The Infinity Protocol will provide the infrastructure for decentralized finance, empowering developers, liquidity providers, financial institutions, exchanges, wallets and traders to participate in an open and accessible financial marketplace. As a decentralized financial bridge between digital assets, providing an open, transparent, secure and censorship-resistant protocol for anyone looking to tap into a global and open liquidity, exchange and lending protocol, with many more protocols to come and advancements planned.


| Title | Network | Github | Contract |
| :---: | :---: | :---: | :---: |
| **InfinityFactory** | BSCTEST | https://github.com/InfinityWallet/Infinity-Core | [0xCe8B58aa5CCcc32f5537347054fa171cE21A15e6](https://testnet.bscscan.com/address/0xCe8B58aa5CCcc32f5537347054fa171cE21A15e6) |
| **InfinityRouter** | BSCTEST | https://github.com/InfinityWallet/Infinity-Periphery | [0xEf06e77dd92C7E2Fc986748a5Df8aBC862ffA221](https://testnet.bscscan.com/address/0xEf06e77dd92C7E2Fc986748a5Df8aBC862ffA221) |



## Infinity Protocol Liquidity Tokens

Infinity Protocol liquidity tokens for a pair can be identified by their name and symbol on the blockchain. This allows users to easily see what liquidity they have on many wallets and explorers.

For example the liquidity token for a pool containing SHARD and BUSD would have:

**Name** - Infinity (SHARD-BUSD)

**Symbol** - ♾️SHARD-BUSD


**Non-standard tokens**

When interacting with non-standard tokens which burn on transfer use the functions appended with “SupportingFeeOnTransferTokens”.

**Dual LP Pools**
Infinity liquidity pool tokens are named based on their component tokens. This allows them to be easily identified and listed on tracking sites and exchanges, as well as combined into multi-tier multi-token pools.


## The Guide
This guide will look into the basics of directly interacting with the router contract to add/remove liquidity and swap. For a complete list of router functions please refer to the router interface ***“IInfinityRouter01.sol”***.


### Adding liquidity
When adding liquidity if the token pair does not exist it will be automatically created, the ratio of the tokens added will determine the initial price of the pair.

You can add liquidity at the exact ratio of the pair reserves using the addLiquidity function, or add at any ratio or in a single token with the addLiquidityFlexible function. We will look at how to flexibly add liquidity.

The **addLiquidityFlexible** function accepts these parameters:

| Type | Parameter | Description |
| :--- | :--- | :--- |
| address | **tokenA** | The contract address of one of the tokens you wish to provide. |
| address | **tokenB** | The contract address of the other token. |
| uint | **amountA** | The wei amount of tokenA to provide. |
| uint | **amountB** | The wei amount of tokenB to provide. |
| uint | **liquidityMin** | The minimum amount of liquidity pool tokens you will accept in return. |
| address | **to** | The address to receive the liquidity tokens. |
| uint | **deadline** | The Unix-time deadline which the transaction must be confirmed before. |


The expected liquidity to be minted can be calculated with the Infinity SDK using the getLiquidityMintedFlexible function. You can then modify this value by your slippage tolerance to provide a liquidityMin. However for ease of use on the testnet you can choose a liquidityMin of 0.


### Removing liquidity
You can redeem your liquidity pool tokens at any time and choose to receive either both of the tokens from a pair or a single token. The amounts received depend on the current pool reserves and their ratio in the pool. To remove both tokens at the current ratio use the removeLiquidity function, or to remove liquidity in a single token use the removeLiquiditySingle function.

The **removeLiquidity** function accepts these parameters:

| Type | Parameter | Description |
| :--- | :--- | :--- |
| address | **tokenA** | The contract address of one of the tokens in the pool. |
| address | **tokenB** | The contract address of the other token in the pool. |
| uint | **liquidity** | The amount of liquidity pool tokens you want to redeem. |
| uint | **amountAMin** | The minimum amount of tokenA you will accept in return. |
| uint | **amountBMin** | The minimum amount of tokenB you will accept in return. |
| address | **to** | The address to receive the tokens. |
| uint | **deadline** | The Unix-time deadline which the transaction must be confirmed before. |


The **removeLiquiditySingle** function accepts these parameters:

| Type | Parameter | Description |
| :--- | :--- | :--- |
| address | **tokenA** | The contract address of the other token in the pool. |
| address | **tokenOut** | The contract address of the token you want out of the pool. |
| uint | **liquidity** | The amount of liquidity pool tokens you want to redeem. |
| uint | **amountOutMin** | The minimum amount of the tokenOut you will accept in return. |
| address | **to** | The address to receive the tokens. |
| uint | **deadline** | The Unix-time deadline which the transaction must be confirmed before. |

The expected tokens to be received can be calculated with the Infinity SDK using the getLiquidityValue and getLiquidityValueSingle functions. You can then modify this value by your slippage tolerance to provide an amountOutMin. However for ease of use on the testnet you can choose an amountOutMin of 0.


### Swapping
There are various functions in the router which can be used for swapping. Multiple trades can be executed in a single transaction which can be useful especially if there is no direct pair available for your desired swap. We will look at how to swap an exact amount of tokenIn for tokenOut.

First we will use the getAmountsOut function in the router to calculate the expected amount to receive.

The **getAmountsOut** function accepts these parameters:

| Type | Parameter | Description |
| :--- | :--- | :--- |
| uint | **amountIn** | The amount of tokenIn that you would like to swap. |
| address[] | **path** | An array of token addresses representing the hops for this trade. For a basic 1 hop trade this would be the contract address of [tokenIn, tokenOut]. |


You can then modify the result of this function by your slippage tolerance to calculate an amountOutMin. For example multiply the result by 0.9 for a -10% slippage tolerance.

We will then use the **swapExactTokensForTokens** function accepting these parameters:


| Type | Parameter | Description |
| :--- | :--- | :--- |
| uint | **amountIn** | The amount of tokenIn that you would like to swap. |
| uint | **amountOutMin** | The minimum amount of the tokenOut you will accept in return. |
| address[] | **path** | An array of token addresses representing the hops for this trade. For a basic 1 hop trade this would be the contract address of [tokenIn, tokenOut]. |
| address | **to** | The address to receive the tokens. |
| uint | **deadline** | The Unix-time deadline which the transaction must be confirmed before. |


# Why Binance Smart Chain (BSC)?
We look to make the user experience the best it can be for novice and advanced users alike. Binance Smart Chain was an obvious choice as it provides the security we require on a fast, low fee blockchain. Along with an impressive suite of bridged coins and tokens, which we look to further expand on.


# Infinity Crypto
Infinity Crypto is an innovative new all-in-one decentralized financial ecosystem composed of multiple component platforms built on top of the Infinity Protocol, bringing new advancements and ease-of-use features to make earning and trading easier than ever. The platform will provide users with easy and instant access to a wide range of decentralized financial services on the Binance Smart Chain, while holding your tokens securely on either the decentralized companion [Infinity Wallet](https://infinitywallet.io/) or your wallet of choice.


## Notable advancements include
- Sublime usability: Infinity is designed to be easy to use for defi beginners and advanced users alike and the entire platform has been hand-crafted to a premium quality standard to ensure any user can user decentralised finance as easy as centralised finance allowing for easier mass adoption of decentralised finance globally with new and innovative features.

- Flexible liquidity providing: Want to put all your BNB and SHARD balance into a BNB-SHARD pool but don't know how to calculate the precise amount you need to swap between one and the other before adding? Or maybe you just want to add SHARD without BNB? With Infinity its simple, just select what you want to put in and click to add liquidity.

- Single token removal: Do you just want SHARD out of the SHARD-BNB pool? No need to manually remove both and swap, save gas and time by using the single remove to cash your liquidity in directly for SHARD.

- Advanced analytics: Easily live monitor the overall platform, coins, pools and other stats in an intuitive and easy to use UI.

- Advanced trading platform: Everything you need in one place, whether its advanced charting, prices, reserve stats, multi-dimensional order book, market trade and liquidity histories, your token balances and transaction history, trading or adding liquidity with a single click. It's all under your fingertips on the advanced trading page.

- Infinity bridge: Integrating the Binance bridge and expanding it with additional Infinity bridged coins to make BSC the ultimate hub of cross-chain decentralized finance globally.

- Staking pools: Earn additional rewards when providing liquidity to select PoS staking liquidity pools, deposited straight to the pool so you can passively compound all your rewards in a single pool earning the benefit of staking rewards + liquidity providing fees.

- Pool historical price charts: Track the change in $USD value of a pools liquidity tokens over time with advanced charting, making assessing the true performance of a pool easier than ever.

- Infinity Wallet: Easily connect with a click of a button to the decentralized companion wallet, where you can securely manage your coins and tokens and interact to trade, earn or provide liquidity.

## Screens

### Home
![](https://i.gyazo.com/2da361a82932057ce31de339304ddbce.png)

### Advanced Trading
![](https://i.gyazo.com/fde2b9a8aaf8b9bd794bcd4ee09a4db6.png)

### Markets
![](https://i.gyazo.com/32edcbc06329b469ededcfa6bda39f9e.png)

### Swap
![](https://i.gyazo.com/8ee00df114a9c2743919267051223335.png)

### Add Liquidity
![](https://i.gyazo.com/a2fff6e76d704b12d373b7995fb1d12d.png)
![](https://i.gyazo.com/1f266356a94c8086a98275d0acf61dcf.png)
![](https://i.gyazo.com/61adec14a59ab9a6c72f157acb5dd80d.png)

### Remove Liquidity

Dual Token Removal
![](https://i.gyazo.com/dd0f5c788d78dcefba002d989f926594.png)

Single Token Removal
![](https://i.gyazo.com/ea4418b37e68eff5cc57e950bef29d2d.png)

### Bridge
![](https://i.gyazo.com/db9f8491bcf58ee9c7998cd4ea32b0af.png)
![](https://i.gyazo.com/4cd19c40226d2edd2e7422270e37fbc1.png)

### Overall Analytics
![](https://i.gyazo.com/1fa0a7dbeded09be06a0ab4a5a8cb86a.png)

### Pools
![](https://i.gyazo.com/a5d86566fe600fee837d75847d97e9f3.png)

### Coins
![](https://i.gyazo.com/ab33b639906a86d06ec86ac5dc486cdc.png)

### Individual Coin & Pool Pages
![](https://i.gyazo.com/39e5a4ba27643634b93ca67ca056877b.png)

### Whitelist Burning Governance
![](https://i.gyazo.com/e45556562be4b164f707c789960e5ebc.png)
![](https://i.gyazo.com/c179ceca17185c7e679c4ee038a41652.png)

### Wallet Mangement with Balance & Portfolio
![](https://i.gyazo.com/7e403f3b42322b4333931c5ed2aaf982.png)
![](https://i.gyazo.com/f971ef449d052bb25a4dd52386787ad1.png)

### Wallet Trade History
![](https://i.gyazo.com/7166ee4b922a07fa0cf361a3986a4852.png)

### Wallet Liquidity Positions
![](https://i.gyazo.com/b7d0be77699b81b8442b6d0ac66b2d28.png)
![](https://i.gyazo.com/b2b33bab790d3319b5824cea1145338a.png)

### Wallet Liquidity History
![](https://i.gyazo.com/d4c7ac2b55c779cf703ec1240234465e.png)

### Wallet Bridge History
![](https://i.gyazo.com/0a23534b503f2d45313ae5765e5b2da5.png)

### Wallet Rewards
![](https://i.gyazo.com/0d7af58966e2b5d9bd179407d411f243.png)


## Future Features
There are many other features which we are not yet ready to reveal, however the Infinity Protocol and Infinity Crypto look to innovate the industry with new technology, advancing blockchain with a leading protocol and platform starting on the Binance Smart Chain brining a real value platform to the chain, some features on the roadmap are:

- Decentralized Token Offerings: This will allow new tokens to easily launch in a fully decentralized environment, based on a set of pre-determined rules.

- Decentralized Referral System: Invite friends to earn a percentage of their trading fees forever all on the blockchain.

- Limit Orders: Integration of on-chain orderbook with limit orders to expand the available options for all traders alongside pool liquidity.

- Margin Trading: Trade tokens with leverage on-chain rewards providers.

- Decentralized Loans: Users will be able to provide liquidity and earn interest, or take out a loan at competitive rates fully on the blockchain.

- Buy/Sell Crypto to Fiat: In the near future, we look to integrate the ability for new users to easily get started within the defi platform by providing the ability to buy/sell crypto from and to fiat.

- Manage your funds and tokens by connecting your wallet and being able to send, receive and trade within one unique defi platform.

- Gamification: Integrating new ways to earn rewards such as NFT's, achievements, trading battles, competitions and much more.

- Yield Farming: Earn yield by providing liquidity into specific pools to be rewarded.

- Decentralised Index's: Decentralized governed indexs fully on the blockchain, allowing to easily diverisfy across a wide range of cryptocurrencies in seconds.

- Decentralised Index Trading: Trade crypto indexs on a decentralized protocol & platform with one of the leading UI & UX in the crypto industry. 

- Integration of the Infinity Protocol into the Infinity Wallet for direct wallet trading, staking, loans, liquidity providing and much more.
