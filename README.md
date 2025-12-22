# **Project Title: Truly Random**

**Concept:** A High-Assurance, Decentralized Randomness Oracle & Verifiable Lottery Protocol.

## 1. **Core Mission**
To provide a **Zero-Cost, Provably Fair** alternative to centralized or expensive randomness services (like Chainlink VRF) by aggregating independent, cryptographically secure public beacons and immutable blockchain ledgers.

## 2. **Technical Architecture (The "Fortress of Truth")**
The system operates on a **Commit-Reveal Time-Lock** mechanism to prevent front-running and manipulation.

### A. The Aggregator Suite (The "Big 5")
To ensure unbiasable entropy, the system pulls data from five distinct geopolitical and technical sources:

1. **Drand (League of Entropy)**: Decentralized threshold cryptography. (Target: Round scheduled at Time $T$).
2. **NIST Randomness Beacon**: U.S. Government-backed hardware entropy. (Target: First pulse after $T$).
3. **Random UChile**: Academic/University-backed beacon. (Target: First pulse after $T$).
4. **Bitcoin Blockchain**: Proof-of-Work immutability. (Target: First block hash mined after $T$).
5. **Ethereum Beacon Chain**: Proof-of-Stake consensus randomness. (Target: First block finalized after $T$).

### B. Quorum & Resilience (Go/No-Go Logic)
The generation proceeds _**IF AND ONLY IF**_:

- **Beacons**: At least **2 of 3** (Drand, NIST, UChile) are online and verified.
- **Blockchains**: At least **1 of 2** (BTC, ETH) is online and verified.
- **Failure Protocol**: If Quorum is not met within 10 minutes, the system issues a **Signed Attestation of Failure** and triggers a refund/abort.

### C. The Algorithm (SHA3-256 Sponge)

- **Normalization**: All inputs are stripped of prefixes (like 0x), converted to lowercase Hex (Base 16).
- **Canonicalization Defense**: Inputs are absorbed into a **SHA3-256 (Keccak)** sponge separated by a specific delimiter (e.g., |) to prevent concatenation attacks.
- **Final Output**: A standard 32-byte (64-character hex) hash.

### D. Mathematical Mapping (Modulo Bias Defense)
To map a 256-bit hash to a lottery winner without favoring lower numbers:

**Method**: Large Integer Modulo.
**Logic**: Take the first **128 bits (16 bytes)** of the SHA-3 output.
**Calculation**: Cast to a `uint128_t` (Huge Integer) and perform: $Winner = Huge\_Int \pmod{Total\_Tickets}$.
**Note**: For extremely high-stakes, Rejection Sampling remains a secondary consideration.

## 3. Business & Market Strategy
### A. The Minimum Viable Product (MVP)

1. **The Backend**: A C++ Daemon that monitors the "Big 5" APIs and logs results to a public JSON file.
2. **The Certificate**: A "Commit Certificate" issued to users at the time of entry, locking them to a future timestamp $T$.
3. **The Verifier**: A client-side JavaScript tool allowing users to paste raw API data and verify the SHA-3 math independently.

### B. Selling Points (The "Chainlink Killer")

- **Cost**: 100% free for users to verify; no "Oracle Fees" (LINK) required.
- **Trust**: No single entity (including the developer) can predict or rig the result.
- **Transparency**: Public Query Logs and Signed Attestations of Failure ensure no "cherry-picking" of sources.

### C. Target Markets
- **International**: Web3 Indie Games (Loot drops/crit hits), NFT Minting (rarity distribution), and DAO Governance (random audits).
- **Philippines (Local)**: B2B "Trust API" for licensed gaming operators (POGOs/e-Bingo) to prove fairness to skeptical players.

### D. Monetization (Freemium Model)
- **Free Tier**: Public access to the randomness data for manual verification.
- **Paid Tier**: Includes a "Verified by Nexus" UI badge, higher API rate limits for high-frequency games, and white-label customization for enterprise clients.

## 4. Key Use-Case Parameters
- **Tolerance**: The "No More Bets" window (e.g., 5-10 minutes before $T$) to ensure the future hash cannot be predicted.
- **Timestamp Anchor**: All draw targets are based on UTC time, not block index, to ensure cross-chain synchronization.

## 5. Others
- [ ] Possible smart contract (layer 2) deployment.
