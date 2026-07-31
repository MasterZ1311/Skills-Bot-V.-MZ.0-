# ⛓️ Web3 / Blockchain Developer — Skills Guide

Web3 developers build decentralized applications, smart contracts, token systems, DeFi protocols, and NFT platforms on blockchain networks.

---

## 🗺️ Skill Map

| Concern | Top Skills |
|---|---|
| Smart Contracts | `@blockchain-developer`, `@solidity-security` |
| DeFi | `@defi-protocol-templates` |
| NFTs | `@nft-standards` |
| Testing | `@web3-testing` |
| Bitcoin / Lightning | `@lightning-architecture-review`, `@lightning-channel-factories` |
| Wallets | `@emblemai-crypto-wallet` |
| Business Dev | `@crypto-bd-agent` |

---

## 📜 Smart Contracts

### `@blockchain-developer`
Solidity, EVM, contract architecture, deployment.
```
@blockchain-developer Design a blockchain ticketing system for our events platform:
Concept: event tickets as NFTs (ERC-721)

Smart contract: EventTicket.sol
- mint(address to, uint256 eventId, uint256 seatNumber): organizer mints tickets
- transferFrom: disabled (soulbound — non-transferable tickets)
- validate(uint256 tokenId): marks ticket as used (for venue check-in)
- burn(uint256 tokenId): for cancellation + refund
- getTicketInfo(tokenId): returns eventId, owner, used status

Factory contract: EventFactory.sol
- createEvent(name, capacity, price): deploys new EventTicket contract
- Maps eventId → contract address

Deployment: Polygon (low gas) or Base (L2 Ethereum)
Frontend: wagmi + viem for wallet integration
```

### `@solidity-security`
Security audit of Solidity smart contracts.
```
@solidity-security Audit our EventTicket.sol contract:
[paste contract code]

Check for:
- Reentrancy: are external calls made before state changes?
- Access control: only organizer can mint, only contract can validate?
- Integer overflow: using SafeMath or Solidity 0.8+ checked arithmetic?
- Front-running: can miners exploit ticket minting order?
- Denial of service: can a bad actor block all ticket operations?
- Centralization risks: owner has too much power?
- Gas optimization: any loops that could hit gas limit?

Output: findings with severity (Critical/High/Medium/Low) and remediation.
Reference: SWC Registry, Consensys best practices.
```

---

## 💰 DeFi

### `@defi-protocol-templates`
DeFi patterns: liquidity pools, staking, yield, AMMs.
```
@defi-protocol-templates Design a staking mechanism for our events token (EVNT):
- Token holders stake EVNT → earn platform fee share
- Staking period: 30/60/90 day lock-up tiers
- Rewards: 50% of platform fees distributed to stakers
- Governance: staked tokens → voting rights on platform decisions

Smart contracts:
- EVNTStaking.sol: stake, unstake, claim rewards
- RewardDistributor.sol: collect fees, distribute proportionally
- Governance.sol: proposal, vote, execute

Security: flash loan attack prevention (snapshot voting power at proposal time)
```

---

## 🎨 NFTs

### `@nft-standards`
ERC-721, ERC-1155, metadata standards, royalties.
```
@nft-standards Design the NFT system for event commemorative collectibles:
After attending an event → organizer issues a collectible NFT to attendees

Standard: ERC-1155 (multiple editions of same event)
Metadata (off-chain, IPFS):
{
  name: "[Event Name] Attendee",
  description: "Commemorating attendance at [Event] on [Date]",
  image: "[Event photo on IPFS]",
  attributes: [
    { trait_type: "Event", value: "Annual Hackathon 2025" },
    { trait_type: "College", value: "VIT Vellore" },
    { trait_type: "Category", value: "Hackathon" },
    { trait_type: "Edition", value: "1 of 500" }
  ]
}

Royalties: ERC-2981 (5% royalty on secondary sales)
Soulbound option: make non-transferable for attendance proof
IPFS: pin metadata and images with Pinata or web3.storage
```

---

## 🧪 Testing

### `@web3-testing`
Smart contract testing with Hardhat/Foundry.
```
@web3-testing Write tests for our EventTicket smart contract:
Framework: Foundry (forge test) or Hardhat (ethers.js)

Test cases:
1. mint: organizer mints ticket → correct owner, eventId, seatNumber
2. mint: non-organizer tries to mint → reverts with AccessDenied
3. transferFrom: any address tries to transfer → reverts (soulbound)
4. validate: organizer validates ticket → used=true, emits TicketUsed event
5. validate: validate already-used ticket → reverts with AlreadyUsed
6. burn: organizer burns for refund → token destroyed, OwnerOf reverts
7. getTicketInfo: returns correct data for all ticket states

Coverage target: 100% line and branch coverage on all contract functions.
Gas report: show gas cost for each function.
```

---

## ⚡ Bitcoin / Lightning Network

### `@lightning-architecture-review`
Lightning Network channel and routing architecture.
```
@lightning-architecture-review Review our Lightning payment integration for events:
We want to accept Bitcoin payments for event tickets via Lightning.

Evaluate:
- Node setup: managed (Voltage, Alby) vs self-hosted (LND, CLN)?
- Liquidity: how to ensure inbound liquidity for payments?
- Invoice generation: BOLT11 vs BOLT12 (reusable)?
- Payment confirmation: how to confirm before granting registration?
- Timeout handling: what if user doesn't pay within 15 minutes?
- Refunds: Lightning doesn't support native refunds — strategy?
- Backend: lnbits or LND API for payment generation and monitoring

Recommend: simplest reliable path for accepting Lightning payments.
```

---

## 🔗 Complete Web3 Prompt Chain

```
1️⃣  @blockchain-developer
    "Design EventTicket ERC-721 (soulbound) + EventFactory contracts"

2️⃣  @solidity-security
    "Security audit: reentrancy, access control, overflow, front-running"

3️⃣  @web3-testing
    "Write Foundry tests: 100% coverage, gas report"

4️⃣  @nft-standards
    "Design ERC-1155 commemorative NFT with IPFS metadata"

5️⃣  @defi-protocol-templates
    "Add EVNT token staking for governance and revenue share"

6️⃣  @lightning-architecture-review
    "Integrate Lightning payment option for ticket purchase"
```
