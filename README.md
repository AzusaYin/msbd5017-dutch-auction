* 合约项目：`~/nft-contracts`（里面有 `hardhat.config.ts`、`scripts/setupLocalAuction.ts` 等）
* 前端项目：`~/nft-contracts/frontend-hardhat`

---

## ✅ 每次开机后的步骤

### 🟣 第 1 步：启动 Hardhat 本地链（必须一直开着）

**终端 1：**

```bash
cd ~/nft-contracts
npx hardhat node
```

看到类似：

```text
Started HTTP and WebSocket JSON-RPC server at http://127.0.0.1:8545/
```

就说明本地链 OK 了，这个终端不要关。

---

### 🟣 第 2 步：一键部署合约 + 创建拍卖

**另开 终端 2：**

```bash
cd ~/nft-contracts
npx hardhat run scripts/setupLocalAuction.ts --network localhost
cp deploy-local.json frontend-hardhat/deploy-local.json
```

这一步会：

* 重新部署 PromptNFT / ImageNFT / AuctionFactory
* 自动 mint 一套 NFT
* 自动 approve + createAuction
* 在当前目录生成 / 覆盖 `deploy-local.json`

终端里会打印出：

* 各合约地址
* DutchAuction 地址
* tokenId 等信息

---

### 🟣 第 3 步：启动前端（Vite）

**开 终端 3：**

```bash
cd ~/nft-contracts/frontend-hardhat
npm run dev
```

默认会在 `http://localhost:5173` 开一个前端服务。

> 第一次运行过 `npm install` 之后，以后重启电脑不用再装依赖。

---

### 🟣 第 4 步：MetaMask 检查一下（通常不用再改）

1. 浏览器打开 `http://localhost:5173`
2. 确认 MetaMask：

   * 已经选中网络：**Hardhat Localhost**
   * 该网络的配置是：

     * RPC URL: `http://127.0.0.1:8545`
     * Chain ID: `31337`
   * 账户是你导入的那个私钥：
     `0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80`
     （地址：`0xf39F…`）

这些配置只要设好一次，以后重启浏览器 / 电脑 MetaMask 会记住，基本不用再动。

---

### 🟣 第 5 步：在前端操作

1. 页面上点 **Connect**（连接 MetaMask）
2. 上面会显示：
   `Loaded config from deploy-local.json — tokenId: 1, auction: 0x....`
3. 点击 **Load**
   会看到：

   * Token ID
   * Auction Address
   * Current Price
   * End Time
   * Winner 等信息
4. 直接点 **Bid at current price (...) ETH**
   就会用 MetaMask 当前账户对这场拍卖出价。

---

## 🔁 之后每次重启，只要记住一句话：

> **“先 `npx hardhat node`，再 `setupLocalAuction`，拷贝 JSON，再 `npm run dev`。”**