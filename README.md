# Khutruk

Khutruk is an open-source Web3 crowdfunding platform focused on social and disaster-relief fundraising.  
It combines:

- **Next.js 14** frontend and API routes
- **Prisma + MongoDB** for off-chain app data
- **Solidity + Hardhat** smart contracts for on-chain campaign and donation flows
- **MetaMask + ethers.js** wallet and contract interactions
- **Pinata/IPFS** media upload for campaign images

## Features

- User signup/login with JWT authentication
- Wallet linking (MetaMask address mapped to authenticated user)
- Multi-step campaign creation
  - Campaign metadata (category, title, description)
  - Campaign image upload to IPFS
  - On-chain campaign creation transaction
  - Off-chain campaign persistence in MongoDB
- Campaign discovery dashboard and campaign detail pages
- On-chain donation flow
- On-chain withdrawal request and release flow for campaign creators

## Tech Stack

- **Frontend:** Next.js App Router, React, TypeScript, Tailwind CSS, shadcn/ui
- **Web3:** ethers.js, MetaMask SDK, Hardhat, Hardhat Ignition
- **Backend/Data:** Next.js API routes, Prisma Client, MongoDB
- **Storage:** Pinata IPFS gateway

## Project Structure

```text
contracts/                 Solidity contracts
ignition/modules/          Hardhat Ignition deployment modules
prisma/schema.prisma       Database schema (MongoDB)
src/app/                   App Router pages + API routes
src/domain/repositories/   Frontend data access layer for API endpoints
src/services/campaign/     Smart contract interaction services
src/AddressABI/            Contract ABI + configured deployed address
test/                      Hardhat tests (template Lock contract)
```

## API Routes (Current)

| Route | Method | Purpose |
|---|---|---|
| `/api/create-user` | POST | Register user and return JWT |
| `/api/user-login` | POST | Authenticate user and return JWT |
| `/api/retrieve-users` | GET | Get current user from JWT |
| `/api/connect-metamask` | POST | Attach wallet address to authenticated user |
| `/api/create-campaign` | POST | Persist campaign metadata |
| `/api/retrieve-campaign` | GET | List campaigns |
| `/api/retrieve-campaign/[sequenceId]` | GET | Get campaign by sequence id |
| `/api/donation` | POST | Donation persistence endpoint scaffold (not fully implemented) |

## Smart Contract Overview

Main contract: `contracts/DisasterRelifCampaign.sol`

Core on-chain actions:

- `createCampaign(title, description, targetAmount)`
- `donate(campaignId)` (payable)
- `requestWithdrawal(campaignId, amount, reason)`
- `releaseWithdrawal(campaignId, requestIndex)`
- `getCampaign(campaignId)`

Key rules:

- Max withdrawal request per release window: **30%** of available funds
- Delay between releases: **3 days**
- Additional delay before release approval: **24 hours**

## Prerequisites

- Node.js 18+ (recommended)
- npm
- MongoDB connection string
- MetaMask browser extension (for wallet interactions)
- (Optional) Sepolia ETH for deployment/testing on Sepolia

## Environment Variables

Create a `.env` file in the repository root:

```env
DATABASE_URL=
NEXT_PUBLIC_BASE_API_URL=http://localhost:3000/api/
NEXT_PUBLIC_JWT_SECRET=
NEXT_PUBLIC_IPFS_JWT=
NEXT_PUBLIC_IPFS_GATEWAY=
PRIVATE_KEY=
```

Notes:

- `NEXT_PUBLIC_BASE_API_URL` is used by frontend repository calls.
- `PRIVATE_KEY` is used by Hardhat deploy script for Sepolia.
- IPFS variables are used by Pinata uploads.

## Local Development

1. Install dependencies:

   ```bash
   npm install
   ```

2. Generate Prisma client:

   ```bash
   npx prisma generate
   ```

3. Start development server:

   ```bash
   npm run dev
   ```

4. Open `http://localhost:3000`.

## Smart Contract Workflow

- Compile contracts:

  ```bash
  npx hardhat compile
  ```

- Run tests:

  ```bash
  npx hardhat test
  ```

- Deploy with Ignition (configured script):

  ```bash
  npm run deploy
  ```

## Quality Checks

- Lint frontend:

  ```bash
  npm run lint
  ```

## Open Source

This project is open source under **The Unlicense**.  
See [`LICENSE`](./LICENSE) for details.

## Contributing

Contributions are welcome:

1. Fork the repository
2. Create a feature branch
3. Make focused changes with clear commit messages
4. Open a pull request

Please follow the [Code of Conduct](./CODE_OF_CONDUCT.md).
