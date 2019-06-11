# Portal specification: `Account`

Hello, world!

## Background

## Description

## Code samples

Demonstrations of implementations of various necessary actions using the `kosu.js` and `web3` libraries.

You must have the latest version of [`kosu.js`](https://www.npmjs.com/package/@kosu/kosu.js) installed in your project, as well as [a specific version of `web3`](https://www.npmjs.com/package/web3/v/1.0.0-beta.37).

Additional notes:
- Examples include TypeScript type definitions where possible.
- Where `kosu` is used, it refers to an instance of the `Kosu` class.
- Similarly, `web3` refers to an instance of the `Web3` class.
- The term `coinbase` refers to the user's primary unlocked account (in this context, their Metamask address).
- Most numbers (from `kosu.js` and `web3`) are expected as, and returned as [`BigNumber`](https://www.npmjs.com/package/bignumber.js) instances.
- Most token values are returned as, and expected in units of wei, so remember to convert where necessary.
- Methods that use the `await` keyword imply they are in `async` functions, even if not shown.
- Promise syntax may be used if necessary, but we recommend `async/await` where possible.

### View token balance
- **Description:** view the user's balance of KOSU tokens in wei (does not include treasury balance).
- **Example:**
  ```typescript
  const coinbase: string = await web3.eth.getCoinbase();
  const balance: BigNumber = await kosu.kosuToken.balanceOf(coinbase);
  ```

### Set treasury allowance
- **Description:**
- **Example:**
  ```javascript
  
  ```

### Deposit tokens
- **Description:**
- **Example:**
  ```javascript
  
  ```

### Withdraw tokens
- **Description:**
- **Example:**
  ```javascript
  
  ```

### View treasury balance
- **Description:**
- **Example:**
  ```javascript
  
  ```

### Bond (register) tokens
- **Description:**
- **Example:**
  ```javascript
  
  ```

### Un-bond (release) tokens
- **Description:**
- **Example:**
  ```javascript
  
  ```