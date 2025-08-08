# IRL.energy × Stellar — Technical Architecture

_Last updated: Aug 8, 2025_

## Overview
IRL.energy is a cultural rewards platform enabling event attendees, creators, and venues to earn and redeem points via blockchain-based payouts. The system integrates **Stellar USDC**, future **$IRL** tokens, and **NFTs** to deliver instant, low-cost rewards while providing a scalable partner dashboard for global adoption.

---

## High-Level Architecture (Mermaid)

```mermaid
flowchart LR
    A[User (Web/Mobile Web)] -->|Auth (Privy Passkey)| B[Frontend (Next.js/React)]
    B --> C[Events API]
    B --> D[Points API]
    B --> E[Rewards API]
    B --> F[Partner Dashboard]
    B --> G[Global Venue Map]
    C --> H[(PostgreSQL)]
    D --> H
    E --> H
    F --> H
    subgraph Backend (Node/Express)
      C
      D
      E
      F
    end

    subgraph Stellar Integration
      I[Stellar SDK / Horizon]
      J[USDC on Stellar]
      K[$IRL (future) bridged]
      L[Soroban Contracts (future)]
    end

    E -- Redeem --> I
    I --> J
    I --> K
    B -- Wallet Connect --> I
    L -. quests/rewards logic .- E

    subgraph Infra
      M[(Redis Cache)]
      N[Monitoring (Datadog)]
      O[Cloudflare Edge]
    end

    B --> O
    Backend --> N
    Backend --> M
```

---

## Core Components

### Frontend (Web & Mobile Web)
- **Framework**: Next.js + React
- **Authentication**: Privy (passkey auth, social/email login)
- **UI Modules**:
  - Event check-in (QR/NFC/GPS)
  - User dashboard (points, rewards, history)
  - Reward Center (perks, redemptions, confirmations)
  - Global venue map (live check-ins & active perks)
  - Leaderboard & achievements
  - Partner dashboard (events, issuance, analytics, payouts)

### Backend & API Layer
- **Stack**: Node.js + Express
- **Database**: PostgreSQL (events, users, points)
- **Cache**: Redis (leaderboards, live updates)
- **APIs**:
  - Events API — create/update events, manage check-ins, attendance
  - Points API — accrue/deduct IRL Points, link to rewards
  - Rewards API — manage perks, trigger Stellar payouts
  - Partner API — B2B integrations (ticketing, venue software)
- **Monitoring**: Datadog + Horizon logs

### Blockchain Integration (Stellar)
- **Wallet Creation**: Abstracted wallet generation via Stellar SDK for non-crypto-native users
- **Assets**:
  - **USDC on Stellar** for payouts/redemptions
  - **$IRL (future)** bridged to Stellar for payouts
  - **NFTs** via Soroban (future) for cultural identity & collectibles
- **On/Off Ramps**: Coinbase Pay / Ramp Network
- **Transactions**:
  - Check-in → points issuance
  - Points redemption → Stellar payout (USDC / $IRL)
  - Partner payouts → USDC or bridged $IRL

### Gamification Layer
- **Points System**: Tiered loyalty (base/mid/top + Patron Pass)
- **Quests**: Time- & location-based; Soroban onchain validation (future)
- **Leaderboards**: Live via WebSockets
- **NFT-based Unlocks**: Tickets, access, perks (future)

---

## Payment Flow with Stellar
1. User checks in at event → **IRL Points** added to profile
2. User redeems points in the **Reward Center**
3. Backend triggers **Stellar transaction** → USDC (or $IRL) payout to user wallet
4. **Partner payouts** to venues/creators processed on Stellar (bulk or instant)

---

## Deployment & Infrastructure
- **Hosting**: Vercel (frontend), AWS (backend services), Cloudflare (edge)
- **DB Hosting**: Supabase (Postgres)
- **Nodes**: Hosted **Stellar Horizon** instance (prod) + Testnet node (staging)
- **Observability**: Datadog + structured logs

---

## Roadmap Alignment
- **Phase 1 – MVP (Aug–Oct 2025)**: Event check-ins, points, USDC redemptions, partner dashboard
- **Phase 2 – Testnet (Oct–Nov 2025)**: Wallet creation, NFT perks, cultural identity tokens, real-time payouts
- **Phase 3 – Mainnet (Nov–Dec 2025)**: Cross-chain bridging, global venue map at scale, Soroban gamification

---

## Public Link Guidance
Host this README in a public GitHub repo (recommended):
- Repo name: `irl-stellar-architecture`
- File: `README.md` (this document)

GitHub automatically renders Mermaid diagrams, so reviewers can see the architecture visually.

