# **Local Development Guide — NFT Dutch Auction Platform**

This document describes how to start the full development environment (Hardhat local network + contract deployment + frontend).
Follow these steps each time you boot your machine.

---

# **📌 Project Structure**

```
~/nft-contracts                     ← Smart contract project (Hardhat)
    ├── hardhat.config.ts
    ├── scripts/setupLocalAuction.ts
    ├── contracts/
    ├── deploy-local.json
    └── ...

~/nft-contracts/frontend-hardhat    ← Frontend (Vite + React + Ethers.js)
```

---

# **🟣 Step 1 — Start the Hardhat Local Blockchain (must stay open)**

**Terminal 1:**

```bash
cd ~/nft-contracts
npx hardhat node
```

If you see output such as:

```
Started HTTP and WebSocket JSON-RPC server at http://127.0.0.1:8545/
```

the local blockchain is running correctly.
**Keep this terminal open at all times** — the frontend and contracts depend on it.

---

# **🟣 Step 2 — Deploy All Contracts + Create a Local Auction**

Open **Terminal 2**:

```bash
cd ~/nft-contracts
npx hardhat run scripts/setupLocalAuction.ts --network localhost
cp deploy-local.json frontend-hardhat/deploy-local.json
```

This step will automatically:

* Deploy PromptNFT, ImageNFT, and AuctionFactory
* Mint a test NFT
* Approve the NFT for auction
* Create a Dutch Auction on the local chain
* Generate (or overwrite) `deploy-local.json` with all contract addresses

Your terminal will print:

* NFT contract addresses
* DutchAuction contract address
* Token ID
* Auction parameters

---

# **🟣 Step 3 — Start the Frontend (Vite)**

Open **Terminal 3**:

```bash
cd ~/nft-contracts/frontend-hardhat
npm run dev
```

This starts the frontend at:

```
http://localhost:5173
```

> If you already ran `npm install` once, you do **not** need to reinstall dependencies each reboot.

---

# **🟣 Step 4 — MetaMask Setup (Usually Only Once)**

In your browser:

1. Open the frontend: `http://localhost:5173`
2. Open MetaMask and make sure:

### **Network configuration**

* Network name: **Hardhat Localhost**
* RPC URL: `http://127.0.0.1:8545`
* Chain ID: `31337`
* Currency symbol: ETH

### **Imported Account**

Import the default Hardhat account private key:

```
0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
```

This corresponds to address:

```
0xf39F...92266
```

After this initial setup, MetaMask will remember the configuration on future restarts.

---

# **🟣 Step 5 — Interacting with the App**

1. Click **Connect** to link MetaMask.
2. You should see:

```
Loaded config from deploy-local.json — tokenId: 1, auction: 0x...
```

3. Click **Load Auction**, and the page will display:

* Token ID
* Auction Address
* Current Price (automatically decreasing)
* End Time
* Winner (empty until someone buys)

4. Click **Bid at current price (...) ETH**
   MetaMask will pop up a transaction confirmation.

Once confirmed, the auction ends instantly and ownership transfers on-chain.

---

# **🎉 You are now fully set up!**

Your full local environment (contracts + frontend + blockchain) is ready for testing and development.
