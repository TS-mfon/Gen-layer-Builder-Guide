# The Ultimate Guide to Building on GenLayer with the Project Boilerplate

> **Everything you need to know to build, deploy, and ship dApps on GenLayer — derived entirely from the official boilerplate repository.**

---

## Table of Contents

1. [What is GenLayer?](#1-what-is-genlayer)
2. [What is the Boilerplate?](#2-what-is-the-boilerplate)
3. [Repository Structure — Full Map](#3-repository-structure--full-map)
4. [Prerequisites & Requirements](#4-prerequisites--requirements)
5. [Getting Started — Step by Step](#5-getting-started--step-by-step)
6. [Understanding Intelligent Contracts (Python)](#6-understanding-intelligent-contracts-python)
   - 6.1 [Contract Anatomy](#61-contract-anatomy)
   - 6.2 [Storage Types](#62-storage-types)
   - 6.3 [Method Decorators](#63-method-decorators)
   - 6.4 [Web Access (AI + Internet)](#64-web-access-ai--internet)
   - 6.5 [LLM Access](#65-llm-access)
   - 6.6 [Equivalence Principle (Validation)](#66-equivalence-principle-validation)
   - 6.7 [Complete Football Bets Contract — Annotated](#67-complete-football-bets-contract--annotated)
7. [Deployment System](#7-deployment-system)
   - 7.1 [Deploy Script Anatomy](#71-deploy-script-anatomy)
   - 7.2 [Network Selection](#72-network-selection)
   - 7.3 [Deployment Flow](#73-deployment-flow)
8. [Testing Contracts](#8-testing-contracts)
   - 8.1 [Test Framework (gltest)](#81-test-framework-gltest)
   - 8.2 [Writing Tests](#82-writing-tests)
   - 8.3 [Test Fixtures & Schemas](#83-test-fixtures--schemas)
   - 8.4 [Running Tests](#84-running-tests)
9. [Frontend Architecture (Next.js)](#9-frontend-architecture-nextjs)
   - 9.1 [Tech Stack Overview](#91-tech-stack-overview)
   - 9.2 [App Structure](#92-app-structure)
   - 9.3 [Environment Configuration](#93-environment-configuration)
   - 9.4 [GenLayer Client Setup](#94-genlayer-client-setup)
   - 9.5 [Wallet Management (MetaMask)](#95-wallet-management-metamask)
   - 9.6 [Contract Interaction Class](#96-contract-interaction-class)
   - 9.7 [React Hooks for Contract Data](#97-react-hooks-for-contract-data)
   - 9.8 [UI Components](#98-ui-components)
   - 9.9 [Styling & Design System](#99-styling--design-system)
   - 9.10 [Toast Notifications](#910-toast-notifications)
10. [How to Build Your Own dApp](#10-how-to-build-your-own-dapp)
    - 10.1 [Step 1: Write Your Contract](#101-step-1-write-your-contract)
    - 10.2 [Step 2: Write Your Deploy Script](#102-step-2-write-your-deploy-script)
    - 10.3 [Step 3: Write Your Tests](#103-step-3-write-your-tests)
    - 10.4 [Step 4: Build the Frontend Contract Class](#104-step-4-build-the-frontend-contract-class)
    - 10.5 [Step 5: Create React Hooks](#105-step-5-create-react-hooks)
    - 10.6 [Step 6: Build UI Components](#106-step-6-build-ui-components)
    - 10.7 [Step 7: Deploy & Connect](#107-step-7-deploy--connect)
11. [Key Patterns & Conventions](#11-key-patterns--conventions)
12. [Complete File Reference](#12-complete-file-reference)
13. [Quick Command Reference](#13-quick-command-reference)

---

## 1. What is GenLayer?

GenLayer is an **AI-native blockchain** where smart contracts can:

- **Natively access the internet** — fetch web pages, call APIs
- **Make decisions using AI (LLMs)** — interpret data, classify results, make judgments
- **Validate non-deterministic outputs** — using an "Equivalence Principle" consensus mechanism

Contracts are written in **Python** and executed in the **GenVM** (GenLayer Virtual Machine). On the frontend, you interact with contracts using the **genlayer-js** SDK.

---

## 2. What is the Boilerplate?

The boilerplate is a **production-ready starter template** that includes:

| Layer | What's Included |
|-------|----------------|
| **Contract** | A Python intelligent contract (`football_bets.py`) |
| **Deployment** | TypeScript deploy script using `genlayer-js` SDK |
| **Testing** | Python integration tests using `gltest` framework |
| **Frontend** | Full Next.js 15 app with wallet integration, data fetching, and UI |
| **Configuration** | Environment files, TypeScript configs, Tailwind CSS |

The example contract is a **Football Betting Game** where users predict match outcomes, and GenLayer's AI verifies results by scraping real match data from the web.

---

## 3. Repository Structure — Full Map

```
genlayer-project-boilerplate/
├── package.json                    # Root workspace config (npm workspaces)
├── tsconfig.json                   # Root TypeScript config
├── requirements.txt                # Python dependencies for testing
├── LICENSE                         # MIT License
├── README.md                       # Project documentation
├── CLAUDE.md                       # AI assistant guidance
├── .gitignore                      # Git ignore rules
├── __init__.py                     # Python package marker (root)
│
├── contracts/                      # INTELLIGENT CONTRACTS (Python)
│   ├── __init__.py
│   └── football_bets.py           # The main contract
│
├── deploy/                         # DEPLOYMENT SCRIPTS (TypeScript)
│   └── deployScript.ts            # Contract deployment logic
│
├── test/                           # INTEGRATION TESTS (Python)
│   ├── __init__.py
│   ├── test_footbal_bet.py        # Test cases
│   └── football_bets_get_contract_schema_for_code.py  # Test fixtures/schemas
│
├── config/                         # CONFIGURATION (Python)
│   ├── __init__.py
│   └── genlayer_config.py         # RPC config loader
│
└── frontend/                       # NEXT.JS FRONTEND
    ├── package.json                # Frontend dependencies
    ├── .env.example                # Environment template
    ├── next.config.ts              # Next.js configuration
    ├── tsconfig.json               # Frontend TypeScript config
    ├── tailwind.config.ts          # Tailwind CSS config
    ├── postcss.config.mjs          # PostCSS config
    ├── components.json             # shadcn/ui config
    ├── README.md                   # Frontend documentation
    │
    ├── app/                        # Next.js App Router
    │   ├── layout.tsx              # Root layout (fonts, metadata, Providers)
    │   ├── page.tsx                # Home page
    │   ├── providers.tsx           # QueryClient + WalletProvider + Toaster
    │   └── globals.css             # Global styles (brand colors, animations)
    │
    ├── components/                 # React Components
    │   ├── Navbar.tsx              # Navigation bar with stats
    │   ├── BetsTable.tsx           # Bets display table
    │   ├── Leaderboard.tsx         # Points leaderboard
    │   ├── CreateBetModal.tsx      # Bet creation form dialog
    │   ├── AccountPanel.tsx        # Wallet connect/disconnect/switch
    │   ├── AddressDisplay.tsx      # Address shortener with copy
    │   ├── Logo.tsx                # GenLayer brand logo (SVG)
    │   └── ui/                     # shadcn/ui primitives
    │       ├── alert.tsx
    │       ├── badge.tsx
    │       ├── button.tsx
    │       ├── dialog.tsx
    │       ├── input.tsx
    │       └── label.tsx
    │
    ├── lib/                        # Library / Utilities
    │   ├── utils.ts                # cn() helper (clsx + tailwind-merge)
    │   ├── utils/
    │   │   └── toast.ts            # Toast notification helpers
    │   ├── contracts/
    │   │   ├── FootballBets.ts     # Contract interaction class
    │   │   └── types.ts            # TypeScript type definitions
    │   ├── hooks/
    │   │   └── useFootballBets.ts  # React Query hooks for contract
    │   └── genlayer/
    │       ├── client.ts           # GenLayer client + MetaMask helpers
    │       ├── wallet.ts           # Re-exports from WalletProvider
    │       └── WalletProvider.tsx  # React context for wallet state
    │
    └── public/                     # Static assets
        ├── favicon.svg
        ├── site.webmanifest
        └── fonts/
            ├── Switzer-Regular.woff2
            ├── Switzer-Bold.woff2
            └── Switzer-Light.woff2
```

---

## 4. Prerequisites & Requirements

### Required Software

| Tool | Purpose | Install |
|------|---------|---------|
| **Node.js** (18+) | Frontend & deployment | nodejs.org |
| **npm** or **bun** | Package management | Bundled with Node / bun.sh |
| **Python** (3.10+) | Contracts & testing | python.org |
| **GenLayer CLI** | Deploy & network management | `npm install -g genlayer` |
| **GenLayer Studio** | Local dev environment | [Docs](https://docs.genlayer.com/developers/intelligent-contracts/tooling-setup) or [Hosted](https://studio.genlayer.com/) |
| **MetaMask** | Wallet for frontend | metamask.io |

### Python Dependencies (`requirements.txt`)

```
requests==2.31.0
python-dotenv==1.0.1
eth-account==0.13.3
eth-utils==5.0.0
genlayer-test==0.1.1
```

### Key npm Dependencies

**Root:**
- `genlayer-js` ^0.18.3

**Frontend:**
- `next` ^16.0.0
- `react` ^19.2.0
- `genlayer-js` ^0.18.3
- `@tanstack/react-query` ^5.90.5
- `wagmi` ^2.19.2
- `viem` 2.21.54
- `@radix-ui/react-dialog`, `@radix-ui/react-label`, `@radix-ui/react-slot`
- `tailwindcss` ^4.1.16
- `sonner` ^1.7.1
- `lucide-react` ^0.548.0

---

## 5. Getting Started — Step by Step

### Step 1: Clone the Repository

```bash
git clone https://github.com/genlayerlabs/genlayer-project-boilerplate.git
cd genlayer-project-boilerplate
```

### Step 2: Install Dependencies

```bash
# Root dependencies (genlayer-js for deployment)
npm install

# Frontend dependencies
cd frontend
npm install    # or: bun install
cd ..

# Python dependencies (for testing)
pip install -r requirements.txt   # ideally in a virtualenv
```

### Step 3: Start GenLayer Studio

Either:
- **Locally**: Follow the [GenLayer Studio setup docs](https://docs.genlayer.com/developers/intelligent-contracts/tooling-setup)
- **Hosted**: Use [studio.genlayer.com](https://studio.genlayer.com/)

### Step 4: Select a Network

```bash
genlayer network
```

Choose from:
- `studionet` — Hosted GenLayer Studio
- `localnet` — Local GenLayer instance
- `testnet-*` — Public test networks

### Step 5: Deploy the Contract

```bash
npm run deploy
# or directly: genlayer deploy
```

This executes `/deploy/deployScript.ts`, which:
1. Reads the contract file from `/contracts/football_bets.py`
2. Initializes the consensus smart contract
3. Deploys the contract
4. Waits for the transaction to be ACCEPTED or FINALIZED
5. Prints the deployed contract address

**Copy the printed contract address** — you'll need it for the frontend.

### Step 6: Configure the Frontend

```bash
cd frontend
cp .env.example .env
```

Edit `.env`:
```env
NEXT_PUBLIC_GENLAYER_RPC_URL=https://studio.genlayer.com/api
NEXT_PUBLIC_GENLAYER_CHAIN_ID=61999
NEXT_PUBLIC_GENLAYER_CHAIN_NAME=GenLayer Studio
NEXT_PUBLIC_GENLAYER_SYMBOL=GEN
NEXT_PUBLIC_CONTRACT_ADDRESS=<paste_your_deployed_address_here>
```

### Step 7: Run the Frontend

```bash
npm run dev    # or: bun dev
```

Open [http://localhost:3000](http://localhost:3000).

### Step 8: Run Tests (Optional)

```bash
gltest
```

Requires GenLayer Studio to be running.

---

## 6. Understanding Intelligent Contracts (Python)

### 6.1 Contract Anatomy

Every GenLayer contract follows this structure:

```python
# Dependency header (required)
# { "Depends": "py-genlayer:test" }

from genlayer import *

class MyContract(gl.Contract):
    # Storage declarations (typed state variables)
    my_data: TreeMap[Address, str]

    def __init__(self):
        # Constructor — called once at deployment
        pass

    @gl.public.view
    def read_method(self) -> str:
        # Read-only — does not modify state
        return "hello"

    @gl.public.write
    def write_method(self, value: str) -> None:
        # State-modifying — requires a transaction
        self.my_data[gl.message.sender_address] = value
```

Key points:
- Contracts **extend `gl.Contract`**
- State is declared as **class-level type annotations**
- The `__init__` method is the **constructor** (called at deployment)
- `gl.message.sender_address` gives you the caller's address (like `msg.sender` in Solidity)

### 6.2 Storage Types

| Type | Description | Example |
|------|-------------|---------|
| `TreeMap[K, V]` | Key-value map (like Solidity mapping) | `TreeMap[Address, u256]` |
| `DynArray[T]` | Dynamic-length array | `DynArray[str]` |
| `Array[T]` | Fixed-length array | `Array[str]` |
| `u256` | Unsigned 256-bit integer | For points, balances |
| `Address` | Blockchain address type | For user addresses |
| `@allow_storage` | Decorator to make custom classes storable | For dataclasses |

**Custom storable types** use `@allow_storage` with `@dataclass`:

```python
from dataclasses import dataclass

@allow_storage
@dataclass
class Bet:
    id: str
    has_resolved: bool
    game_date: str
    team1: str
    team2: str
    predicted_winner: str
    real_winner: str
    real_score: str
```

**Nested storage** is supported — e.g., `TreeMap[Address, TreeMap[str, Bet]]` (a map of maps).

### 6.3 Method Decorators

| Decorator | Purpose | Gas Cost | Can Modify State? |
|-----------|---------|----------|-------------------|
| `@gl.public.view` | Read-only queries | Free | No |
| `@gl.public.write` | State mutations | Costs gas | Yes |
| `@gl.public.write.payable` | Accept value (tokens) | Costs gas | Yes |

Methods **without decorators** are private/internal (like `_check_match` in the example).

### 6.4 Web Access (AI + Internet)

GenLayer contracts can **fetch web content** natively:

```python
# Render a web page as text
web_data = gl.nondet.web.render(url, mode="text")

# Render a web page as HTML
web_html = gl.nondet.web.render(url, mode="html")

# Render a web page as screenshot (returns Image)
screenshot = gl.nondet.web.render(url, mode="screenshot")

# Simple HTTP GET
response = gl.nondet.web.get(url, headers={})

# HTTP POST
response = gl.nondet.web.post(url, body="...", headers={})
```

In the Football Bets contract, this is used to fetch match results from BBC Sport:

```python
web_data = gl.nondet.web.render(resolution_url, mode="text")
```

### 6.5 LLM Access

Contracts can **call an LLM** to process data:

```python
# Get a text response
result = gl.nondet.exec_prompt("Analyze this data: ...")

# Get a structured JSON response
result = gl.nondet.exec_prompt(
    "Extract the score...",
    response_format="json"
)
# result is already a dict

# With images
result = gl.nondet.exec_prompt(
    "What's in this image?",
    images=[image_bytes]
)
```

In the Football Bets contract, the LLM extracts match scores from web content:

```python
task = f"""
Extract the match result for:
Team 1: {team1}
Team 2: {team2}

Web content:
{web_data}

Respond in JSON:
{{
    "score": str,    // e.g., "1:2" or "-" if unresolved
    "winner": int    // 0 for draw, -1 if unresolved
}}
"""
result = gl.nondet.exec_prompt(task, response_format="json")
```

### 6.6 Equivalence Principle (Validation)

Since web content and LLM outputs are **non-deterministic**, GenLayer uses the **Equivalence Principle** to validate that multiple validators reach the same conclusion:

| Type | Function | Use Case |
|------|----------|----------|
| **Strict** | `gl.eq_principle.strict_eq(fn)` | Results must be **exactly identical** |
| **Comparative** | `gl.eq_principle.prompt_comparative(prompt)` | Results should be **similar** (LLM judges) |
| **Non-Comparative** | `gl.eq_principle.prompt_non_comparative(prompt)` | Each result is **independently valid** |

The Football Bets contract uses strict equality:

```python
# The function get_match_result is called by multiple validators
# Their results must match exactly (after JSON serialization with sorted keys)
result_json = json.loads(gl.eq_principle.strict_eq(get_match_result))
```

The pattern is:
1. Define a function that produces non-deterministic output
2. Wrap it with an equivalence principle
3. The system runs it on multiple validators and checks consensus

### 6.7 Complete Football Bets Contract — Annotated

```python
# { "Depends": "py-genlayer:test" }

import json
from dataclasses import dataclass
from genlayer import *


# Custom storable data class for bet records
@allow_storage
@dataclass
class Bet:
    id: str                  # Unique ID: "date_team1_team2" (lowercase)
    has_resolved: bool       # Whether the bet outcome has been determined
    game_date: str           # Date string: "YYYY-MM-DD"
    resolution_url: str      # BBC Sport URL for match results
    team1: str               # First team name
    team2: str               # Second team name
    predicted_winner: str    # "1" (team1), "2" (team2), or "0" (draw)
    real_winner: str         # Actual winner after resolution
    real_score: str          # Actual score (e.g., "1:0")


class FootballBets(gl.Contract):
    # Storage: nested maps — Address → (BetID → Bet)
    bets: TreeMap[Address, TreeMap[str, Bet]]
    # Storage: Address → points count
    points: TreeMap[Address, u256]

    def __init__(self):
        pass  # No initialization needed — TreeMaps auto-initialize

    # PRIVATE: Fetch and parse match result from web
    def _check_match(self, resolution_url: str, team1: str, team2: str) -> dict:
        def get_match_result() -> str:
            # Fetch web page content
            web_data = gl.nondet.web.render(resolution_url, mode="text")

            # Ask LLM to extract match result
            task = f"""
Extract the match result for:
Team 1: {team1}
Team 2: {team2}

Web content:
{web_data}

Respond in JSON:
{{
    "score": str, // e.g., "1:2" or "-" if unresolved
    "winner": int // 0 for draw, -1 if unresolved
}}
"""
            result = gl.nondet.exec_prompt(task, response_format="json")
            return json.dumps(result, sort_keys=True)

        # Validate across validators using strict equality
        result_json = json.loads(gl.eq_principle.strict_eq(get_match_result))
        return result_json

    # PUBLIC WRITE: Create a new bet prediction
    @gl.public.write
    def create_bet(self, game_date: str, team1: str, team2: str,
                   predicted_winner: str) -> None:
        match_resolution_url = (
            "https://www.bbc.com/sport/football/scores-fixtures/" + game_date
        )

        sender_address = gl.message.sender_address

        # Generate deterministic bet ID
        bet_id = f"{game_date}_{team1}_{team2}".lower()

        # Check for duplicate bets
        if sender_address in self.bets and bet_id in self.bets[sender_address]:
            raise Exception("Bet already created")

        # Create and store the bet
        bet = Bet(
            id=bet_id, has_resolved=False, game_date=game_date,
            resolution_url=match_resolution_url, team1=team1, team2=team2,
            predicted_winner=predicted_winner, real_winner="", real_score="",
        )
        self.bets.get_or_insert_default(sender_address)[bet_id] = bet

    # PUBLIC WRITE: Resolve a bet (check actual result)
    @gl.public.write
    def resolve_bet(self, bet_id: str) -> None:
        if self.bets[gl.message.sender_address][bet_id].has_resolved:
            raise Exception("Bet already resolved")

        bet = self.bets[gl.message.sender_address][bet_id]
        bet_status = self._check_match(bet.resolution_url, bet.team1, bet.team2)

        if int(bet_status["winner"]) < 0:
            raise Exception("Game not finished")

        bet.has_resolved = True
        bet.real_winner = str(bet_status["winner"])
        bet.real_score = bet_status["score"]

        # Award point if prediction was correct
        if bet.real_winner == bet.predicted_winner:
            if gl.message.sender_address not in self.points:
                self.points[gl.message.sender_address] = 0
            self.points[gl.message.sender_address] += 1

    # PUBLIC VIEW: Get all bets (all users)
    @gl.public.view
    def get_bets(self) -> dict:
        return {k.as_hex: v for k, v in self.bets.items()}

    # PUBLIC VIEW: Get all points (all users)
    @gl.public.view
    def get_points(self) -> dict:
        return {k.as_hex: v for k, v in self.points.items()}

    # PUBLIC VIEW: Get points for a specific player
    @gl.public.view
    def get_player_points(self, player_address: str) -> int:
        return self.points.get(Address(player_address), 0)
```

---

## 7. Deployment System

### 7.1 Deploy Script Anatomy

The deploy script (`deploy/deployScript.ts`) uses the `genlayer-js` SDK:

```typescript
import { readFileSync } from "fs";
import path from "path";
import {
  TransactionHash,
  TransactionStatus,
  GenLayerClient,
  DecodedDeployData,
  GenLayerChain,
} from "genlayer-js/types";
import { localnet } from "genlayer-js/chains";

export default async function main(client: GenLayerClient<any>) {
  // 1. Read the contract file as bytes
  const filePath = path.resolve(process.cwd(), "contracts/football_bets.py");
  const contractCode = new Uint8Array(readFileSync(filePath));

  // 2. Initialize consensus
  await client.initializeConsensusSmartContract();

  // 3. Deploy the contract
  const deployTransaction = await client.deployContract({
    code: contractCode,
    args: [],  // Constructor arguments (none for this contract)
  });

  // 4. Wait for acceptance
  const receipt = await client.waitForTransactionReceipt({
    hash: deployTransaction as TransactionHash,
    status: TransactionStatus.ACCEPTED,
    retries: 200,
  });

  // 5. Extract the deployed address
  const deployedContractAddress =
    (client.chain as GenLayerChain).id === localnet.id
      ? receipt.data.contract_address
      : (receipt.txDataDecoded as DecodedDeployData)?.contractAddress;

  console.log(`Contract deployed at address: ${deployedContractAddress}`);
}
```

Key points:
- The function receives a pre-configured `GenLayerClient` from the CLI
- Constructor args are passed in the `args` array
- Address extraction differs between localnet and other networks
- The function is the **default export** — the CLI calls it automatically

### 7.2 Network Selection

```bash
genlayer network
```

Available networks:
- **studionet**: Hosted GenLayer Studio (default RPC: `https://studio.genlayer.com/api`)
- **localnet**: Local GenLayer instance
- **testnet-***: Public test networks

### 7.3 Deployment Flow

```
1. genlayer network          → Select target network
2. npm run deploy            → Runs: genlayer deploy
   └── genlayer CLI reads deploy/deployScript.ts
       └── Reads contracts/football_bets.py
       └── Deploys via GenLayer client
       └── Waits for ACCEPTED/FINALIZED status
       └── Prints contract address
3. Copy address to frontend/.env
```

---

## 8. Testing Contracts

### 8.1 Test Framework (gltest)

GenLayer uses `gltest` — a Python testing framework that deploys contracts to a running GenLayer Studio and executes integration tests.

**Install**: `pip install genlayer-test==0.1.1`

**Key imports**:
```python
from gltest import get_contract_factory, default_account
from gltest.helpers import load_fixture
from gltest.assertions import tx_execution_succeeded
```

### 8.2 Writing Tests

Tests follow a **deploy → interact → assert** pattern:

```python
def deploy_contract():
    """Factory function that deploys the contract and verifies initial state."""
    factory = get_contract_factory("FootballBets")
    contract = factory.deploy()

    # Verify initial state is empty
    assert contract.get_points(args=[]) == {}
    assert contract.get_bets(args=[]) == {}
    return contract


def test_football_bets_success_win():
    # Deploy using fixture (caches deployment across tests)
    contract = load_fixture(deploy_contract)

    # Create a bet
    result = contract.create_bet(args=["2024-06-20", "Spain", "Italy", "1"])
    assert tx_execution_succeeded(result)

    # Verify the bet was created
    bets = contract.get_bets(args=[])
    assert bets == {
        default_account.address: {
            "2024-06-20_spain_italy": {
                "game_date": "2024-06-20",
                "has_resolved": False,
                "id": "2024-06-20_spain_italy",
                "predicted_winner": "1",
                "real_score": "",
                "real_winner": "",
                "resolution_url": "https://www.bbc.com/sport/football/scores-fixtures/2024-06-20",
                "team1": "Spain",
                "team2": "Italy",
            }
        }
    }

    # Resolve the bet (with extended timeout for AI processing)
    resolve_result = contract.resolve_bet(
        args=["2024-06-20_spain_italy"],
        wait_interval=10000,  # 10 seconds between checks
        wait_retries=15,      # Up to 15 retries
    )
    assert tx_execution_succeeded(resolve_result)

    # Verify points were awarded
    assert contract.get_player_points(args=[default_account.address]) == 1
```

**Key patterns**:
- `get_contract_factory("ClassName")` — finds the contract by class name
- `contract.method_name(args=[...])` — calls contract methods
- `tx_execution_succeeded(result)` — asserts a write transaction succeeded
- `load_fixture(fn)` — caches contract deployment across tests
- `default_account.address` — the test account's address
- `wait_interval` and `wait_retries` — for AI-dependent operations that take time

### 8.3 Test Fixtures & Schemas

The boilerplate includes a **contract schema** file that defines expected method signatures:

```python
football_bets_contract_schema = {
    "result": {
        "ctor": {"kwparams": {}, "params": []},
        "methods": {
            "create_bet": {
                "params": [
                    ["game_date", "string"],
                    ["team1", "string"],
                    ["team2", "string"],
                    ["predicted_winner", "string"],
                ],
                "readonly": False,
                "ret": "null",
            },
            "get_bets": {"params": [], "readonly": True, "ret": "dict"},
            "get_player_points": {
                "params": [["player_address", "string"]],
                "readonly": True,
                "ret": "int",
            },
            "get_points": {"params": [], "readonly": True, "ret": "dict"},
            "resolve_bet": {
                "params": [["bet_id", "string"]],
                "readonly": False,
                "ret": "null",
            },
        },
    },
}
```

And **expected state fixtures** for different scenarios (win resolved, draw unresolved, etc.).

### 8.4 Running Tests

```bash
# Ensure GenLayer Studio is running first
gltest
```

The test suite covers three scenarios:
1. **Successful win prediction** — user predicts correctly → gets 1 point
2. **Successful draw prediction** — user predicts draw correctly → gets 1 point
3. **Unsuccessful prediction** — user predicts wrong → gets 0 points

---

## 9. Frontend Architecture (Next.js)

### 9.1 Tech Stack Overview

| Technology | Version | Purpose |
|------------|---------|---------|
| Next.js | 16 | React framework (App Router) |
| React | 19 | UI library |
| TypeScript | 5.9 | Type safety |
| Tailwind CSS | 4 | Utility-first styling |
| TanStack Query | 5 | Data fetching, caching, polling |
| genlayer-js | 0.18 | GenLayer SDK |
| viem | 2.21 | Ethereum utilities |
| wagmi | 2.19 | Wallet connectors |
| Radix UI | — | Accessible primitives (Dialog, Label, Slot) |
| shadcn/ui | — | Pre-built components (Button, Badge, Alert, Input) |
| Sonner | 1.7 | Toast notifications |
| Lucide React | — | Icons |

### 9.2 App Structure

The frontend uses the **Next.js App Router** pattern:

```
app/
├── layout.tsx      ← Root layout: fonts, metadata, <Providers>
├── page.tsx        ← Home page: Navbar + BetsTable + Leaderboard
├── providers.tsx   ← QueryClientProvider + WalletProvider + Toaster
└── globals.css     ← Full design system (OKLCH colors, animations)
```

**Provider hierarchy** (in `providers.tsx`):
```
<QueryClientProvider>        ← TanStack Query (data fetching)
  <WalletProvider>           ← Custom wallet context (MetaMask state)
    {children}               ← App content
  </WalletProvider>
  <Toaster />                ← Sonner toast container
</QueryClientProvider>
```

### 9.3 Environment Configuration

**`.env.example`**:
```env
# GenLayer RPC endpoint
NEXT_PUBLIC_GENLAYER_RPC_URL=https://studio.genlayer.com/api

# Chain configuration
NEXT_PUBLIC_GENLAYER_CHAIN_ID=61999
NEXT_PUBLIC_GENLAYER_CHAIN_NAME=GenLayer Studio
NEXT_PUBLIC_GENLAYER_SYMBOL=GEN

# Your deployed contract address
NEXT_PUBLIC_CONTRACT_ADDRESS=your_contract_address
```

All variables use `NEXT_PUBLIC_` prefix for client-side access.

### 9.4 GenLayer Client Setup

**`lib/genlayer/client.ts`** — The core client module:

```typescript
import { createClient } from "genlayer-js";
import { studionet } from "genlayer-js/chains";

// Create a GenLayer client (with optional account for write operations)
export function createGenLayerClient(address?: string) {
  const config: any = {
    chain: studionet,
  };
  if (address) {
    config.account = address as `0x${string}`;
  }
  return createClient(config);
}
```

The module also provides:
- `getStudioUrl()` — returns the RPC URL from env
- `getContractAddress()` — returns the contract address from env
- `isMetaMaskInstalled()` — checks for `window.ethereum`
- `requestAccounts()` — triggers MetaMask connect popup
- `connectMetaMask()` — full connect flow (request accounts + switch network)
- `switchToGenLayerNetwork()` — adds/switches to GenLayer chain in MetaMask
- `switchAccount()` — shows MetaMask account picker
- `createMetaMaskWalletClient()` — creates a viem wallet client

**Network configuration** is built from env vars:
```typescript
export const GENLAYER_NETWORK = {
  chainId: "0xF21F",  // 61999 in hex
  chainName: "GenLayer Studio",
  nativeCurrency: { name: "GEN", symbol: "GEN", decimals: 18 },
  rpcUrls: ["https://studio.genlayer.com/api"],
  blockExplorerUrls: [],
};
```

### 9.5 Wallet Management (MetaMask)

**`lib/genlayer/WalletProvider.tsx`** — React Context for wallet state:

```typescript
export interface WalletState {
  address: string | null;
  chainId: string | null;
  isConnected: boolean;
  isLoading: boolean;
  isMetaMaskInstalled: boolean;
  isOnCorrectNetwork: boolean;
}

// Usage in components:
const {
  address,
  isConnected,
  isMetaMaskInstalled,
  isOnCorrectNetwork,
  isLoading,
  connectWallet,
  disconnectWallet,
  switchWalletAccount,
} = useWallet();
```

**Key behaviors**:
- **Auto-reconnect**: On page load, checks for existing MetaMask permissions (without prompting)
- **Disconnect persistence**: Stores a `wallet_disconnected` flag in localStorage to prevent auto-reconnect after intentional disconnect
- **Event listeners**: Listens for MetaMask's `accountsChanged`, `chainChanged`, and `disconnect` events
- **Network validation**: Checks if user is on the correct GenLayer chain

### 9.6 Contract Interaction Class

**`lib/contracts/FootballBets.ts`** — TypeScript class wrapping contract calls:

```typescript
class FootballBets {
  private contractAddress: `0x${string}`;
  private client: ReturnType<typeof createClient>;

  constructor(contractAddress: string, address?: string | null, studioUrl?: string) {
    this.contractAddress = contractAddress as `0x${string}`;
    this.client = createClient({
      chain: studionet,
      account: address as `0x${string}`,
      endpoint: studioUrl,
    });
  }

  // READ: Get all bets
  async getBets(): Promise<Bet[]> {
    const bets = await this.client.readContract({
      address: this.contractAddress,
      functionName: "get_bets",
      args: [],
    });
    // Convert GenLayer Map structure to typed array
    // GenLayer returns Maps, not plain objects
    // ...
  }

  // WRITE: Create a new bet
  async createBet(gameDate, team1, team2, predictedWinner): Promise<TransactionReceipt> {
    const txHash = await this.client.writeContract({
      address: this.contractAddress,
      functionName: "create_bet",
      args: [gameDate, team1, team2, predictedWinner],
      value: BigInt(0),
    });

    const receipt = await this.client.waitForTransactionReceipt({
      hash: txHash,
      status: "ACCEPTED",
      retries: 24,
      interval: 5000,
    });

    return receipt;
  }
}
```

**Critical pattern — GenLayer returns Maps, not objects**:
```typescript
// GenLayer readContract returns Map objects
if (bets instanceof Map) {
  return Array.from(bets.entries()).flatMap(([owner, betMap]) => {
    return Array.from((betMap as any).entries()).map(([id, betData]: any) => {
      // Convert Map entries to plain object
      const betObj = Array.from((betData as any).entries()).reduce(
        (obj, [key, value]) => { obj[key] = value; return obj; },
        {}
      );
      return { id, ...betObj, owner } as Bet;
    });
  });
}
```

**TypeScript types** (`lib/contracts/types.ts`):
```typescript
export interface Bet {
  id: string;
  game_date: string;
  team1: string;
  team2: string;
  predicted_winner: string;  // "1", "2", or "0"
  has_resolved: boolean;
  real_winner?: string;
  real_score?: string;
  resolution_url?: string;
  owner: string;
}

export interface LeaderboardEntry {
  address: string;
  points: number;
}

export interface TransactionReceipt {
  status: string;
  hash: string;
  blockNumber?: number;
  [key: string]: any;
}
```

### 9.7 React Hooks for Contract Data

**`lib/hooks/useFootballBets.ts`** — TanStack Query hooks:

#### Contract Instance Hook
```typescript
export function useFootballBetsContract(): FootballBets | null {
  const { address } = useWallet();
  const contractAddress = getContractAddress();
  const studioUrl = getStudioUrl();

  return useMemo(() => {
    if (!contractAddress) return null;
    return new FootballBets(contractAddress, address, studioUrl);
  }, [contractAddress, address, studioUrl]);
}
```

#### Query Hooks (Read)
```typescript
// All bets
export function useBets() {
  const contract = useFootballBetsContract();
  return useQuery<Bet[], Error>({
    queryKey: ["bets"],
    queryFn: () => contract?.getBets() ?? [],
    refetchOnWindowFocus: true,
    staleTime: 2000,
    enabled: !!contract,
  });
}

// Player points
export function usePlayerPoints(address: string | null) {
  const contract = useFootballBetsContract();
  return useQuery<number, Error>({
    queryKey: ["playerPoints", address],
    queryFn: () => contract?.getPlayerPoints(address) ?? 0,
    enabled: !!address && !!contract,
    staleTime: 2000,
  });
}

// Leaderboard
export function useLeaderboard() {
  const contract = useFootballBetsContract();
  return useQuery<LeaderboardEntry[], Error>({
    queryKey: ["leaderboard"],
    queryFn: () => contract?.getLeaderboard() ?? [],
    staleTime: 2000,
    enabled: !!contract,
  });
}
```

#### Mutation Hooks (Write)
```typescript
// Create bet
export function useCreateBet() {
  const contract = useFootballBetsContract();
  const { address } = useWallet();
  const queryClient = useQueryClient();

  const mutation = useMutation({
    mutationFn: async ({ gameDate, team1, team2, predictedWinner }) => {
      if (!contract) throw new Error("Contract not configured");
      if (!address) throw new Error("Wallet not connected");
      return contract.createBet(gameDate, team1, team2, predictedWinner);
    },
    onSuccess: () => {
      // Invalidate all related queries to trigger refetch
      queryClient.invalidateQueries({ queryKey: ["bets"] });
      queryClient.invalidateQueries({ queryKey: ["playerPoints"] });
      queryClient.invalidateQueries({ queryKey: ["leaderboard"] });
      success("Bet created successfully!");
    },
    onError: (err) => {
      error("Failed to create bet", { description: err.message });
    },
  });

  return { ...mutation, createBet: mutation.mutate };
}

// Resolve bet (similar pattern with resolvingBetId tracking)
export function useResolveBet() { /* ... */ }
```

### 9.8 UI Components

#### Navbar (`components/Navbar.tsx`)
- Fixed position with scroll-based animations (padding, height, border-radius)
- Shows GenLayer logo (full on desktop, mark on mobile)
- Center: bet statistics (Total Bets, Resolved)
- Right: Create Bet button + Account Panel

#### BetsTable (`components/BetsTable.tsx`)
- Table with columns: Date, Teams, Prediction, Status, Owner, Actions
- Shows badges for resolved/pending status
- "Resolve" button only visible to bet owner when unresolved
- Handles loading, error, empty, and setup-required states

#### Leaderboard (`components/Leaderboard.tsx`)
- Ranked list of players by points
- Trophy/Medal/Award icons for top 3
- Highlights current user with "You" badge
- Sorted highest-to-lowest

#### CreateBetModal (`components/CreateBetModal.tsx`)
- Dialog with form: date picker, team names, winner prediction (3-way selector)
- Form validation (all fields required)
- Auto-closes on success or wallet disconnect
- Disabled when wallet not connected

#### AccountPanel (`components/AccountPanel.tsx`)
- **Disconnected**: "Connect Wallet" button → MetaMask connection dialog
- **Connected**: Shows address + points in navbar card, account details dialog
- Features: connect, disconnect, switch account
- Handles MetaMask not installed with install link

#### AddressDisplay (`components/AddressDisplay.tsx`)
- Truncates addresses (e.g., `0x1234...5678`)
- Optional copy-to-clipboard button
- Configurable max length

#### Logo (`components/Logo.tsx`)
- GenLayer brand SVG logo
- Variants: `full` (mark + text), `mark` (icon only), `wordmark` (text only)
- Sizes: `sm`, `md`, `lg`
- Themes: `light`, `dark`

### 9.9 Styling & Design System

The project uses a custom **GenLayer Brand 2025** design system built on Tailwind CSS v4 with OKLCH colors:

**Color Palette**:
```css
--background: oklch(0 0 0);              /* Pure black */
--foreground: oklch(0.98 0 0);           /* Near white */
--primary: oklch(0.65 0.22 300);         /* Purple (#9B6AF6) */
--pink: oklch(0.78 0.18 330);            /* Pink (#E37DF7) */
--blue: oklch(0.55 0.37 265);            /* Blue (#110FFF) */
--card: oklch(0.25 0.08 265);            /* Navy (#282B5D) */
--destructive: oklch(0.65 0.25 25);      /* Red */
```

**Custom Utilities**:
- `.brand-card` — Navy background with border and shadow
- `.brand-navbar` — Black with backdrop blur
- `.btn-primary` — Purple-to-pink gradient
- `.btn-secondary` — Navy with border
- `.btn-blue` — Blue solid
- `.gradient-purple-pink` — Brand gradient background
- `.animate-fade-in` — Fade up animation
- `.animate-slide-up` — Slide up animation

**Typography**:
- Body: Inter (via `--font-body`)
- Headings: Space Grotesk (via `--font-display`)

**Background**: Animated gradient with subtle purple and blue radial gradients on black.

### 9.10 Toast Notifications

**`lib/utils/toast.ts`** — Branded toast helpers using Sonner:

```typescript
success("Bet created!");                                    // Green, 4s
error("Failed", { description: "Try again" });              // Red, 6s
warning("Network issue");                                   // Yellow, 5s
info("Tip: ...");                                           // Default, 3s
loading("Processing...");                                   // Infinite
promise(asyncFn, { loading: "...", success: "...", error: "..." });
configError("Setup required", "Missing config");            // Red, infinite
userRejected("Cancelled");                                  // Muted, 2s
```

---

## 10. How to Build Your Own dApp

### 10.1 Step 1: Write Your Contract

Create a new file in `/contracts/`:

```python
# contracts/my_contract.py
# { "Depends": "py-genlayer:test" }

from genlayer import *

class MyContract(gl.Contract):
    data: TreeMap[Address, str]

    def __init__(self):
        pass

    @gl.public.view
    def get_data(self, addr: Address) -> str:
        return self.data.get(addr, "")

    @gl.public.write
    def set_data(self, value: str) -> None:
        self.data[gl.message.sender_address] = value
```

### 10.2 Step 2: Write Your Deploy Script

Update `/deploy/deployScript.ts`:

```typescript
import { readFileSync } from "fs";
import path from "path";
import { TransactionHash, TransactionStatus, GenLayerClient, DecodedDeployData, GenLayerChain } from "genlayer-js/types";
import { localnet } from "genlayer-js/chains";

export default async function main(client: GenLayerClient<any>) {
  // Point to YOUR contract file
  const filePath = path.resolve(process.cwd(), "contracts/my_contract.py");
  const contractCode = new Uint8Array(readFileSync(filePath));

  await client.initializeConsensusSmartContract();

  const deployTransaction = await client.deployContract({
    code: contractCode,
    args: [],  // Pass constructor args here if needed
  });

  const receipt = await client.waitForTransactionReceipt({
    hash: deployTransaction as TransactionHash,
    status: TransactionStatus.ACCEPTED,
    retries: 200,
  });

  if (receipt.status !== 5 && receipt.status !== 6 &&
      receipt.statusName !== "ACCEPTED" && receipt.statusName !== "FINALIZED") {
    throw new Error(`Deployment failed. Receipt: ${JSON.stringify(receipt)}`);
  }

  const deployedContractAddress =
    (client.chain as GenLayerChain).id === localnet.id
      ? receipt.data.contract_address
      : (receipt.txDataDecoded as DecodedDeployData)?.contractAddress;

  console.log(`Contract deployed at address: ${deployedContractAddress}`);
}
```

### 10.3 Step 3: Write Your Tests

Create `/test/test_my_contract.py`:

```python
from gltest import get_contract_factory, default_account
from gltest.helpers import load_fixture
from gltest.assertions import tx_execution_succeeded


def deploy_contract():
    factory = get_contract_factory("MyContract")
    contract = factory.deploy()
    initial_data = contract.get_data(args=[default_account.address])
    assert initial_data == ""
    return contract


def test_set_and_get():
    contract = load_fixture(deploy_contract)

    result = contract.set_data(args=["hello world"])
    assert tx_execution_succeeded(result)

    data = contract.get_data(args=[default_account.address])
    assert data == "hello world"
```

### 10.4 Step 4: Build the Frontend Contract Class

Create `/frontend/lib/contracts/MyContract.ts`:

```typescript
import { createClient } from "genlayer-js";
import { studionet } from "genlayer-js/chains";

class MyContract {
  private contractAddress: `0x${string}`;
  private client: ReturnType<typeof createClient>;

  constructor(contractAddress: string, address?: string | null) {
    this.contractAddress = contractAddress as `0x${string}`;
    const config: any = { chain: studionet };
    if (address) config.account = address as `0x${string}`;
    this.client = createClient(config);
  }

  async getData(addr: string): Promise<string> {
    const result = await this.client.readContract({
      address: this.contractAddress,
      functionName: "get_data",
      args: [addr],
    });
    return String(result) || "";
  }

  async setData(value: string): Promise<any> {
    const txHash = await this.client.writeContract({
      address: this.contractAddress,
      functionName: "set_data",
      args: [value],
      value: BigInt(0),
    });

    return this.client.waitForTransactionReceipt({
      hash: txHash,
      status: "ACCEPTED" as any,
      retries: 24,
      interval: 5000,
    });
  }
}

export default MyContract;
```

### 10.5 Step 5: Create React Hooks

Create `/frontend/lib/hooks/useMyContract.ts`:

```typescript
"use client";

import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";
import { useMemo } from "react";
import MyContract from "../contracts/MyContract";
import { getContractAddress } from "../genlayer/client";
import { useWallet } from "../genlayer/wallet";

export function useMyContract(): MyContract | null {
  const { address } = useWallet();
  const contractAddress = getContractAddress();

  return useMemo(() => {
    if (!contractAddress) return null;
    return new MyContract(contractAddress, address);
  }, [contractAddress, address]);
}

export function useData(addr: string | null) {
  const contract = useMyContract();

  return useQuery({
    queryKey: ["myData", addr],
    queryFn: () => contract?.getData(addr!) ?? "",
    enabled: !!contract && !!addr,
    staleTime: 2000,
  });
}

export function useSetData() {
  const contract = useMyContract();
  const { address } = useWallet();
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: async (value: string) => {
      if (!contract) throw new Error("Contract not configured");
      if (!address) throw new Error("Wallet not connected");
      return contract.setData(value);
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ["myData"] });
    },
  });
}
```

### 10.6 Step 6: Build UI Components

Use the existing component patterns. The boilerplate gives you:
- `useWallet()` for wallet state
- `<Button variant="gradient">` for branded buttons
- `<Dialog>` for modals
- `<Badge>` for status indicators
- `<AddressDisplay>` for addresses
- Toast helpers for notifications
- Brand CSS utilities (`.brand-card`, `.btn-primary`, etc.)

### 10.7 Step 7: Deploy & Connect

```bash
# 1. Select network
genlayer network

# 2. Deploy
npm run deploy

# 3. Copy address to frontend/.env
# NEXT_PUBLIC_CONTRACT_ADDRESS=0x...

# 4. Run frontend
cd frontend && npm run dev
```

---

## 11. Key Patterns & Conventions

### Contract Patterns
- **Bet IDs**: Generated as `f"{date}_{team1}_{team2}".lower()` — deterministic, human-readable
- **Nested TreeMaps**: `TreeMap[Address, TreeMap[str, Bet]]` — per-user storage
- **`get_or_insert_default`**: Creates the inner map if it doesn't exist
- **Address conversion**: Use `k.as_hex` when returning addresses as dict keys
- **Error handling**: `raise Exception("message")` for contract errors
- **Non-deterministic pattern**: Define inner function → wrap with equivalence principle → parse result

### Frontend Patterns
- **Contract class**: Encapsulates all contract calls, handles Map→Object conversion
- **Hook per operation**: Separate hooks for reads (`useQuery`) and writes (`useMutation`)
- **Query invalidation**: On successful mutation, invalidate all affected queries
- **Null contract**: Return `null` from `useContract()` when address not configured → show "Setup Required" UI
- **Wallet guard**: Check `!address` before write mutations, show error toast
- **Map parsing**: GenLayer SDK returns `Map` objects — always convert with `Array.from(map.entries())`

### Project Patterns
- **Workspace**: Root `package.json` uses npm workspaces with `["frontend"]`
- **Scripts**: Root scripts proxy to frontend (`cd frontend && npm run dev`)
- **Deploy script**: Default export function receiving pre-configured client
- **Test fixtures**: Use `load_fixture()` for deployment caching across tests

---

## 12. Complete File Reference

| File | Purpose | Language |
|------|---------|----------|
| `package.json` | Root workspace config, deploy script | JSON |
| `tsconfig.json` | Root TypeScript config (ES2022, bundler) | JSON |
| `requirements.txt` | Python test dependencies | Text |
| `contracts/football_bets.py` | Football betting intelligent contract | Python |
| `deploy/deployScript.ts` | Contract deployment script | TypeScript |
| `test/test_footbal_bet.py` | Integration tests (3 scenarios) | Python |
| `test/football_bets_get_contract_schema_for_code.py` | Schema + test fixtures | Python |
| `config/genlayer_config.py` | RPC config loader (dotenv) | Python |
| `frontend/package.json` | Frontend dependencies (Next, React, genlayer-js) | JSON |
| `frontend/.env.example` | Environment template | env |
| `frontend/next.config.ts` | Next.js config (strict mode, turbopack) | TypeScript |
| `frontend/tsconfig.json` | Frontend TS config (strict, paths alias) | JSON |
| `frontend/tailwind.config.ts` | Tailwind content paths | TypeScript |
| `frontend/postcss.config.mjs` | PostCSS with @tailwindcss/postcss | JavaScript |
| `frontend/components.json` | shadcn/ui config (new-york style, aliases) | JSON |
| `frontend/app/layout.tsx` | Root layout: fonts (Inter, Space Grotesk), metadata | TSX |
| `frontend/app/page.tsx` | Home: Navbar, BetsTable, Leaderboard, How It Works | TSX |
| `frontend/app/providers.tsx` | QueryClient + WalletProvider + Toaster | TSX |
| `frontend/app/globals.css` | Full design system (OKLCH, animations, brand CSS) | CSS |
| `frontend/lib/utils.ts` | `cn()` helper (clsx + tailwind-merge) | TypeScript |
| `frontend/lib/utils/toast.ts` | Toast notification helpers (Sonner) | TypeScript |
| `frontend/lib/genlayer/client.ts` | GenLayer client, MetaMask helpers, network config | TypeScript |
| `frontend/lib/genlayer/wallet.ts` | Re-exports from WalletProvider + formatAddress | TypeScript |
| `frontend/lib/genlayer/WalletProvider.tsx` | React context: wallet state, connect/disconnect | TSX |
| `frontend/lib/contracts/FootballBets.ts` | Contract interaction class (read/write) | TypeScript |
| `frontend/lib/contracts/types.ts` | Bet, LeaderboardEntry, TransactionReceipt types | TypeScript |
| `frontend/lib/hooks/useFootballBets.ts` | TanStack Query hooks for contract | TypeScript |
| `frontend/components/Navbar.tsx` | Navigation with scroll animations | TSX |
| `frontend/components/BetsTable.tsx` | Bets table with resolve action | TSX |
| `frontend/components/Leaderboard.tsx` | Points leaderboard | TSX |
| `frontend/components/CreateBetModal.tsx` | Bet creation form dialog | TSX |
| `frontend/components/AccountPanel.tsx` | Wallet management dialog | TSX |
| `frontend/components/AddressDisplay.tsx` | Address truncation + copy | TSX |
| `frontend/components/Logo.tsx` | GenLayer SVG logo component | TSX |
| `frontend/components/ui/button.tsx` | Button (variants: gradient, blue, outline, etc.) | TSX |
| `frontend/components/ui/dialog.tsx` | Dialog/Modal (Radix UI) | TSX |
| `frontend/components/ui/input.tsx` | Input field | TSX |
| `frontend/components/ui/label.tsx` | Label (Radix UI) | TSX |
| `frontend/components/ui/badge.tsx` | Badge (default, secondary, destructive, outline) | TSX |
| `frontend/components/ui/alert.tsx` | Alert (default, destructive) | TSX |

---

## 13. Quick Command Reference

```bash
# ─── Setup ───────────────────────────────────
npm install                          # Install root dependencies
cd frontend && npm install           # Install frontend dependencies
pip install -r requirements.txt      # Install Python test dependencies
cp frontend/.env.example frontend/.env  # Create env config

# ─── Network ─────────────────────────────────
genlayer network                     # Select network (studionet/localnet/testnet)

# ─── Deploy ──────────────────────────────────
npm run deploy                       # Deploy contract (runs: genlayer deploy)

# ─── Frontend ────────────────────────────────
npm run dev                          # Start dev server (localhost:3000)
npm run build                        # Production build
npm run start                        # Start production server
npm run lint                         # Run ESLint

# ─── Testing ─────────────────────────────────
gltest                               # Run all contract tests

# ─── Direct Frontend Commands ────────────────
cd frontend
bun dev                              # Dev server with Bun
bun install                          # Install with Bun
npm run dev                          # Dev server with npm
```

---

**License**: MIT

**Built with**: GenLayer, genlayer-js SDK, Next.js 15, React 19, TanStack Query, Tailwind CSS v4, MetaMask

**Resources**:
- [GenLayer Documentation](https://docs.genlayer.com/)
- [GenLayerJS SDK](https://docs.genlayer.com/api-references/genlayer-js)
- [SDK API Reference](https://sdk.genlayer.com/main/_static/ai/api.txt)
- [GenLayer Studio](https://studio.genlayer.com/)
