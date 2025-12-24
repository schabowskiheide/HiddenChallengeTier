# 🧗 HiddenChallengeTier
Hidden Challenge Tier is a confidential progress-tracking dApp where players encrypt the number of completed challenges and send it to an FHEVM smart contract. 
The contract compares this encrypted value against encrypted thresholds and derives a tier without ever seeing the raw count. 
Only an encrypted tier code (None / Rookie / Pro / Legend) can be decrypted later through the relayer.

---
## Contract
- **Contract name:** `HiddenChallengeTier`
- **Network:** Sepolia
- **Contract address:** `0x3b2b61469d8a633d895f7F3b0e8ed94e1F30f187` 
- **Relayer SDK:** `@zama-fhe/relayer-sdk` (v0.3.x required)

---

## Features

Encrypted submission of completed-challenges count per player key (with owner-only updates).
Homomorphic comparison against on-chain thresholds for Rookie, Pro, and Legend tiers.
Encrypted tier code (0=None, 1=Rookie, 2=Pro, 3=Legend) that can be made publicly decryptable.
Simple web frontend using Relayer SDK and Ethers v6 for end-to-end encrypted UX.

---

Modern glassmorphic UI built with pure HTML + CSS.  
Powered by Zama Relayer SDK v0.3.0-5 and Ethers.js v6.15.


Zero knowledge of inputs — full privacy preserved

Modern dual-column glassmorphic UI built with pure HTML + CSS

Powered by Zama Relayer SDK v0.3.0 and Ethers.js v6

🛠 Quick Start
Prerequisites

Node.js ≥ 20

npm / yarn / pnpm

MetaMask or any injected Ethereum-compatible wallet

## Installation (development)
1. Clone repo  
```bash
git clone <repo-url>
cd health-metric-zone
Install dependencies (example)

npm install
# or
yarn install

Install Zama Relayer SDK on frontend

npm install @zama-fhe/relayer-sdk @fhevm/solidity ethers

Build & deploy (Hardhat)

npx hardhat clean
npx hardhat compile
npx hardhat deploy --network sepolia
Make sure your hardhat.config.js includes the Zama config and the Solidity version ^0.8.27.


Make sure `hardhat.config.js` has the fhEVM config and Solidity `^0.8.27`.

---

## Frontend (Quickstart)

1. Set CONFIG.CONTRACT_ADDRESS in frontend/index.html to the deployed 
HiddenChallengeTier address (already set to 0x3b2b61469d8a633d895f7F3b0e8ed94e1F30f187 in this example).

2. Serve the frontend (npx serve frontend or any static file server).

Player (submit or update progress):

Derive userId = keccak256(utf8(playerKey)) on the frontend.

Encrypt the completed-challenges count via Relayer using add16(completed) and call:
submitCompleted(bytes32 userId, bytes32 encCompleted, bytes attestation)

The same playerKey can be reused to update progress; only the original owner address can overwrite the value.

Compute hidden tier:

Encrypt zero once via add16(0) and call:
computeTier(bytes32 userId, bytes32 encZero, bytes attestation)

The contract compares the encrypted count to Rookie/Pro/Legend thresholds and stores an encrypted tier code.

Read tierHandle(bytes32 userId) to obtain the ciphertext handle for the tier.

Reveal tier:

Call makeTierPublic(bytes32 userId) from the same address that owns this userId.

Decrypt the tier on the frontend:

const out = await relayer.publicDecrypt([handle]);
const v = out.clearValues[handle] ?? out.clearValues[handle.toLowerCase()];
const jackpotReady = BigInt(v) === 1n;
Map tierCode to labels (e.g. 0 → "None", 1 → "Rookie", 2 → "Pro", 3 → "Legend") and display to the user.

---

## Project Structure

---

# Security & Privacy
The contract never stores plain health data.
FHE.allow and FHE.allowThis are used so only authorized parties (owner + contract) can decrypt.
Users must protect their wallets and local attestation proofs — if lost, privacy is still preserved (attestations are on inputs).

# Common Commands:

Compile: npx hardhat compile
Deploy: npx hardhat deploy --network sepolia
Serve frontend: npx serve frontend or any static server

Troubleshooting
If publicDecrypt returns undefined: ensure you passed a clean bytes32 handle and that the contract used FHE.makePubliclyDecryptable(...).
If Relayer worker fails in browser: ensure server sends Cross-Origin-Opener-Policy: same-origin and Cross-Origin-Embedder-Policy: require-corp headers.

📁 Project Structure
tinderdao-private-match/
├── contracts/
│   └── HiddenChallengeTier.sol              # Main FHE-enabled matchmaking contract
├── deploy/                                  # Deployment scripts
├── frontend/                                # Web UI (FHE Relayer integration)
│   └── index.html
├── hardhat.config.js                        # Hardhat + FHEVM config
└── package.json                             # Dependencies and npm scripts

📜 Available Scripts
Command	Description
npm run compile	Compile all smart contracts
npm run test	Run unit tests
npm run clean	Clean build artifacts
npm run start	Launch frontend locally
npx hardhat deploy --network sepolia	Deploy to FHEVM Sepolia testnet
npx hardhat verify	Verify contract on Etherscan
🔗 Frontend Integration

The frontend (pure HTML + vanilla JS) uses:

@zama-fhe/relayer-sdk v0.3.0

ethers.js v6.13

Web3 wallet (MetaMask) connection

Workflow:

Connect wallet

Encrypt & Submit a preference query (desired criteria)

Compute match handle via computeMatchHandle()

Make public the result using makeMatchPublic()

Publicly decrypt → get final result (MATCH ✅ / NO MATCH ❌)

🧩 FHEVM Highlights

Encrypted types: euint8, euint16

Homomorphic operations: FHE.eq, FHE.and, FHE.or, FHE.gt, FHE.lt

Secure access control using FHE.allow & FHE.allowThis

Public decryption enabled with FHE.makePubliclyDecryptable

Frontend encryption/decryption handled via Relayer SDK proofs

📚 Documentation

Zama FHEVM Overview

Relayer SDK Guide

Solidity Library: FHE.sol

Ethers.js v6 Documentation

🆘 Support

🐛 GitHub Issues: Report bugs or feature requests

💬 Zama Discord: discord.gg/zama-ai
 — community help

📄 License

BSD-3-Clause-Clear License
See the LICENSE
 file for full details.