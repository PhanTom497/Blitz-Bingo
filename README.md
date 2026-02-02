# ⚡ Blitz Bingo

> **High-Stakes Dice Bingo on the Linera Network**

[![Live Demo](https://img.shields.io/badge/Live%20Demo-Play%20Now-purple?style=for-the-badge&logo=vercel)](https://blitz-bingo.vercel.app/)
[![Linera](https://img.shields.io/badge/Built%20On-Linera-blue?style=for-the-badge)](https://linera.io)

**Blitz Bingo** is a fully decentralized, high-performance solo gaming application built on the **Linera** blockchain. It demonstrates the power of Linera's **micro-rollup** architecture to deliver a "Web2-like" user experience with sub-second finality, eliminating the lag typically associated with blockchain gaming.

---

## 🎮 The Game

Blitz Bingo combines the thrill of dice rolls with the strategy of Bingo.
- **Roll & Match**: Roll 5 dice to match numbers on your unique 5x5 Bingo card.
- **Instant Actions**: Every roll, bet, and claim is a blockchain transaction that confirms instantly.
- **Fair Play**: Game logic and randomness are enforced by on-chain WASM smart contracts.
- **Provable Fairness**: All outcomes are verifiable on the Linera network.

---

## 🏗 Architecture & Linera Protocol Features

Blitz Bingo is natively designed for Linera's **multi-chain (micro-chain)** paradigm. Unlike traditional EVM dApps that congested a single global state, Blitz Bingo gives every player their own "User Chain".

### Key Linera Features Used:
1.  **Micro-Rollups**:
    - **User Chains**: Each player acts on their own lightweight chain (`User Chain`). This ensures parallel execution and infinite scalability.
    - **Application Chain**: A central chain acts as the "Server" to coordinate global state, but gameplay actions happen on user chains first.
2.  **Fast Finality**:
    - Linera’s specific consensus allows for near-instant block confirmation, enabling a snappy "click-and-play" experience without waiting for 15-second block times.
3.  **WASM Smart Contracts (Rust)**:
    - Game logic is written in Rust and compiled to WebAssembly (Wasm32).
    - Contracts handle complex state transitions: `Bet -> Roll -> Update Card -> payout/loss`.
4.  **GraphQL Integration**:
    - The frontend communicates with the blockchain via a generated GraphQL schema, allowing for precise and efficient data fetching.

### Architecture Diagram
```mermaid
graph TD
    User[Player] -->|1. Action (Bet/Roll)| Frontend[React Frontend]
    Frontend -->|2. GraphQL Mutation| Client[Linera Client / Wallet]
    Client -->|3. Propose Block| UserChain[User Micro-Chain]
    UserChain -->|4. Cross-Chain Message| AppChain[Application Chain]
    AppChain -->|5. Verify & Execute| Logic[WASM Contract]
    Logic -->|6. Return Result| UserChain
    UserChain -->|7. Finalize| Frontend
```

---

## 🛠 Tech Stack

### Backend (On-Chain)
- **Language**: Rust
- **Platform**: Linera SDK (WASM)
- **State Management**: `MapView` for scalable storage
- **Serialization**: `Bincode` & `Serde`

### Frontend (Client)
- **Framework**: React 18 + TypeScript + Vite
- **Styling**: Tailwind CSS + Custom "Glassmorphism" Design
- **Animations**: Framer Motion
- **Connection**: Custom `useLinera` hook interfacing with Linera GraphQL Service

---

## 🚀 Setup & Local Development

To run Blitz Bingo locally or deploy your own instance:

### Prerequisites
- Rust & Cargo
- Linera Toolchain (`linera`, `linera-proxy`, `linera-service`)

### 1. Build Contracts
```bash
git clone https://github.com/your-repo/blitz-bingo.git
cd blitz-bingo/Game/blitz-bingo
cargo build --release --target wasm32-unknown-unknown
```

### 2. Deploy to Linera Testnet
```bash
linera wallet init --faucet https://faucet.testnet-conway.linera.net
linera publish-and-create \
  target/wasm32-unknown-unknown/release/blitz_bingo_contract.wasm \
  target/wasm32-unknown-unknown/release/blitz_bingo_service.wasm \
  --json-argument 'null'
```
*Note the Application ID generated from this step.*

### 3. Run Frontend
```bash
cd ../blitz-bingo-frontend
# Create .env file with your VITE_BLITZ_APP_ID
npm install
npm run dev
```

---

## 👥 Team & Contact

**Submission for Linera Buildathon Wave 6**

- **Ethereum Wallet (for Grant Distribution)**: `0xE1782602C8994aA3997fd30Df84BcB5d498F674f`

*Building the future of decentralized gaming with fast finality.*
