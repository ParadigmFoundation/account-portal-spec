# Portal specification: `Account`

Hello, world!

## Background

## Description

## Code samples
Demonstrations of implementations of various necessary actions using the `kosu.js` and `web3` libraries.

- You must have the latest version of [`kosu.js`](https://www.npmjs.com/package/@kosu/kosu.js) installed in your project, as well as [a specific version of `web3`](https://www.npmjs.com/package/web3/v/1.0.0-beta.37). 
- The [`bignumber.js`](https://www.npmjs.com/package/bignumber.js) package should also be available. 
- Additional notes:
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
- **Method:** `kosu.kosuToken.balanceOf`
- **Example:**
  ```typescript
  // Viewing user's KOSU wallet balance

  const coinbase: string = await web3.eth.getCoinbase();
  const balance: BigNumber = await kosu.kosuToken.balanceOf(coinbase);
  ```

### Set treasury allowance
- **Description:** indicate a number of the user's KOSU tokens (in wei) that the treasury may spend/move on their behalf.
- **Method:** `kosu.treasury.approveTreasury`
- **Notes:**
  - An allowance for the treasury must be set prior to bonding tokens or staking.
  - To set an "unlimited" allowance, you can use the maximum `uint256` value (shown below).
- **Example:**
  ```typescript
  // Setting an "unlimited" Treasury allowance

  const MAX_UINT_256_STR = "115792089237316195423570985008687907853269984665640564039457584007913129639935";
  const MAX_UINT_256: BigNumber = new BigNumber(MAX_UINT_256_STR);
  const MAX_UINT_256_WEI: BigNumber = new BigNumber(web3.utils.toWei(MAX_UINT_256));

  // will prompt for user signature (i.e. MetaMask)
  const receipt: TransactionReceiptWithDecodedLogs = await kosu.treasury.approveTreasury(
      MAX_UINT_256_WEI,
  );
  ```

### View treasury allowance
- **Description:** see the current treasury allowance for the user (detected via `coinbase`).
- **Method:** `kosu.treasury.treasuryAllowance`
- **Note:** this method is useful to detect if an allowance must be set for the user.
- **Example:**
  ```typescript
  // Reading the user's Treasury allowance

  const currentAllowance: BigNumber = await kosu.treasury.treasuryAllowance();
  ```

### Deposit tokens
- **Description:** transfer tokens from the user's wallet to the Kosu treasury.
- **Method:** `kosu.treasury.deposit`
- **Note:** the user must have sufficient [treasury allowance](#view-treasury-allowance) set prior to depositing.
- **Example:**
  ```typescript
  // Depositing 10 KOSU tokens into the Treasury

  const TEN_TOKENS_WEI: BigNumber = new BigNumber(web3.utils.toWei("10"));

  // will prompt for user signature (i.e. MetaMask)
  const receipt: TransactionReceiptWithDecodedLogs = await kosu.treasury.deposit(
      TEN_TOKENS_WEI,
  );
  ```

### Withdraw tokens
- **Description:** transfer tokens from the treasury to the user's wallet.
- **Method:** `kosu.treasury.withdraw`
- **Example:**
  ```typescript
  // Withdrawing 10 KOSU tokens from the Treasury

  const TEN_TOKENS_WEI: BigNumber = new BigNumber(web3.utils.toWei("10"));

  // will prompt for user signature (i.e. MetaMask)
  const receipt: TransactionReceiptWithDecodedLogs = await kosu.treasury.withdraw(
      TEN_TOKENS_WEI,
  );
  ```

### View treasury balance
- **Description:** view the number of tokens (in wei) the user currently has in the treasury.
- **Method:** `kosu.treasury.currentBalance`
- **Example:**
  ```typescript
  // Viewing user's current Treasury balance

  const coinbase: string = await web3.eth.getCoinbase();
  const systemBalance: BigNumber = await kosu.treasury.currentBalance(coinbase);
  ```

### View system balance
- **Description:** view the total number of tokens (in wei) the user has in the whole Kosu contract system.
- **Method:** `kosu.treasury.systemBalance`
- **Example:**
  ```typescript
  // Viewing user's current total system balance

  const coinbase: string = await web3.eth.getCoinbase();
  const systemBalance: BigNumber = await kosu.treasury.systemBalance(coinbase);
  ```

### View bonded token balance
- **Description:** view the total number of tokens (in wei) the user has bonded in the poster registry.
- **Method:** `kosu.posterRegistry.tokensContributed`
- **Example:**
  ```typescript
  // Viewing user's bonded tokens in PosterRegistry

  const bondedTokens: BigNumber = await kosu.posterRegistry.tokensContributed();
  ```

### Bond (register) tokens
- **Description:** bond (lock) tokens into the Kosu poster registry contract for access to the Kosu network.
- **Method:** `kosu.posterRegistry.registerTokens`
- **Example:**
  ```typescript
  // Bonding 5 KOSU tokens in the PosterRegistry

  const FIVE_TOKENS_WEI = new BigNumber(web3.utils.toWei("5"));
  
  const receipt: TransactionReceiptWithDecodedLogs = await kosu.posterRegistry.registerTokens(
      FIVE_TOKENS_WEI,
  );
  ```

### Un-bond (release) tokens
- **Description:** un-bond (unlock) tokens from the Kosu poster registry.
- **Method:** `kosu.posterRegistry.releaseTokens`
- **Example:**
  ```typescript
  // Un-bonding 5 KOSU tokens from the PosterRegistry

  const FIVE_TOKENS_WEI = new BigNumber(web3.utils.toWei("5"));
  
  const receipt: TransactionReceiptWithDecodedLogs = await kosu.posterRegistry.releaseTokens(
      FIVE_TOKENS_WEI,
  );
  ```