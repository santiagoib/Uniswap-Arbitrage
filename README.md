# UniV2-Arbitrage-Playground

**Educational arbitrage strategies on Uniswap V2**

This repository brings together two smart contracts written in Solidity that illustrate how to carry out arbitrage strategies on Uniswap V2. The goal is to provide a practical example that serves both as a learning resource and a professional portfolio piece, showcasing how to interact with different components of the DEX and how to verify the profitability of each operation.

## 🎯 What does this project do?

- **Arbitrage using routers:**  
  `UniswapV2Arb1.sol` executes a double swap. It takes a `tokenIn` and swaps it for `tokenOut` on one router, then reverses the operation on a second router. If the final balance exceeds the initial amount plus a minimum profit (`minProfit`), the profit is sent to the user.

- **Arbitrage using flash swaps:**  
  The same contract, `UniswapV2Arb1.sol`, can initiate a flash swap: it borrows a token from a Uniswap V2 pair, performs a double swap on the routers, pays the fees, and returns the loan, keeping the gains.

- **Arbitrage between pairs without routers:**  
  `UniswapV2Arb2.sol` demonstrates direct arbitrage between two Uniswap pairs. It manually calculates the output token amount using the constant product formula `x*y=k` and executes a swap on the second pair after a flash loan from the first. It then repays the loan and retains the profit.

## ⭐ Key Features

- **Real interaction with Uniswap V2:** Uses both `IUniswapV2Router02` and `IUniswapV2Pair` for swaps, flash loans, and reserve queries.  
- **Manual calculation of amounts:** Employs the `getAmountOut` formula with the 0.3% fee (997/1000) to understand the mechanics under the hood.  
- **Profit parameters:** Each function includes a `minProfit` parameter to protect the user from executing unprofitable operations.  
- **Security hooks and validations:** It is recommended to strengthen the authenticity of callback calls (`uniswapV2Call`) and use `SafeERC20` to handle non-standard tokens.  
- **Fork-testing compatibility:** The contracts are structured to work properly in a mainnet fork environment.  

## 🧪 Test Suite

A minimalist test suite is included for each contract (under `/test`) to validate behavior in a mainnet fork:

- **Arb1.t.sol**: Verifies the full router-to-router arbitrage flow, as well as the flash swap in `UniswapV2Arb1`.  
- **Arb2.t.sol**: Checks the direct pair-to-pair arbitrage in `UniswapV2Arb2`, covering token order and fees.  

The tests cover successful cases, failures due to insufficient profit, and callback authenticity. Running