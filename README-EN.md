# How to deploy production on ethereum

## A. Basic install

### 1. Install nodejs https://nodejs.org/en

### 2. Install yarn https://classic.yarnpkg.com/lang/en/docs/install

## B. Deploy contracts

### Step 1: Use Metamask or Trust Wallet to create a wallet with 12 words (MNEMONIC) (note: the wallet is completely new, not using an existing one).

### Step 2: Open Terminal (macOS & Linux) and access the source code directory.

Run the 'yarn' command in the terminal

```bash
yarn
```

### Step 3: Set up environment variables.

a. Set the 12-word mnemonic phrase generated during wallet creation to the 'MNEMONIC' variable, as shown in the example below.

Ex:

```bash
export MNEMONIC='spin express raise cage lawsuit absorb surround dumb knee fog dinner must'
```

b. Set the lock time after deposit in full protect mode (environment variable: 'FULL_PROTECT_LOCK_TIME') to the desired duration in seconds (default is 300 seconds).

Ex:

```bash
export FULL_PROTECT_LOCK_TIME=300
```

c. Set the token reward per block in big and small protect modes (environment variable: 'REWARD_PER_BLOCK') to the desired amount in SDOGE (default is 10 SDOGE).

Ex:

```bash
export REWARD_PER_BLOCK=10
```

d. Set the address of the stable token (environment variable: 'STABLE_TOKEN_ADDRESS').

Ex:

```bash
export STABLE_TOKEN_ADDRESS=0xdac17f958d2ee523a2206206994597c13d831ec7
```

### Step 4: To find the wallet address for deployment.

```bash
yarn workspace erc20 scripts:find:wallet:mainnet
```

Example Output:

```json
Found wallet to deploy token SDOGE {
  estimatedAddress: '0x794f75bbadd58dc91802c6979b1ac708f74383fe',
  signer: '0x4ab2D60D60511cB6877c651F769CE9B4b8f6d404', // This signer is the wallet used for deployment
  index: 0
}
```

### Step 5: Transfer the native currency (ETH) to the "signer" wallet that was found above.

In the example above is "0x4ab2D60D60511cB6877c651F769CE9B4b8f6d404"

Note: It is recommended to transfer an extra balance of approximately $100 - $200 worth of ETH to ensure uninterrupted deployment.

### Step 6: Run the script for deployment.

```bash
yarn workspace erc20 deploy:mainnet
```

## C. Deploy frontend

### 1. Set the wallet addresses for various types of tokens.

$ In file 'apps/web/src/config/addresses/tokens.ts' line 30

```ts
export const mainnetTokens: Tokens = {
  sdoge: new Token(ChainId.ETHEREUM, '', 18, 'SDOGE', 'SDOGE'),
  weth: new Token(ChainId.ETHEREUM, '', 18, 'WETH', 'WETH'),
  pairWStable: new Token(ChainId.ETHEREUM, '', 18, 'SDOGE-BUSD LP', 'SDOGE-BUSD LP'),
  usdt: new Token(ChainId.ETHEREUM, '', 6, 'USDT', 'Tether USD'),
  pairWEthUsdt: new Token(ChainId.ETHEREUM, '', 18, 'WETH-USDT', 'WETH-USDT'),
}
```

Set the contract address, name, and decimals for each token accordingly.

Note:

- pairWStable: The pair contract address between SDOGE and stable token
- pairWEthUsdt: The pair contract address between WETH and USDT.

$ In file 'apps/web/src/config/addresses/contracts.ts' line 1

```ts
chefV2: {
    97: '0x94a54F6C8FA9945c9ef328Ce5e5391B6f7E72e0b',
    1: '',
  },
  sdoge: {
    97: '0xF8D5d8783f8e8A64c6904A7e96f108D124024FE6',
    1: '0x56141231B6484c1d486bca435c856771f76400cC',
  },
  fullProtectSDoge: {
    97: '0xD0901FD6E4220274d0F7423c8cb4A0347Ad5966E',
    1: '0x9A7d489bEb6DBCCF53d206d3416711380beC2759',
  },
  multicall: {
    1: '0xcA11bde05977b3631167028862bE2a173976CA11',
    4: '0xcA11bde05977b3631167028862bE2a173976CA11',
    5: '0xcA11bde05977b3631167028862bE2a173976CA11',
    56: '0xcA11bde05977b3631167028862bE2a173976CA11',
    97: '0xcA11bde05977b3631167028862bE2a173976CA11',
  },
  pairWETH: {
    97: '0xfbfEF65D57Ae4F72bCcabfE1BDBe54219857e41A',
    1: '0xfbfEF65D57Ae4F72bCcabfE1BDBe54219857e41A',
  },
  routerV2: {
    97: '0xD99D1c33F9fC3444f8101754aBC46c52416550D1',
    1: '0x78eFFdeee8f8C79d781e12B9EBd9244a1ee3Eca8',
  },
  usdt: {
    97: '0xaB1a4d4f1D656d2450692D237fdD6C7f9146e814',
    1: '0xaB1a4d4f1D656d2450692D237fdD6C7f9146e814',
  },
```

Set the corresponding contract address

Note:

- ChefV2: The contract address of the Big Protect and Small Protect.
- fullProtectSDoge: The contract address of the Full Protect

### 2. Deploy frontend on vercel

Customers can build their project on 'https://vercel.com' and point their domain name to it.
