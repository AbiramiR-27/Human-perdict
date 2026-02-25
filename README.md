# Human Predict

Human Predict is a prediction-market workspace with three main parts:

- A React frontend (`Frontend/`)
- Solidity smart contracts (`cre-workflow/hardhat-contracts/`)
- A Chainlink CRE workflow module (`cre-workflow/prediction-market-demo/`)

This README is the main entry point for running and understanding the full project.

## Workspace Layout

```text
Human perdict/
├─ Frontend/                         # Vite + React UI
├─ cre-workflow/
│  ├─ hardhat-contracts/             # Solidity contract + deploy script
│  └─ prediction-market-demo/        # CRE workflow trigger/handler
└─ package.json                      # Root-level lightweight deps
```

## Architecture (High Level)

1. Users interact with the UI in `Frontend/`.
2. The contract emits `SettlementRequested(marketId)` for expired markets.
3. CRE workflow (`prediction-market-demo`) listens for that event on Sepolia.
4. Workflow fetches external data and calls `onReport(...)` on the contract.
5. Market resolves and frontend can display updated outcome.

## Prerequisites

- Node.js 18+
- npm 9+
- Wallet + Sepolia ETH (for contract deployment/transactions)

Optional but useful:

- Hardhat CLI tooling (installed via local project deps)
- Chainlink CRE CLI/runtime (if you plan to run workflow simulation)

## Quick Start

### 1) Frontend

```bash
cd Frontend
npm install
npm run dev
```

Main docs: `Frontend/README.md`

### 2) Smart Contracts (Hardhat)

```bash
cd cre-workflow/hardhat-contracts
npm install
npx hardhat compile
npx hardhat test
```

Deploy script exists at `scripts/deploy.ts`.

### 3) CRE Workflow Module

```bash
cd cre-workflow/prediction-market-demo
npm install
```

Workflow definition is in `workflow.yaml` and source handler is `index.ts`.

## Important Files

- Frontend app routes: `Frontend/src/App.tsx`
- Demo data: `Frontend/src/lib/data.ts`
- Frontend verification context: `Frontend/src/lib/verification.tsx`
- Contract: `cre-workflow/hardhat-contracts/contracts/PredictionMarket.sol`
- Hardhat config: `cre-workflow/hardhat-contracts/hardhat.config.ts`
- Deploy script: `cre-workflow/hardhat-contracts/scripts/deploy.ts`
- CRE workflow trigger config: `cre-workflow/prediction-market-demo/workflow.yaml`
- CRE workflow logic: `cre-workflow/prediction-market-demo/index.ts`

## Environment & Configuration Notes

- `cre-workflow/project.yaml` currently points to Sepolia RPC.
- Deployment script uses `FORWARDER_ADDRESS` from environment with a Sepolia default fallback.
- Contract/workflow addresses should be aligned between deployment output and CRE workflow config.

## Current Status Notes

- Frontend currently uses mock/in-memory data patterns for parts of the UX.
- The contract and CRE code are present, but full end-to-end integration requires wiring addresses, environment variables, and workflow runtime setup.

## Recommended Development Order

1. Run frontend and verify UI flows.
2. Compile and test contracts.
3. Deploy contract to Sepolia and save deployed address.
4. Configure CRE workflow with deployed contract details.
5. Validate end-to-end settlement flow.

## Troubleshooting

- If frontend fails to start, remove `node_modules` and reinstall in `Frontend/`.
- If Hardhat commands fail, verify you are in `cre-workflow/hardhat-contracts/`.
- If settlement does not trigger, confirm workflow event signature/address and Sepolia chain settings.

## License

Add your preferred license file for distribution (`LICENSE`).
