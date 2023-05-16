# How to deploy production on ethereum

## A. Basic install

### 1. Install nodejs https://nodejs.org/en

### 2. Install yarn https://classic.yarnpkg.com/lang/en/docs/install

## B. Deploy contracts

### Step 1: Sử dụng metamask hoặc trustwallet tạo ví với 12 words (MNEMONIC) (lưu ý: ví hoàn toàn mới, không sử dụng ví đã sử dụng)

### Step 2: Mở termial (macos & linux) truy cập vào thư mục source code

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
