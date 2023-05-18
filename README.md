# How to deploy production on ethereum

## A. Basic install

### 1. Install nodejs https://nodejs.org/en

### 2. Install yarn https://classic.yarnpkg.com/lang/en/docs/install

## B. Deploy contracts

### Step 1: Sử dụng metamask hoặc trustwallet tạo ví với 12 words (MNEMONIC) (lưu ý: ví hoàn toàn mới, không sử dụng ví đã sử dụng)

### Step 2: Mở termial (macos & linux) truy cập vào thư mục source code

chạy lệnh 'yarn' trên terminal

```bash
yarn
```

### Step 3: Cài đặt biến môi trường

a. Cài 12 words sau khi tạo ví vào biến 'MNEMONIC' như ví dụ ở dưới

Ex:

```bash
export MNEMONIC='spin express raise cage lawsuit absorb surround dumb knee fog dinner must'
```

b. Cài đặt thời gian lock sau khi deposit ở full protect (biến môi trường là 'FULL_PROTECT_LOCK_TIME') (đơn vị là giây) (mặc định là 300 giây)

Ex:

```bash
export FULL_PROTECT_LOCK_TIME=300
```

c. Cài đặt phần thưởng token mỗi block ở big & small protec ( biến môi trường là 'REWARD_PER_BLOCK') (đơn vị là SDOGE) (măc định là 10 SDOGE)

Ex:

```bash
export REWARD_PER_BLOCK=10
```

d. Cài đặt địa chỉ token stable ( biến môi trường là 'STABLE_TOKEN_ADDRESS')

Ex:

```bash
export STABLE_TOKEN_ADDRESS=0xdac17f958d2ee523a2206206994597c13d831ec7
```

### Step 4: Tìm địa chỉ ví để deploy

```bash
yarn workspace erc20 scripts:find:wallet:mainnet
```

Example Output:

```json
Found wallet to deploy token SDOGE {
  estimatedAddress: '0x794f75bbadd58dc91802c6979b1ac708f74383fe',
  signer: '0x4ab2D60D60511cB6877c651F769CE9B4b8f6d404', // signer này là ví để deploy
  index: 0
}
```

### Step 5: Transfer Native Currency (ETH) tới ví "signer" đã tìm được ở phía trên,

trong ví dụ trên là "0x4ab2D60D60511cB6877c651F769CE9B4b8f6d404"

Lưu ý: Nên chuyển khoảng dư dư khoảng 100 - 200$ as ETH để deploy không bị gián đoạn

### Step 6: Chạy scripts deploy

```bash
yarn workspace erc20 deploy:mainnet
```

## C. Deploy frontend

### 1. Cài đặt địa chỉ ví các loại token

$ Trong file 'apps/web/src/config/addresses/tokens.ts' line 30

```ts
export const mainnetTokens: Tokens = {
  sdoge: new Token(ChainId.ETHEREUM, '', 18, 'SDOGE', 'SDOGE'),
  weth: new Token(ChainId.ETHEREUM, '', 18, 'WETH', 'WETH'),
  pairWStable: new Token(ChainId.ETHEREUM, '', 18, 'SDOGE-BUSD LP', 'SDOGE-BUSD LP'),
  usdt: new Token(ChainId.ETHEREUM, '', 6, 'USDT', 'Tether USD'),
  pairWEthUsdt: new Token(ChainId.ETHEREUM, '', 18, 'WETH-USDT', 'WETH-USDT'),
}
```

Set ví, tên, decimals tương ứng

Chú thích

- pairWStable: là pair contract address giữa sdoge-stable token
- pairWEthUsdt: là pair contract address giữa weth-usdt

$ Trong file 'apps/web/src/config/addresses/contracts.ts' line 1

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

Set contract address tương ứng

Chú thích:

- ChefV2: là contract address của big, small protect
- fullProtectSDoge: là contract address của full protect

### 2. Deploy frontend on vercel

Khách hàng có thể build trên 'https://vercel.com' và trỏ tên miền vào.
