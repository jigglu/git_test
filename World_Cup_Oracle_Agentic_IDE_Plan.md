# ⚡ World Cup Oracle + Micro-Markets on Solana
## Comprehensive Implementation Blueprint for Agentic IDE Execution

---

## Phase 1: Monorepo Setup & Architecture Skeleton

### Task 1.1: Initialize Turborepo Monorepo Structure
**Objective:** Create the foundational monorepo structure using Turborepo with dedicated workspaces for contracts, frontend, backend, and shared packages.  
**Target Files:** `package.json`, `turbo.json`, `pnpm-workspace.yaml`, `.gitignore`, `.nvmrc`

**Prompt for Agentic IDE:**
> Create a Turborepo monorepo with pnpm workspaces. 
>
> **Structure:**
> ```text
> /apps
>   /web          # Next.js 14+ frontend
>   /relayer      # Node.js backend service
> /packages
>   /contracts    # Anchor Solana programs
>   /shared       # Shared TypeScript types, constants, utilities
>   /ui           # Shared UI components (if needed)
> ```
> 
> **Root package.json:**
> - name: "world-cup-oracle"
> - private: true
> - Use pnpm with workspace protocol
> - Scripts: "dev", "build", "test", "lint" that run turbo
> 
> **turbo.json:**
> - Configure pipeline for build, dev, test, lint
> - Set proper task dependencies (shared builds before apps)
> - Enable caching
> 
> **pnpm-workspace.yaml:**
> - Include "apps/*" and "packages/*"
> 
> **.nvmrc:** Set Node.js version to 20.x
> 
> **.gitignore:** Include node_modules, .turbo, target, .anchor, .env*, dist, .next

---

### Task 1.2: Initialize Anchor Workspace for Solana Programs
**Objective:** Set up the Anchor framework workspace within the contracts package with proper Solana toolchain configuration.  
**Target Files:** `packages/contracts/Anchor.toml`, `packages/contracts/Cargo.toml`, `packages/contracts/programs/oracle/Cargo.toml`, `packages/contracts/programs/prediction_market/Cargo.toml`

**Prompt for Agentic IDE:**
> Initialize an Anchor workspace in `packages/contracts/` with TWO programs:
> 1. "oracle" - handles TxLINE data ingestion and verification
> 2. "prediction_market" - handles market creation, betting, settlement
> 
> **Anchor.toml configuration:**
> - cluster = "localnet" (with commented devnet/mainnet alternatives)
> - wallet path = "~/.config/solana/id.json"
> - Define both programs with their program IDs (generate placeholder keypairs)
> - Set `[scripts]` for test command
> - Use Anchor version 0.30.x compatible settings
> 
> **Root Cargo.toml (workspace):**
> - members = ["programs/*"]
> - Define shared dependencies with workspace inheritance:
>   - anchor-lang = "0.30.1"
>   - anchor-spl = "0.30.1"
>   - solana-program = "1.18"
> 
> **Each program Cargo.toml:**
> - Inherit workspace dependencies
> - Set crate-type = ["cdylib", "lib"]
> - Enable "no-entrypoint" feature for testing
> - Name them "oracle" and "prediction_market" respectively
> 
> **Create placeholder lib.rs for each program with:**
> - `declare_id!` macro with placeholder pubkey
> - Empty program module with `anchor_lang::prelude::*`

---

### Task 1.3: Initialize Next.js 14+ Frontend Application
**Objective:** Create the Next.js frontend application with App Router, Tailwind CSS, and shadcn/ui pre-configured.  
**Target Files:** `apps/web/package.json`, `apps/web/next.config.js`, `apps/web/tailwind.config.ts`, `apps/web/tsconfig.json`, `apps/web/app/layout.tsx`, `apps/web/app/page.tsx`, `apps/web/components.json`

**Prompt for Agentic IDE:**
> Initialize Next.js 14+ application in `apps/web/` with these specifications:
> 
> **package.json dependencies:**
> - next: ^14.2.x
> - react: ^18.3.x
> - react-dom: ^18.3.x
> - tailwindcss: ^3.4.x
> - framer-motion: ^11.x
> - @solana/web3.js: ^1.95.x
> - @solana/wallet-adapter-react: ^0.15.x
> - @solana/wallet-adapter-react-ui: ^0.9.x
> - @solana/wallet-adapter-wallets: ^0.19.x
> - @coral-xyz/anchor: ^0.30.x
> - lucide-react (for icons)
> - class-variance-authority, clsx, tailwind-merge (for shadcn)
> - zustand (state management)
> - @tanstack/react-query: ^5.x
> 
> **next.config.js:**
> - Enable `experimental.serverActions`
> - Configure webpack to handle Solana packages (fallback for crypto, stream, etc.)
> - Set `transpilePackages` for workspace packages
> 
> **tailwind.config.ts:**
> - Content paths include `./app`, `./components`, `./packages/ui`
> - Extend theme with custom colors for:
>   - primary: electric blue (#3B82F6)
>   - accent: vibrant green (#10B981) for wins
>   - danger: red (#EF4444) for losses
>   - dark background shades (#0A0A0F, #12121A, #1A1A2E)
> - Add custom animations: "fade-in", "slide-up", "pulse-glow"
> - Configure dark mode as "class"
> 
> **Initialize shadcn/ui:**
> - `components.json` with New York style, zinc base color
> - CSS variables enabled
> - Tailwind config path set correctly
> - Import alias: `@/components`
> 
> **Create base app/layout.tsx:**
> - Import globals.css
> - Set up dark mode by default (class="dark" on html)
> - Use Inter font from next/font/google
> - Basic metadata for "World Cup Oracle"
> 
> **Create placeholder app/page.tsx:**
> - Simple "World Cup Oracle" heading to verify setup works

---

### Task 1.4: Initialize Backend Relayer Service
**Objective:** Create the Node.js/TypeScript backend service structure for the oracle relayer with proper configuration.  
**Target Files:** `apps/relayer/package.json`, `apps/relayer/tsconfig.json`, `apps/relayer/src/index.ts`, `apps/relayer/.env.example`

**Prompt for Agentic IDE:**
> Initialize Node.js TypeScript service in `apps/relayer/`:
> 
> **package.json:**
> - name: "@world-cup-oracle/relayer"
> - type: "module" (ESM)
> - Dependencies:
>   - @solana/web3.js: ^1.95.x
>   - @coral-xyz/anchor: ^0.30.x
>   - dotenv: ^16.x
>   - zod: ^3.x (for validation)
>   - pino: ^9.x (logging)
>   - node-cron: ^3.x (scheduled tasks)
>   - tweetnacl: ^1.x (for Ed25519 signature verification)
>   - bs58: ^6.x
> - DevDependencies:
>   - typescript: ^5.4.x
>   - tsx: ^4.x (for running TS directly)
>   - @types/node: ^20.x
>   - vitest: ^1.x
> 
> **tsconfig.json:**
> - target: "ES2022"
> - module: "NodeNext"
> - moduleResolution: "NodeNext"
> - strict: true
> - outDir: "./dist"
> - rootDir: "./src"
> - Include workspace shared package
> 
> **Scripts:**
> - "dev": "tsx watch src/index.ts"
> - "build": "tsc"
> - "start": "node dist/index.js"
> - "test": "vitest"
> 
> **Create src/index.ts placeholder:**
> - Import dotenv/config
> - Log "Relayer service starting..."
> - Export empty main function
> 
> **.env.example:**
> - SOLANA_RPC_URL=http://localhost:8899
> - SOLANA_PRIVATE_KEY=
> - TXLINE_API_URL=
> - TXLINE_API_KEY=
> - ORACLE_PROGRAM_ID=
> - MARKET_PROGRAM_ID=

---

### Task 1.5: Create Shared Types Package
**Objective:** Establish the shared TypeScript types package with core domain types for matches, markets, and oracle data structures.  
**Target Files:** `packages/shared/package.json`, `packages/shared/tsconfig.json`, `packages/shared/src/index.ts`, `packages/shared/src/types/match.ts`, `packages/shared/src/types/market.ts`, `packages/shared/src/types/oracle.ts`, `packages/shared/src/constants.ts`

**Prompt for Agentic IDE:**
> Create shared TypeScript package in `packages/shared/`:
> 
> **package.json:**
> - name: "@world-cup-oracle/shared"
> - main: "./dist/index.js"
> - types: "./dist/index.d.ts"
> - exports with proper ESM/CJS dual support
> - Scripts: "build": "tsc", "dev": "tsc --watch"
> 
> **tsconfig.json:**
> - Strict mode, ES2022 target
> - Declaration: true, declarationMap: true
> - Composite: true (for project references)
> 
> **src/types/match.ts:**
> - `MatchStatus` enum: "Scheduled", "Live", "HalfTime", "Finished", "Cancelled"
> - `MatchData` interface:
>   - matchId: string (unique identifier from TxLINE)
>   - homeTeam: string
>   - awayTeam: string
>   - homeScore: number
>   - awayScore: number
>   - currentMinute: number
>   - status: MatchStatus
>   - timestamp: number (unix)
>   - events: MatchEvent[] (goals, corners, cards)
> - `MatchEvent` interface:
>   - type: "Goal" | "Corner" | "YellowCard" | "RedCard" | "Penalty"
>   - minute: number
>   - team: "home" | "away"
>   - player?: string
> 
> **src/types/market.ts:**
> - `MarketType` enum: "GoalInPeriod", "NextCorner", "NextCard", "HalfTimeScore", "FullTimeWinner"
> - `MarketStatus` enum: "Open", "Locked", "Resolved", "Cancelled"
> - `MicroMarket` interface:
>   - marketId: string (on-chain PDA as base58)
>   - matchId: string
>   - marketType: MarketType
>   - description: string
>   - parameters: MarketParameters (start minute, end minute, condition)
>   - yesPool: bigint (lamports)
>   - noPool: bigint (lamports)
>   - status: MarketStatus
>   - outcome?: boolean
>   - createdAt: number
>   - resolvesAt: number
> 
> **src/types/oracle.ts:**
> - `OracleUpdate` interface:
>   - matchId: string
>   - data: MatchData
>   - signature: Uint8Array (Ed25519 from TxLINE)
>   - publicKey: Uint8Array (TxLINE's signing key)
>   - timestamp: number
> 
> **src/constants.ts:**
> - LAMPORTS_PER_SOL: 1_000_000_000
> - MIN_BET_AMOUNT: 0.01 SOL in lamports
> - MAX_BET_AMOUNT: 10 SOL in lamports
> - PLATFORM_FEE_BPS: 200 (2%)
> - MARKET_DURATION_MINUTES: { GoalInPeriod: 10, NextCorner: 5, NextCard: 15 }
> - ORACLE_AUTHORITY_PUBKEY: placeholder
> 
> **src/index.ts:**
> - Re-export all types and constants

---

## Phase 2: Smart Contract Development (Anchor)

### Subphase 2A: Oracle Program

#### Task 2A.1: Define Oracle Program State Accounts
**Objective:** Create the account structures for the Oracle program including the global config, authorized signers registry, and match data storage.  
**Target Files:** `packages/contracts/programs/oracle/src/state/mod.rs`, `packages/contracts/programs/oracle/src/state/config.rs`, `packages/contracts/programs/oracle/src/state/match_data.rs`, `packages/contracts/programs/oracle/src/state/signer_registry.rs`

**Prompt for Agentic IDE:**
> Create Oracle program state accounts in `packages/contracts/programs/oracle/src/state/`:
> 
> **mod.rs:**
> - Declare and re-export: config, match_data, signer_registry modules
> 
> **config.rs - OracleConfig account:**
> - `#[account]` derive macro
> - Fields:
>   - authority: Pubkey (admin who can update config)
>   - market_program: Pubkey (authorized prediction market program)
>   - fee_receiver: Pubkey
>   - is_paused: bool
>   - total_updates: u64 (counter)
>   - bump: u8
> - PDA seeds: `[b"oracle_config"]`
> - Space calculation: 8 (discriminator) + 32 + 32 + 32 + 1 + 8 + 1 = 114
> - Implement Default trait
> 
> **signer_registry.rs - SignerRegistry account:**
> - `#[account]` derive macro
> - Purpose: Store list of authorized TxLINE public keys that can sign match data
> - Fields:
>   - authority: Pubkey
>   - authorized_signers: Vec<Pubkey> (max 10 signers)
>   - bump: u8
> - PDA seeds: `[b"signer_registry"]`
> - Space: 8 + 32 + 4 + (32 * 10) + 1 = 365
> - Methods:
>   - `is_authorized(&self, pubkey: &Pubkey) -> bool`
>   - `add_signer(&mut self, pubkey: Pubkey) -> Result<()>`
>   - `remove_signer(&mut self, pubkey: &Pubkey) -> Result<()>`
> 
> **match_data.rs - MatchDataAccount:**
> - `#[account]` derive macro
> - Fields:
>   - match_id: [u8; 32] (fixed-size hash of external match ID)
>   - home_team: [u8; 32] (padded string)
>   - away_team: [u8; 32] (padded string)
>   - home_score: u8
>   - away_score: u8
>   - current_minute: u8
>   - status: MatchStatus (enum: Scheduled=0, Live=1, HalfTime=2, Finished=3, Cancelled=4)
>   - total_goals: u8
>   - total_corners: u8
>   - total_cards: u8
>   - last_event_type: EventType (enum: None=0, Goal=1, Corner=2, YellowCard=3, RedCard=4, Penalty=5)
>   - last_event_minute: u8
>   - last_event_team: u8 (0=none, 1=home, 2=away)
>   - last_update_slot: u64
>   - last_update_timestamp: i64
>   - update_count: u32
>   - bump: u8
> - PDA seeds: `[b"match", match_id.as_ref()]`
> - Space calculation with all fields
> - Include MatchStatus and EventType as separate enums with `#[derive(AnchorSerialize, AnchorDeserialize, Clone, Copy, PartialEq, Eq)]`

#### Task 2A.2: Define Oracle Program Error Codes
**Objective:** Create comprehensive error codes for the Oracle program covering all failure scenarios.  
**Target Files:** `packages/contracts/programs/oracle/src/errors.rs`

**Prompt for Agentic IDE:**
> Create Oracle program errors in `packages/contracts/programs/oracle/src/errors.rs`:
> Use `#[error_code]` attribute macro for Anchor errors.
> 
> Define `OracleError` enum with these variants (include descriptive `#[msg("...")]` for each):
> 
> **Authorization errors:**
> - Unauthorized: "Caller is not authorized to perform this action"
> - InvalidSignature: "The provided Ed25519 signature is invalid"
> - SignerNotAuthorized: "The signing public key is not in the authorized signers list"
> - InvalidSignerPublicKey: "Invalid signer public key format"
> 
> **State errors:**
> - OraclePaused: "Oracle is currently paused"
> - MatchAlreadyExists: "Match data account already exists"
> - MatchNotFound: "Match data account not found"
> - InvalidMatchStatus: "Invalid match status transition"
> 
> **Data validation errors:**
> - InvalidMatchId: "Match ID format is invalid"
> - StaleData: "Update timestamp is older than existing data"
> - DataTooOld: "Data timestamp is too far in the past"
> - InvalidScore: "Score values are invalid"
> - InvalidMinute: "Match minute value is out of range"
> - TimestampInFuture: "Timestamp cannot be in the future"
> 
> **Registry errors:**
> - SignerRegistryFull: "Maximum number of authorized signers reached"
> - SignerAlreadyExists: "Signer is already in the authorized list"
> - SignerNotFound: "Signer not found in the authorized list"
> - CannotRemoveLastSigner: "Cannot remove the last authorized signer"
> 
> **Constraint errors:**
> - InvalidProgramId: "Invalid program ID for CPI"
> - InvalidAccountOwner: "Account has incorrect owner"

#### Task 2A.3: Create Oracle Initialization Instruction
**Objective:** Implement the initialize instruction to set up the Oracle config and signer registry PDAs.  
**Target Files:** `packages/contracts/programs/oracle/src/instructions/mod.rs`, `packages/contracts/programs/oracle/src/instructions/initialize.rs`

**Prompt for Agentic IDE:**
> Create Oracle initialize instruction in `packages/contracts/programs/oracle/src/instructions/`:
> 
> **mod.rs:**
> - Declare and re-export: `initialize` module
> - (Will add more modules in subsequent tasks)
> 
> **initialize.rs:**
> Define `Initialize` accounts struct with `#[derive(Accounts)]`:
> - authority: Signer<'info> (pays for account creation, becomes admin)
> - oracle_config: Account<'info, OracleConfig> using init, payer, space, seeds=[b"oracle_config"], bump
> - signer_registry: Account<'info, SignerRegistry> using init, payer, space, seeds=[b"signer_registry"], bump
> - market_program: UncheckedAccount<'info> (the prediction market program ID to authorize)
> - fee_receiver: UncheckedAccount<'info>
> - system_program: Program<'info, System>
> 
> Define `InitializeParams` struct:
> - initial_signer: Pubkey (first authorized TxLINE signer)
> 
> Implement initialize handler function:
> ```rust
> pub fn initialize(ctx: Context<Initialize>, params: InitializeParams) -> Result<()>
> ```
> **Logic:**
> 1. Get bumps from ctx.bumps
> 2. Initialize oracle_config:
>    - authority = ctx.accounts.authority.key()
>    - market_program = ctx.accounts.market_program.key()
>    - fee_receiver = ctx.accounts.fee_receiver.key()
>    - is_paused = false
>    - total_updates = 0
>    - bump = config bump
> 3. Initialize signer_registry:
>    - authority = ctx.accounts.authority.key()
>    - authorized_signers = vec![params.initial_signer]
>    - bump = registry bump
> 4. Emit an InitializedEvent with authority and initial signer
> 
> **Define InitializedEvent:**
> ```rust
> #[event]
> pub struct InitializedEvent {
>     pub authority: Pubkey,
>     pub initial_signer: Pubkey,
>     pub timestamp: i64,
> }
> ```

#### Task 2A.4: Create Oracle Update Match Data Instruction
**Objective:** Implement the core instruction that receives signed match data from TxLINE, verifies the Ed25519 signature, and writes/updates on-chain match state.  
**Target Files:** `packages/contracts/programs/oracle/src/instructions/update_match.rs`

**Prompt for Agentic IDE:**
> Create update_match instruction in `packages/contracts/programs/oracle/src/instructions/update_match.rs`:
> 
> This is the CRITICAL instruction for oracle functionality. It must:
> 1. Verify Ed25519 signature from TxLINE
> 2. Check signer is authorized
> 3. Create or update match data account
> 4. Emit events for market program to listen
> 
> **Define UpdateMatch accounts struct:**
> - relayer: Signer<'info> (pays gas, not the data signer)
> - oracle_config: Account<'info, OracleConfig> (verify not paused)
> - signer_registry: Account<'info, SignerRegistry> (check authorized signer)
> - match_account: Account<'info, MatchDataAccount> using init_if_needed with:
>   - seeds = [b"match", params.match_id.as_ref()]
>   - bump
>   - payer = relayer
>   - space = MatchDataAccount::SPACE
> - ed25519_program: UncheckedAccount<'info> (Ed25519SigVerify111111111111111111111111111)
> - instructions_sysvar: UncheckedAccount<'info> (Sysvar1nstructions1111111111111111111111111)
> - system_program: Program<'info, System>
> 
> **Define UpdateMatchParams struct:**
> - match_id: [u8; 32]
> - home_team: [u8; 32]
> - away_team: [u8; 32]
> - home_score: u8
> - away_score: u8
> - current_minute: u8
> - status: u8
> - total_goals: u8
> - total_corners: u8
> - total_cards: u8
> - last_event_type: u8
> - last_event_minute: u8
> - last_event_team: u8
> - timestamp: i64
> - signature: [u8; 64]
> - signer_pubkey: [u8; 32]
> 
> **Implement update_match handler:**
> 1. Check oracle_config.is_paused == false, else OraclePaused error
> 2. Verify signer_pubkey is in signer_registry.authorized_signers:
>    - Convert signer_pubkey bytes to Pubkey
>    - Call signer_registry.is_authorized()
>    - Error: SignerNotAuthorized
> 3. Verify Ed25519 signature:
>    - Build the message that was signed (serialize all match data fields in deterministic order)
>    - Use solana_program::ed25519_program to verify
>    - IMPORTANT: Must verify signature was checked in a previous instruction via instructions sysvar
>    - Pattern: Check instructions_sysvar for Ed25519Program instruction at index 0
>    - Parse the Ed25519 instruction data to extract signature, pubkey, message
>    - Verify they match our params
>    - Error if invalid: InvalidSignature
> 4. Validate timestamp:
>    - Cannot be in future (with 60 second tolerance)
>    - Must be newer than match_account.last_update_timestamp (if account exists)
>    - Error: StaleData or TimestampInFuture
> 5. Validate data ranges:
>    - current_minute <= 120 (with extra time)
>    - status in valid range 0-4
>    - scores reasonable (< 20)
> 6. Update match_account fields:
>    - Set all fields from params
>    - Increment update_count
>    - Set last_update_slot = Clock::get()?.slot
>    - Set bump if newly initialized
> 7. Increment oracle_config.total_updates
> 8. Emit MatchUpdatedEvent:
>    - match_id
>    - status
>    - home_score
>    - away_score
>    - current_minute
>    - last_event_type
>    - timestamp
> 
> Add import for Clock sysvar.

#### Task 2A.5: Create Oracle Admin Instructions
**Objective:** Implement admin-only instructions for managing the oracle: pause/unpause, add/remove signers, update config.  
**Target Files:** `packages/contracts/programs/oracle/src/instructions/admin.rs`

**Prompt for Agentic IDE:**
> Create admin instructions in `packages/contracts/programs/oracle/src/instructions/admin.rs`:
> All admin instructions require authority signature matching oracle_config.authority.
> 
> **1. PAUSE/UNPAUSE ORACLE:**
> Define SetPaused accounts:
> - authority: Signer<'info>
> - oracle_config: Account<'info, OracleConfig> mut, has_one = authority
> 
> ```rust
> pub fn set_paused(ctx: Context<SetPaused>, paused: bool) -> Result<()>
> ```
> - Set oracle_config.is_paused = paused
> - Emit PausedEvent { paused, timestamp }
> 
> **2. ADD AUTHORIZED SIGNER:**
> Define AddSigner accounts:
> - authority: Signer<'info>
> - oracle_config: Account<'info, OracleConfig> has_one = authority
> - signer_registry: Account<'info, SignerRegistry> mut, has_one = authority
> 
> ```rust
> pub fn add_signer(ctx: Context<AddSigner>, new_signer: Pubkey) -> Result<()>
> ```
> - Check vec length < 10, else SignerRegistryFull
> - Check signer not already present, else SignerAlreadyExists
> - Push new_signer to authorized_signers
> - Emit SignerAddedEvent { signer: new_signer }
> 
> **3. REMOVE AUTHORIZED SIGNER:**
> Define RemoveSigner accounts (same as AddSigner)
> ```rust
> pub fn remove_signer(ctx: Context<RemoveSigner>, signer_to_remove: Pubkey) -> Result<()>
> ```
> - Check vec length > 1, else CannotRemoveLastSigner
> - Find and remove signer, else SignerNotFound
> - Emit SignerRemovedEvent { signer: signer_to_remove }
> 
> **4. UPDATE CONFIG:**
> Define UpdateConfig accounts:
> - authority: Signer<'info>
> - oracle_config: Account<'info, OracleConfig> mut, has_one = authority
> - new_authority: Option<UncheckedAccount<'info>>
> - new_fee_receiver: Option<UncheckedAccount<'info>>
> - new_market_program: Option<UncheckedAccount<'info>>
> 
> ```rust
> pub fn update_config(ctx: Context<UpdateConfig>, params: UpdateConfigParams) -> Result<()>
> ```
> 
> UpdateConfigParams:
> - new_authority: Option<Pubkey>
> - new_fee_receiver: Option<Pubkey>
> - new_market_program: Option<Pubkey>
> 
> **Logic:** Update each field if Some, emit ConfigUpdatedEvent
> 
> Update mod.rs to include admin module exports.

#### Task 2A.6: Assemble Oracle Program Entry Point
**Objective:** Wire up all Oracle instructions into the main program module with proper imports and the program macro.  
**Target Files:** `packages/contracts/programs/oracle/src/lib.rs`

**Prompt for Agentic IDE:**
> Assemble Oracle program in `packages/contracts/programs/oracle/src/lib.rs`:
> 
> **Structure:**
> ```rust
> use anchor_lang::prelude::*;
> 
> declare_id!("YOUR_ORACLE_PROGRAM_ID"); // Will be replaced with actual keypair
> 
> pub mod errors;
> pub mod instructions;
> pub mod state;
> 
> use errors::*;
> use instructions::*;
> use state::*;
> 
> #[program]
> pub mod oracle {
>     use super::*;
> 
>     pub fn initialize(ctx: Context<Initialize>, params: InitializeParams) -> Result<()> {
>         instructions::initialize::initialize(ctx, params)
>     }
> 
>     pub fn update_match(ctx: Context<UpdateMatch>, params: UpdateMatchParams) -> Result<()> {
>         instructions::update_match::update_match(ctx, params)
>     }
> 
>     pub fn set_paused(ctx: Context<SetPaused>, paused: bool) -> Result<()> {
>         instructions::admin::set_paused(ctx, paused)
>     }
> 
>     pub fn add_signer(ctx: Context<AddSigner>, new_signer: Pubkey) -> Result<()> {
>         instructions::admin::add_signer(ctx, new_signer)
>     }
> 
>     pub fn remove_signer(ctx: Context<RemoveSigner>, signer_to_remove: Pubkey) -> Result<()> {
>         instructions::admin::remove_signer(ctx, signer_to_remove)
>     }
> 
>     pub fn update_config(ctx: Context<UpdateConfig>, params: UpdateConfigParams) -> Result<()> {
>         instructions::admin::update_config(ctx, params)
>     }
> }
> ```
> Ensure all module paths are correct and all types are properly imported.
> Add any missing derives or use statements based on previous task implementations.

---

### Subphase 2B: Prediction Market Program

#### Task 2B.1: Define Market Program State Accounts
**Objective:** Create account structures for the Prediction Market program including global config, individual markets, and user positions.  
**Target Files:** `packages/contracts/programs/prediction_market/src/state/mod.rs`, `packages/contracts/programs/prediction_market/src/state/config.rs`, `packages/contracts/programs/prediction_market/src/state/market.rs`, `packages/contracts/programs/prediction_market/src/state/position.rs`

**Prompt for Agentic IDE:**
> Create Prediction Market state accounts in `packages/contracts/programs/prediction_market/src/state/`:
> 
> **mod.rs:** Export all state modules
> 
> **config.rs - MarketConfig account:**
> - `#[account]`
> - Fields:
>   - authority: Pubkey (admin)
>   - oracle_program: Pubkey (authorized oracle)
>   - treasury: Pubkey (receives platform fees)
>   - platform_fee_bps: u16 (basis points, e.g., 200 = 2%)
>   - min_bet_lamports: u64
>   - max_bet_lamports: u64
>   - is_paused: bool
>   - total_markets_created: u64
>   - total_volume_lamports: u128
>   - bump: u8
> - PDA seeds: `[b"market_config"]`
> - Calculate SPACE constant
> 
> **market.rs - Market account:**
> - `#[account]`
> - Fields:
>   - market_id: u64 (incrementing ID)
>   - match_id: [u8; 32] (links to oracle match data)
>   - market_type: MarketType (enum)
>   - creator: Pubkey
>   - description: [u8; 128] (padded string, e.g., "Goal in minutes 50-60?")
>   - start_minute: u8 (market active from this minute)
>   - end_minute: u8 (market locks at this minute)
>   - condition_value: u8 (e.g., for "score > X", this is X)
>   - status: MarketStatus (enum: Open=0, Locked=1, Resolved=2, Cancelled=3)
>   - outcome: Option<bool> (None until resolved, true=YES won, false=NO won)
>   - yes_pool_lamports: u64
>   - no_pool_lamports: u64
>   - total_yes_shares: u64 (for proportional payout calculation)
>   - total_no_shares: u64
>   - created_at: i64
>   - locks_at: i64 (timestamp when betting stops)
>   - resolved_at: Option<i64>
>   - resolution_slot: Option<u64>
>   - bump: u8
> - PDA seeds: `[b"market", market_id.to_le_bytes().as_ref()]`
> - Calculate SPACE
> - MarketType enum: GoalInPeriod=0, NextCorner=1, NextCard=2, TotalGoalsOver=3, TeamToScore=4
> - MarketStatus enum
> 
> **position.rs - UserPosition account:**
> - `#[account]`
> - Purpose: Track individual user's bet on a specific market
> - Fields:
>   - user: Pubkey
>   - market: Pubkey (market PDA)
>   - side: PositionSide (enum: Yes=0, No=1)
>   - amount_lamports: u64 (total bet amount)
>   - shares: u64 (proportional shares for payout)
>   - claimed: bool
>   - created_at: i64
>   - bump: u8
> - PDA seeds: `[b"position", market.as_ref(), user.as_ref()]`
> - Calculate SPACE
> - PositionSide enum

#### Task 2B.2: Define Market Program Error Codes
**Objective:** Create comprehensive error codes for the Prediction Market program.  
**Target Files:** `packages/contracts/programs/prediction_market/src/errors.rs`

**Prompt for Agentic IDE:**
> Create Prediction Market errors in `packages/contracts/programs/prediction_market/src/errors.rs`:
> 
> Define `MarketError` enum with `#[error_code]`:
> 
> **Authorization:**
> - Unauthorized: "Caller is not authorized"
> - InvalidOracleProgram: "Oracle program ID mismatch"
> 
> **Market lifecycle:**
> - MarketPaused: "Market program is paused"
> - MarketNotOpen: "Market is not open for betting"
> - MarketAlreadyLocked: "Market is already locked"
> - MarketNotLocked: "Market must be locked before resolution"
> - MarketAlreadyResolved: "Market has already been resolved"
> - MarketNotResolved: "Market is not yet resolved"
> - MarketCancelled: "Market has been cancelled"
> 
> **Betting:**
> - BetTooSmall: "Bet amount is below minimum"
> - BetTooLarge: "Bet amount exceeds maximum"
> - InsufficientFunds: "Insufficient SOL balance"
> - InvalidBetSide: "Invalid bet side specified"
> - BetWindowClosed: "Betting window has closed for this market"
> 
> **Position:**
> - PositionAlreadyExists: "User already has position on this side"
> - PositionNotFound: "User position not found"
> - AlreadyClaimed: "Winnings already claimed"
> - NothingToClaim: "No winnings to claim (lost bet or no position)"
> - CannotClaimBeforeResolution: "Cannot claim before market resolution"
> 
> **Resolution:**
> - InvalidOutcome: "Invalid outcome value"
> - MatchDataMismatch: "Match data does not correspond to this market"
> - ResolutionConditionNotMet: "Resolution condition not yet met"
> - MatchNotFinished: "Match must be finished or at end minute"
> 
> **Data:**
> - InvalidMarketType: "Invalid market type"
> - InvalidMinuteRange: "End minute must be greater than start minute"
> - DescriptionTooLong: "Market description exceeds maximum length"
> - Overflow: "Arithmetic overflow"

#### Task 2B.3: Create Market Program Initialization Instruction
**Objective:** Implement the initialize instruction for the Prediction Market program config.  
**Target Files:** `packages/contracts/programs/prediction_market/src/instructions/mod.rs`, `packages/contracts/programs/prediction_market/src/instructions/initialize.rs`

**Prompt for Agentic IDE:**
> Create market program initialize in `packages/contracts/programs/prediction_market/src/instructions/`:
> 
> **mod.rs:**
> - Declare initialize module (will add more)
> 
> **initialize.rs:**
> Define `Initialize` accounts:
> - authority: Signer<'info> (payer, becomes admin)
> - market_config: Account<'info, MarketConfig> with init, payer, space, seeds=[b"market_config"], bump
> - oracle_program: UncheckedAccount<'info> (the oracle program to trust)
> - treasury: UncheckedAccount<'info>
> - system_program: Program<'info, System>
> 
> Define `InitializeParams`:
> - platform_fee_bps: u16
> - min_bet_lamports: u64
> - max_bet_lamports: u64
> 
> **Validation constraints** (can use `#[instruction(params: InitializeParams)]` and `require!`):
> - platform_fee_bps <= 1000 (max 10%)
> - min_bet_lamports >= 1_000_000 (0.001 SOL minimum)
> - max_bet_lamports <= 100_000_000_000 (100 SOL max)
> - min_bet < max_bet
> 
> ```rust
> pub fn initialize(ctx: Context<Initialize>, params: InitializeParams) -> Result<()>
> ```
> 
> **Logic:**
> 1. Set all market_config fields from accounts and params
> 2. Set total_markets_created = 0
> 3. Set total_volume_lamports = 0
> 4. Set is_paused = false
> 5. Set bump
> 6. Emit MarketProgramInitializedEvent

#### Task 2B.4: Create Market Creation Instruction
**Objective:** Implement the instruction to create new micro-market instances tied to oracle match data.  
**Target Files:** `packages/contracts/programs/prediction_market/src/instructions/create_market.rs`

**Prompt for Agentic IDE:**
> Create market creation instruction in `packages/contracts/programs/prediction_market/src/instructions/create_market.rs`:
> 
> **Define CreateMarket accounts:**
> - creator: Signer<'info> (anyone can create markets, pays for PDA)
> - market_config: Account<'info, MarketConfig> mut (to increment counter)
> - market: Account<'info, Market> with init, payer=creator, space, seeds=[b"market", next_market_id.to_le_bytes()], bump
> - market_vault: SystemAccount<'info> (PDA that holds betting pool)
>   - seeds = [b"vault", market.key().as_ref()]
>   - Just derive address, don't init (SOL transfers directly)
> - match_data: Account<'info, MatchDataAccount> (from Oracle program, verify exists and is Live)
> - oracle_program: UncheckedAccount<'info> (verify = market_config.oracle_program)
> - system_program: Program<'info, System>
> 
> **Define CreateMarketParams:**
> - market_type: u8
> - description: Vec<u8> (max 128 bytes)
> - start_minute: u8
> - end_minute: u8
> - condition_value: u8 (interpretation depends on market_type)
> - locks_at: i64 (timestamp)
> 
> **Validation:**
> - market_config.is_paused == false
> - oracle_program.key() == market_config.oracle_program
> - match_data.status == Live or Scheduled
> - start_minute < end_minute
> - end_minute <= 120
> - description.len() <= 128
> - market_type in valid range 0-4
> - locks_at is in the future
> - start_minute >= match_data.current_minute (can't create market for past minutes)
> 
> ```rust
> pub fn create_market(ctx: Context<CreateMarket>, params: CreateMarketParams) -> Result<()>
> ```
> 
> **Logic:**
> 1. Get next market_id from market_config.total_markets_created
> 2. Increment market_config.total_markets_created
> 3. Initialize market account:
>    - market_id
>    - match_id from match_data
>    - Copy params to fields
>    - Pad description to [u8; 128]
>    - status = Open
>    - outcome = None
>    - yes_pool_lamports = 0
>    - no_pool_lamports = 0
>    - total_yes_shares = 0
>    - total_no_shares = 0
>    - created_at = Clock::get()?.unix_timestamp
>    - resolved_at = None
>    - bump
> 4. Emit MarketCreatedEvent:
>    - market_id
>    - match_id
>    - market_type
>    - start_minute
>    - end_minute
>    - creator
> 
> Update mod.rs to include create_market.

#### Task 2B.5: Create Place Bet Instruction
**Objective:** Implement the betting instruction allowing users to place YES/NO bets on open markets with proper share calculation.  
**Target Files:** `packages/contracts/programs/prediction_market/src/instructions/place_bet.rs`

**Prompt for Agentic IDE:**
> Create place_bet instruction in `packages/contracts/programs/prediction_market/src/instructions/place_bet.rs`:
> 
> **CRITICAL:** This handles SOL transfers and share calculations for fair payouts.
> 
> **Define PlaceBet accounts:**
> - bettor: Signer<'info> (user placing bet, pays SOL)
> - market_config: Account<'info, MarketConfig> (for min/max bet validation)
> - market: Account<'info, Market> mut (update pools)
> - position: Account<'info, UserPosition> with init_if_needed:
>   - payer = bettor
>   - space = UserPosition::SPACE
>   - seeds = [b"position", market.key().as_ref(), bettor.key().as_ref()]
>   - bump
> - market_vault: SystemAccount<'info> mut
>   - seeds = [b"vault", market.key().as_ref()] (receives SOL)
> - match_data: Account<'info, MatchDataAccount> (verify market still valid based on match state)
> - system_program: Program<'info, System>
> 
> **Define PlaceBetParams:**
> - side: u8 (0 = Yes, 1 = No)
> - amount_lamports: u64
> 
> **Validation:**
> - market_config.is_paused == false
> - market.status == Open (MarketNotOpen error)
> - Clock timestamp < market.locks_at (BetWindowClosed)
> - match_data.current_minute < market.end_minute (BetWindowClosed)
> - amount >= market_config.min_bet_lamports
> - amount <= market_config.max_bet_lamports
> - side == 0 or side == 1
> 
> **Share Calculation** (IMPORTANT for fair payouts):
> Shares represent proportional claim to the losing pool.
> Simple approach: shares = amount_lamports (1:1 mapping)
> This means early bettors and late bettors with same amount get same shares.
> Payout = (user_shares / total_winning_shares) * losing_pool
> 
> ```rust
> pub fn place_bet(ctx: Context<PlaceBet>, params: PlaceBetParams) -> Result<()>
> ```
> 
> **Logic:**
> 1. Transfer SOL from bettor to market_vault using system_program transfer CPI:
>    ```rust
>    let transfer_ix = anchor_lang::system_program::Transfer {
>        from: ctx.accounts.bettor.to_account_info(),
>        to: ctx.accounts.market_vault.to_account_info(),
>    };
>    let cpi_ctx = CpiContext::new(
>        ctx.accounts.system_program.to_account_info(),
>        transfer_ix,
>    );
>    anchor_lang::system_program::transfer(cpi_ctx, params.amount_lamports)?;
>    ```
> 2. Calculate shares (shares = amount_lamports for simplicity)
> 3. Update market pools:
>    - If side == Yes: market.yes_pool_lamports += amount, market.total_yes_shares += shares
>    - If side == No: market.no_pool_lamports += amount, market.total_no_shares += shares
> 4. Update market_config.total_volume_lamports += amount
> 5. Initialize or update position:
>    - If position was just created (check if shares == 0): Set user, market, side, amount_lamports, shares, claimed=false, created_at, bump
>    - If position exists: Verify position.side matches params.side (can't bet both sides with same position)
>    - If different side, return error PositionAlreadyExists
>    - Add to amount_lamports and shares
> 6. Emit BetPlacedEvent:
>    - market_id
>    - bettor
>    - side
>    - amount
>    - shares
>    - new_yes_pool
>    - new_no_pool
> 
> Update mod.rs.

#### Task 2B.6: Create Resolve Market Instruction
**Objective:** Implement market resolution using oracle match data to determine outcome and enable payouts.  
**Target Files:** `packages/contracts/programs/prediction_market/src/instructions/resolve_market.rs`

**Prompt for Agentic IDE:**
> Create resolve_market instruction in `packages/contracts/programs/prediction_market/src/instructions/resolve_market.rs`:
> 
> This instruction reads oracle match data and determines market outcome.
> 
> **Define ResolveMarket accounts:**
> - resolver: Signer<'info> (anyone can trigger resolution when conditions met)
> - market_config: Account<'info, MarketConfig>
> - market: Account<'info, Market> mut
> - match_data: Account<'info, MatchDataAccount> (from Oracle program)
> - oracle_program: UncheckedAccount<'info> (verify ownership)
> 
> **Validation:**
> - oracle_program.key() == market_config.oracle_program
> - match_data is owned by oracle_program
> - market.status == Open or Locked (not already resolved/cancelled)
> - market.match_id == match_data.match_id
> - Resolution condition is met (see below)
> 
> **Resolution Conditions by MarketType:**
> GoalInPeriod (type 0):
> - Condition: Was there a goal between start_minute and end_minute?
> - Check if match_data.last_event_type == Goal AND match_data.last_event_minute >= market.start_minute AND match_data.last_event_minute <= market.end_minute
> - OR: If match finished/past end_minute, check if total_goals changed during that window
> - SIMPLIFICATION: Resolve YES if (match_data.total_goals > 0 AND match is past end_minute)
> Actually need historical tracking. For MVP: Resolve based on current state.
> 
> **For MVP Resolution Logic:**
> Market can resolve when: match_data.current_minute >= market.end_minute OR match_data.status == Finished
> - GoalInPeriod: YES if match_data.last_event_type == Goal (simplified)
> - NextCorner: YES if match_data.total_corners > market.condition_value
> - NextCard: YES if match_data.total_cards > market.condition_value
> - TotalGoalsOver: YES if match_data.total_goals > market.condition_value
> - TeamToScore: YES if (condition_value == 1 && home_score > 0) || (condition_value == 2 && away_score > 0)
> 
> ```rust
> pub fn resolve_market(ctx: Context<ResolveMarket>) -> Result<()>
> ```
> 
> **Logic:**
> 1. Verify resolution conditions are met (current_minute >= end_minute OR match finished)
> 2. If not met, return ResolutionConditionNotMet error
> 3. Determine outcome based on market_type and match_data
> 4. Update market:
>    - status = Resolved
>    - outcome = Some(bool)
>    - resolved_at = Some(Clock timestamp)
>    - resolution_slot = Some(Clock slot)
> 5. Emit MarketResolvedEvent: market_id, outcome, yes_pool, no_pool, resolver
> 
> Update mod.rs.

#### Task 2B.7: Create Claim Winnings Instruction
**Objective:** Implement the instruction for winners to claim their proportional share of the losing pool.  
**Target Files:** `packages/contracts/programs/prediction_market/src/instructions/claim_winnings.rs`

**Prompt for Agentic IDE:**
> Create claim_winnings instruction in `packages/contracts/programs/prediction_market/src/instructions/claim_winnings.rs`:
> 
> **CRITICAL:** This handles SOL payouts from market vault to winners.
> 
> **Define ClaimWinnings accounts:**
> - claimer: Signer<'info> (user claiming, must match position.user)
> - market_config: Account<'info, MarketConfig> (for fee calculation)
> - market: Account<'info, Market> (verify resolved)
> - position: Account<'info, UserPosition> mut
>   - seeds = [b"position", market.key().as_ref(), claimer.key().as_ref()]
>   - has_one = user @ constraint error
> - market_vault: SystemAccount<'info> mut
>   - seeds = [b"vault", market.key().as_ref()]
> - treasury: SystemAccount<'info> mut (receives platform fee)
> - system_program: Program<'info, System>
> 
> **Validation:**
> - market.status == Resolved
> - market.outcome.is_some()
> - position.user == claimer.key()
> - position.claimed == false
> - User was on winning side: (market.outcome == Some(true) && position.side == Yes) OR (market.outcome == Some(false) && position.side == No)
> 
> **Payout Calculation:**
> ```rust
> let losing_pool = if market.outcome == Some(true) {
>     market.no_pool_lamports
> } else {
>     market.yes_pool_lamports
> };
> 
> let total_winning_shares = if market.outcome == Some(true) {
>     market.total_yes_shares
> } else {
>     market.total_no_shares
> };
> 
> // User's share of the losing pool
> let winnings_from_losers = (position.shares as u128)
>     .checked_mul(losing_pool as u128)
>     .unwrap()
>     .checked_div(total_winning_shares as u128)
>     .unwrap() as u64;
> 
> // Total payout = original bet + winnings
> let gross_payout = position.amount_lamports + winnings_from_losers;
> 
> // Platform fee on winnings only (not original stake)
> let fee = winnings_from_losers
>     .checked_mul(market_config.platform_fee_bps as u64)
>     .unwrap()
>     .checked_div(10000)
>     .unwrap();
> 
> let net_payout = gross_payout - fee;
> ```
> 
> ```rust
> pub fn claim_winnings(ctx: Context<ClaimWinnings>) -> Result<()>
> ```
> 
> **Logic:**
> 1. Verify user is winner (check position.side matches outcome)
> 2. If not winner, return NothingToClaim error
> 3. Calculate payout and fee as above
> 4. Transfer fee to treasury from vault (using vault PDA signer seeds)
> 5. Transfer net_payout to claimer from vault
> 
> For PDA signing (vault transfers):
> ```rust
> let market_key = ctx.accounts.market.key();
> let vault_seeds = &[
>     b"vault".as_ref(),
>     market_key.as_ref(),
>     &[vault_bump], // Need to store or derive this
> ];
> let signer_seeds = &[&vault_seeds[..]];
> // Transfer using invoke_signed or anchor helper
> ```
> 
> 6. Mark position.claimed = true
> 7. Emit WinningsClaimedEvent: market_id, claimer, gross_payout, fee, net_payout
> 
> Update mod.rs.

#### Task 2B.8: Create Market Admin Instructions
**Objective:** Implement admin functions: pause/unpause, cancel markets, update config.  
**Target Files:** `packages/contracts/programs/prediction_market/src/instructions/admin.rs`

**Prompt for Agentic IDE:**
> Create market admin instructions in `packages/contracts/programs/prediction_market/src/instructions/admin.rs`:
> 
> **SET PAUSED:**
> Define SetMarketPaused accounts:
> - authority: Signer<'info>
> - market_config: Account<'info, MarketConfig> mut, has_one = authority
> ```rust
> pub fn set_paused(ctx: Context<SetMarketPaused>, paused: bool) -> Result<()>
> ```
> 
> **CANCEL MARKET (refunds all bettors):**
> Define CancelMarket accounts:
> - authority: Signer<'info>
> - market_config: Account<'info, MarketConfig> has_one = authority
> - market: Account<'info, Market> mut
> ```rust
> pub fn cancel_market(ctx: Context<CancelMarket>) -> Result<()>
> ```
> - Only callable by authority
> - Set market.status = Cancelled
> - After cancellation, users can call claim_refund (separate instruction)
> - Emit MarketCancelledEvent
> 
> **CLAIM REFUND (for cancelled markets):**
> Define ClaimRefund accounts:
> - claimer: Signer<'info>
> - market: Account<'info, Market> (verify Cancelled status)
> - position: Account<'info, UserPosition> mut
> - market_vault: SystemAccount<'info> mut (seeds = vault + market)
> - system_program: Program<'info, System>
> ```rust
> pub fn claim_refund(ctx: Context<ClaimRefund>) -> Result<()>
> ```
> - Verify market.status == Cancelled
> - Verify position.claimed == false
> - Transfer position.amount_lamports back to claimer from vault
> - Set position.claimed = true
> - Emit RefundClaimedEvent
> 
> **UPDATE MARKET CONFIG:**
> Define UpdateMarketConfig accounts:
> - authority: Signer<'info>
> - market_config: Account<'info, MarketConfig> mut, has_one = authority
> ```rust
> pub fn update_market_config(ctx: Context<UpdateMarketConfig>, params: UpdateMarketConfigParams) -> Result<()>
> ```
> UpdateMarketConfigParams (all Option):
> - new_authority: Option
> - new_treasury: Option
> - new_platform_fee_bps: Option
> - new_min_bet: Option
> - new_max_bet: Option
> - Validate fee <= 1000 bps if provided.
> 
> Update mod.rs.

#### Task 2B.9: Assemble Prediction Market Program Entry Point
**Objective:** Wire up all Prediction Market instructions into the main program module.  
**Target Files:** `packages/contracts/programs/prediction_market/src/lib.rs`

**Prompt for Agentic IDE:**
> Assemble Prediction Market program in `packages/contracts/programs/prediction_market/src/lib.rs`:
> 
> ```rust
> use anchor_lang::prelude::*;
> declare_id!("YOUR_MARKET_PROGRAM_ID");
> 
> pub mod errors;
> pub mod instructions;
> pub mod state;
> 
> use errors::*;
> use instructions::*;
> use state::*;
> 
> #[program]
> pub mod prediction_market {
>     use super::*;
> 
>     pub fn initialize(ctx: Context<Initialize>, params: InitializeParams) -> Result<()> {
>         instructions::initialize::initialize(ctx, params)
>     }
> 
>     pub fn create_market(ctx: Context<CreateMarket>, params: CreateMarketParams) -> Result<()> {
>         instructions::create_market::create_market(ctx, params)
>     }
> 
>     pub fn place_bet(ctx: Context<PlaceBet>, params: PlaceBetParams) -> Result<()> {
>         instructions::place_bet::place_bet(ctx, params)
>     }
> 
>     pub fn resolve_market(ctx: Context<ResolveMarket>) -> Result<()> {
>         instructions::resolve_market::resolve_market(ctx)
>     }
> 
>     pub fn claim_winnings(ctx: Context<ClaimWinnings>) -> Result<()> {
>         instructions::claim_winnings::claim_winnings(ctx)
>     }
> 
>     pub fn set_paused(ctx: Context<SetMarketPaused>, paused: bool) -> Result<()> {
>         instructions::admin::set_paused(ctx, paused)
>     }
> 
>     pub fn cancel_market(ctx: Context<CancelMarket>) -> Result<()> {
>         instructions::admin::cancel_market(ctx)
>     }
> 
>     pub fn claim_refund(ctx: Context<ClaimRefund>) -> Result<()> {
>         instructions::admin::claim_refund(ctx)
>     }
> 
>     pub fn update_market_config(ctx: Context<UpdateMarketConfig>, params: UpdateMarketConfigParams) -> Result<()> {
>         instructions::admin::update_market_config(ctx, params)
>     }
> }
> ```
> Ensure all imports resolve correctly. Run `anchor build` to verify compilation.

#### Task 2B.10: Generate IDL and TypeScript Types
**Objective:** Build contracts and generate TypeScript types from the Anchor IDL for frontend/backend consumption.  
**Target Files:** `packages/contracts/target/idl/*.json`, `packages/shared/src/idl/oracle.ts`, `packages/shared/src/idl/prediction_market.ts`

**Prompt for Agentic IDE:**
> After anchor build succeeds, set up IDL type generation:
> 
> 1. Run `anchor build` in `packages/contracts/` to generate IDL files in `target/idl/`
> 2. Create `packages/shared/src/idl/` directory
> 3. Copy and export IDL JSON files:
>    - `packages/shared/src/idl/oracle.json` (copy from target/idl/oracle.json)
>    - `packages/shared/src/idl/prediction_market.json` (copy from target/idl/prediction_market.json)
> 
> **Create TypeScript type exports:**
> `packages/shared/src/idl/oracle.ts`:
> ```typescript
> import OracleIDL from './oracle.json';
> export { OracleIDL };
> export type OracleProgram = typeof OracleIDL;
> ```
> 
> `packages/shared/src/idl/prediction_market.ts`:
> ```typescript
> import PredictionMarketIDL from './prediction_market.json';
> export { PredictionMarketIDL };
> export type PredictionMarketProgram = typeof PredictionMarketIDL;
> ```
> 
> **Update packages/shared/src/index.ts to export:**
> ```typescript
> export * from './idl/oracle';
> export * from './idl/prediction_market';
> ```
> 
> Add `"resolveJsonModule": true` to `packages/shared/tsconfig.json`.
> 
> **Create packages/shared/src/program-ids.ts:**
> ```typescript
> import { PublicKey } from '@solana/web3.js';
> export const ORACLE_PROGRAM_ID = new PublicKey('YOUR_ORACLE_PROGRAM_ID');
> export const MARKET_PROGRAM_ID = new PublicKey('YOUR_MARKET_PROGRAM_ID');
> ```
> Export from `index.ts`.

---

## Phase 3: Backend Relayer & Oracle Ingestion Worker

### Task 3.1: Create TxLINE API Client
**Objective:** Implement a typed client for fetching match data from TxLINE's API with proper error handling.  
**Target Files:** `apps/relayer/src/services/txline-client.ts`, `apps/relayer/src/types/txline.ts`

**Prompt for Agentic IDE:**
> Create TxLINE API client in `apps/relayer/src/`:
> 
> **types/txline.ts** - Define expected TxLINE API response types:
> ```typescript
> export interface TxLineMatchEvent {
>   type: 'goal' | 'corner' | 'yellow_card' | 'red_card' | 'penalty';
>   minute: number;
>   team: 'home' | 'away';
>   player?: string;
> }
> export interface TxLineMatchData {
>   matchId: string;
>   homeTeam: string;
>   awayTeam: string;
>   homeScore: number;
>   awayScore: number;
>   currentMinute: number;
>   status: 'scheduled' | 'live' | 'half_time' | 'finished' | 'cancelled';
>   events: TxLineMatchEvent[];
>   timestamp: number; // Unix timestamp
> }
> export interface TxLineSignedPayload {
>   data: TxLineMatchData;
>   signature: string; // Base64 Ed25519 signature
>   publicKey: string; // Base64 public key
> }
> export interface TxLineApiResponse {
>   success: boolean;
>   matches: TxLineSignedPayload[];
>   error?: string;
> }
> ```
> 
> **services/txline-client.ts:**
> ```typescript
> import { z } from 'zod';
> import pino from 'pino';
> import type { TxLineApiResponse, TxLineSignedPayload } from '../types/txline';
> 
> // Zod schemas for validation
> const MatchEventSchema = z.object({...});
> const MatchDataSchema = z.object({...});
> const SignedPayloadSchema = z.object({...});
> const ApiResponseSchema = z.object({...});
> 
> export class TxLineClient {
>   private readonly baseUrl: string;
>   private readonly apiKey: string;
>   private readonly logger: pino.Logger;
> 
>   constructor(config: { baseUrl: string; apiKey: string; logger: pino.Logger }) {
>     // Initialize
>   }
> 
>   async getLiveMatches(): Promise<TxLineSignedPayload[]> {
>     // Fetch /v1/matches/live
>     // Validate with Zod
>     // Return parsed data
>     // Throw on error with proper logging
>   }
> 
>   async getMatch(matchId: string): Promise<TxLineSignedPayload | null> {
>     // Fetch /v1/matches/{matchId}
>     // Handle 404 gracefully
>   }
> }
> 
> export function createTxLineClient(logger: pino.Logger): TxLineClient {
>   return new TxLineClient({
>     baseUrl: process.env.TXLINE_API_URL!,
>     apiKey: process.env.TXLINE_API_KEY!,
>     logger,
>   });
> }
> ```
> Handle rate limiting, retries (3 attempts with exponential backoff), and timeouts (10s).

### Task 3.2: Create Signature Verification Utility
**Objective:** Implement Ed25519 signature verification for TxLINE signed payloads before submitting to the oracle.  
**Target Files:** `apps/relayer/src/utils/crypto.ts`

**Prompt for Agentic IDE:**
> Create signature verification utility in `apps/relayer/src/utils/crypto.ts`:
> 
> ```typescript
> import nacl from 'tweetnacl';
> import bs58 from 'bs58';
> 
> export interface SignatureVerificationResult {
>   valid: boolean;
>   error?: string;
> }
> 
> /**
>  * Verifies an Ed25519 signature from TxLINE
>  * >  * The message format that TxLINE signs:
>  * - Deterministic serialization of match data fields
>  * - Fields concatenated in specific order: matchId|homeTeam|awayTeam|homeScore|awayScore|currentMinute|status|timestamp
>  */
> export function verifyTxLineSignature(
>   data: {
>     matchId: string;
>     homeTeam: string;
>     awayTeam: string;
>     homeScore: number;
>     awayScore: number;
>     currentMinute: number;
>     status: string;
>     timestamp: number;
>   },
>   signatureBase64: string,
>   publicKeyBase64: string
> ): SignatureVerificationResult {
>   try {
>     // 1. Reconstruct the signed message
>     const message = buildSignedMessage(data);
>     const messageBytes = new TextEncoder().encode(message);
>     
>     // 2. Decode signature and public key from base64
>     const signature = Buffer.from(signatureBase64, 'base64');
>     const publicKey = Buffer.from(publicKeyBase64, 'base64');
>     
>     // 3. Verify lengths
>     if (signature.length !== 64) {
>       return { valid: false, error: 'Invalid signature length' };
>     }
>     if (publicKey.length !== 32) {
>       return { valid: false, error: 'Invalid public key length' };
>     }
>     
>     // 4. Verify signature using tweetnacl
>     const valid = nacl.sign.detached.verify(
>       messageBytes,
>       new Uint8Array(signature),
>       new Uint8Array(publicKey)
>     );
>     
>     return { valid };
>   } catch (error) {
>     return { valid: false, error: String(error) };
>   }
> }
> 
> function buildSignedMessage(data: {...}): string {
>   // Deterministic string format that TxLINE uses
>   return `${data.matchId}|${data.homeTeam}|${data.awayTeam}|${data.homeScore}|${data.awayScore}|${data.currentMinute}|${data.status}|${data.timestamp}`;
> }
> 
> /**
>  * Convert public key bytes to Solana Pubkey format
>  */
> export function publicKeyBytesToBase58(bytes: Uint8Array): string {
>   return bs58.encode(bytes);
> }
> 
> /**
>  * Convert match ID string to fixed 32-byte array (SHA256 hash)
>  */
> export function matchIdToBytes(matchId: string): Uint8Array {
>   const encoder = new TextEncoder();
>   const data = encoder.encode(matchId);
>   // Use SubtleCrypto for SHA256
>   // For sync operation, use a simple hash or import crypto
>   // Return first 32 bytes
> }
> ```
> Add comprehensive tests for signature verification in a separate test file.

### Task 3.3: Create Solana Transaction Builder
**Objective:** Implement the transaction builder that creates and sends oracle update transactions to Solana.  
**Target Files:** `apps/relayer/src/services/solana-client.ts`

**Prompt for Agentic IDE:**
> Create Solana client for oracle updates in `apps/relayer/src/services/solana-client.ts`:
> 
> ```typescript
> import {
>   Connection,
>   Keypair,
>   PublicKey,
>   Transaction,
>   TransactionInstruction,
>   Ed25519Program,
>   sendAndConfirmTransaction,
> } from '@solana/web3.js';
> import { Program, AnchorProvider, Wallet, BN } from '@coral-xyz/anchor';
> import type { Oracle } from '@world-cup-oracle/shared';
> import { OracleIDL, ORACLE_PROGRAM_ID } from '@world-cup-oracle/shared';
> import pino from 'pino';
> 
> export interface UpdateMatchParams {
>   matchId: Uint8Array; // 32 bytes
>   homeTeam: Uint8Array; // 32 bytes padded
>   awayTeam: Uint8Array; // 32 bytes padded
>   homeScore: number;
>   awayScore: number;
>   currentMinute: number;
>   status: number;
>   totalGoals: number;
>   totalCorners: number;
>   totalCards: number;
>   lastEventType: number;
>   lastEventMinute: number;
>   lastEventTeam: number;
>   timestamp: number;
>   signature: Uint8Array; // 64 bytes
>   signerPubkey: Uint8Array; // 32 bytes
> }
> 
> export class SolanaOracleClient {
>   private readonly connection: Connection;
>   private readonly program: Program<Oracle>;
>   private readonly relayerKeypair: Keypair;
>   private readonly logger: pino.Logger;
> 
>   constructor(config: {
>     rpcUrl: string;
>     relayerPrivateKey: string;
>     logger: pino.Logger;
>   }) {
>     this.connection = new Connection(config.rpcUrl, 'confirmed');
>     this.relayerKeypair = Keypair.fromSecretKey(
>       Buffer.from(config.relayerPrivateKey, 'base64')
>     );
>     
>     const wallet = new Wallet(this.relayerKeypair);
>     const provider = new AnchorProvider(this.connection, wallet, {
>       commitment: 'confirmed',
>     });
>     
>     this.program = new Program(OracleIDL as Oracle, ORACLE_PROGRAM_ID, provider);
>     this.logger = config.logger;
>   }
> 
>   /**
>    * Build and send oracle update transaction
>    * IMPORTANT: Must include Ed25519 signature verification instruction
>    */
>   async updateMatch(params: UpdateMatchParams): Promise<string> {
>     // 1. Derive PDAs
>     const [configPda] = PublicKey.findProgramAddressSync(
>       [Buffer.from('oracle_config')],
>       ORACLE_PROGRAM_ID
>     );
>     const [registryPda] = PublicKey.findProgramAddressSync(
>       [Buffer.from('signer_registry')],
>       ORACLE_PROGRAM_ID
>     );
>     const [matchPda] = PublicKey.findProgramAddressSync(
>       [Buffer.from('match'), params.matchId],
>       ORACLE_PROGRAM_ID
>     );
>     
>     // 2. Create Ed25519 signature verification instruction
>     // This must be the FIRST instruction in the transaction
>     const message = this.buildSignedMessage(params);
>     const ed25519Ix = Ed25519Program.createInstructionWithPublicKey({
>       publicKey: params.signerPubkey,
>       message: message,
>       signature: params.signature,
>     });
>     
>     // 3. Create oracle update instruction
>     const updateIx = await this.program.methods
>       .updateMatch({
>         matchId: Array.from(params.matchId),
>         homeTeam: Array.from(params.homeTeam),
>         awayTeam: Array.from(params.awayTeam),
>         homeScore: params.homeScore,
>         awayScore: params.awayScore,
>         currentMinute: params.currentMinute,
>         status: params.status,
>         totalGoals: params.totalGoals,
>         totalCorners: params.totalCorners,
>         totalCards: params.totalCards,
>         lastEventType: params.lastEventType,
>         lastEventMinute: params.lastEventMinute,
>         lastEventTeam: params.lastEventTeam,
>         timestamp: new BN(params.timestamp),
>         signature: Array.from(params.signature),
>         signerPubkey: Array.from(params.signerPubkey),
>       })
>       .accounts({
>         relayer: this.relayerKeypair.publicKey,
>         oracleConfig: configPda,
>         signerRegistry: registryPda,
>         matchAccount: matchPda,
>         ed25519Program: Ed25519Program.programId,
>         instructionsSysvar: SYSVAR_INSTRUCTIONS_PUBKEY,
>         systemProgram: SystemProgram.programId,
>       })
>       .instruction();
>       
>     // 4. Build and send transaction
>     const tx = new Transaction().add(ed25519Ix, updateIx);
>     
>     const signature = await sendAndConfirmTransaction(
>       this.connection,
>       tx,
>       [this.relayerKeypair],
>       { commitment: 'confirmed' }
>     );
>     
>     this.logger.info({ signature, matchId: params.matchId }, 'Match updated on-chain');
>     return signature;
>   }
> 
>   private buildSignedMessage(params: UpdateMatchParams): Uint8Array {
>     // Same format as TxLINE uses
>   }
> 
>   async getMatchData(matchId: Uint8Array): Promise<any | null> {
>     // Fetch match PDA account data
>   }
> }
> ```

### Task 3.4: Create Oracle Update Worker
**Objective:** Implement the main worker loop that polls TxLINE, validates data, and submits oracle updates.  
**Target Files:** `apps/relayer/src/workers/oracle-worker.ts`

**Prompt for Agentic IDE:**
> Create oracle worker in `apps/relayer/src/workers/oracle-worker.ts`:
> 
> ```typescript
> import cron from 'node-cron';
> import pino from 'pino';
> import { TxLineClient } from '../services/txline-client';
> import { SolanaOracleClient, UpdateMatchParams } from '../services/solana-client';
> import { verifyTxLineSignature, matchIdToBytes } from '../utils/crypto';
> import type { TxLineSignedPayload } from '../types/txline';
> 
> interface WorkerConfig {
>   pollIntervalSeconds: number;
>   maxConcurrentUpdates: number;
>   logger: pino.Logger;
> }
> 
> export class OracleUpdateWorker {
>   private readonly txLineClient: TxLineClient;
>   private readonly solanaClient: SolanaOracleClient;
>   private readonly logger: pino.Logger;
>   private readonly config: WorkerConfig;
>   private isRunning: boolean = false;
>   private lastProcessedTimestamps: Map<string, number> = new Map();
> 
>   constructor(
>     txLineClient: TxLineClient,
>     solanaClient: SolanaOracleClient,
>     config: WorkerConfig
>   ) {
>     this.txLineClient = txLineClient;
>     this.solanaClient = solanaClient;
>     this.config = config;
>     this.logger = config.logger.child({ worker: 'oracle-update' });
>   }
> 
>   async start(): Promise<void> {
>     this.isRunning = true;
>     this.logger.info('Oracle update worker starting');
>     
>     // Poll every N seconds
>     const cronExpression = `*/${this.config.pollIntervalSeconds} * * * * *`;
>     
>     cron.schedule(cronExpression, async () => {
>       if (!this.isRunning) return;
>       await this.processUpdates();
>     });
>     
>     // Initial run
>     await this.processUpdates();
>   }
> 
>   stop(): void {
>     this.isRunning = false;
>     this.logger.info('Oracle update worker stopped');
>   }
> 
>   private async processUpdates(): Promise<void> {
>     try {
>       // 1. Fetch live matches from TxLINE
>       const matches = await this.txLineClient.getLiveMatches();
>       this.logger.debug({ matchCount: matches.length }, 'Fetched live matches');
>       
>       // 2. Filter to matches that have new data
>       const updatesToProcess = matches.filter(m => this.hasNewData(m));
>       
>       if (updatesToProcess.length === 0) {
>         return;
>       }
>       
>       this.logger.info({ count: updatesToProcess.length }, 'Processing match updates');
>       
>       // 3. Process updates with concurrency limit
>       const chunks = this.chunkArray(updatesToProcess, this.config.maxConcurrentUpdates);
>       
>       for (const chunk of chunks) {
>         await Promise.allSettled(
>           chunk.map(match => this.processMatchUpdate(match))
>         );
>       }
>     } catch (error) {
>       this.logger.error({ error }, 'Error in oracle update cycle');
>     }
>   }
> 
>   private hasNewData(match: TxLineSignedPayload): boolean {
>     const lastTimestamp = this.lastProcessedTimestamps.get(match.data.matchId) ?? 0;
>     return match.data.timestamp > lastTimestamp;
>   }
> 
>   private async processMatchUpdate(payload: TxLineSignedPayload): Promise<void> {
>     const { data, signature, publicKey } = payload;
>     const childLogger = this.logger.child({ matchId: data.matchId });
>     
>     try {
>       // 1. Verify signature locally first (saves gas on bad data)
>       const verification = verifyTxLineSignature(
>         data,
>         signature,
>         publicKey
>       );
>       
>       if (!verification.valid) {
>         childLogger.warn({ error: verification.error }, 'Invalid signature, skipping');
>         return;
>       }
>       
>       // 2. Transform to on-chain format
>       const params = this.transformToUpdateParams(payload);
>       
>       // 3. Submit to Solana
>       const txSignature = await this.solanaClient.updateMatch(params);
>       
>       // 4. Update last processed timestamp
>       this.lastProcessedTimestamps.set(data.matchId, data.timestamp);
>       childLogger.info({ txSignature }, 'Match update submitted');
>     } catch (error) {
>       childLogger.error({ error }, 'Failed to process match update');
>     }
>   }
> 
>   private transformToUpdateParams(payload: TxLineSignedPayload): UpdateMatchParams {
>     const { data, signature, publicKey } = payload;
>     
>     // Convert string fields to fixed-size byte arrays
>     // Convert status string to enum number
>     // Convert event types to numbers
>     // etc.
>     
>     return {
>       matchId: matchIdToBytes(data.matchId),
>       homeTeam: this.padString(data.homeTeam, 32),
>       awayTeam: this.padString(data.awayTeam, 32),
>       homeScore: data.homeScore,
>       awayScore: data.awayScore,
>       currentMinute: data.currentMinute,
>       status: this.statusToNumber(data.status),
>       totalGoals: data.homeScore + data.awayScore,
>       totalCorners: this.countEvents(data.events, 'corner'),
>       totalCards: this.countEvents(data.events, 'yellow_card') + this.countEvents(data.events, 'red_card'),
>       lastEventType: this.getLastEventType(data.events),
>       lastEventMinute: this.getLastEventMinute(data.events),
>       lastEventTeam: this.getLastEventTeam(data.events),
>       timestamp: data.timestamp,
>       signature: Buffer.from(signature, 'base64'),
>       signerPubkey: Buffer.from(publicKey, 'base64'),
>     };
>   }
> 
>   // Helper methods for transformation
>   private padString(str: string, length: number): Uint8Array { /* ... */ }
>   private statusToNumber(status: string): number { /* ... */ }
>   private countEvents(events: any[], type: string): number { /* ... */ }
>   private getLastEventType(events: any[]): number { /* ... */ }
>   private getLastEventMinute(events: any[]): number { /* ... */ }
>   private getLastEventTeam(events: any[]): number { /* ... */ }
>   private chunkArray<T>(array: T[], size: number): T[][] { /* ... */ }
> }
> ```

### Task 3.5: Create Market Resolution Worker
**Objective:** Implement worker that monitors for markets ready to resolve and triggers resolution transactions.  
**Target Files:** `apps/relayer/src/workers/resolution-worker.ts`

**Prompt for Agentic IDE:**
> Create market resolution worker in `apps/relayer/src/workers/resolution-worker.ts`:
> 
> ```typescript
> import cron from 'node-cron';
> import pino from 'pino';
> import { Connection, PublicKey, Keypair } from '@solana/web3.js';
> import { Program, AnchorProvider, Wallet } from '@coral-xyz/anchor';
> import { PredictionMarketIDL, MARKET_PROGRAM_ID, ORACLE_PROGRAM_ID } from '@world-cup-oracle/shared';
> 
> interface MarketAccount {
>   marketId: bigint;
>   matchId: Uint8Array;
>   status: number; // 0=Open, 1=Locked, 2=Resolved, 3=Cancelled
>   endMinute: number;
>   locksAt: bigint;
> }
> 
> interface MatchDataAccount {
>   currentMinute: number;
>   status: number;
> }
> 
> export class MarketResolutionWorker {
>   private readonly connection: Connection;
>   private readonly marketProgram: Program;
>   private readonly resolverKeypair: Keypair;
>   private readonly logger: pino.Logger;
>   private isRunning: boolean = false;
> 
>   constructor(config: {
>     rpcUrl: string;
>     resolverPrivateKey: string;
>     logger: pino.Logger;
>   }) {
>     this.connection = new Connection(config.rpcUrl, 'confirmed');
>     this.resolverKeypair = Keypair.fromSecretKey(
>       Buffer.from(config.resolverPrivateKey, 'base64')
>     );
>     
>     const wallet = new Wallet(this.resolverKeypair);
>     const provider = new AnchorProvider(this.connection, wallet, {
>       commitment: 'confirmed',
>     });
>     
>     this.marketProgram = new Program(
>       PredictionMarketIDL,
>       MARKET_PROGRAM_ID,
>       provider
>     );
>     this.logger = config.logger.child({ worker: 'resolution' });
>   }
> 
>   async start(): Promise<void> {
>     this.isRunning = true;
>     this.logger.info('Market resolution worker starting');
>     
>     // Check every 10 seconds for markets to resolve
>     cron.schedule('*/10 * * * * *', async () => {
>       if (!this.isRunning) return;
>       await this.checkAndResolveMarkets();
>     });
>   }
> 
>   stop(): void {
>     this.isRunning = false;
>   }
> 
>   private async checkAndResolveMarkets(): Promise<void> {
>     try {
>       // 1. Fetch all open/locked markets
>       const markets = await this.fetchActiveMarkets();
>       
>       // 2. For each market, check if resolution conditions are met
>       for (const market of markets) {
>         await this.tryResolveMarket(market);
>       }
>     } catch (error) {
>       this.logger.error({ error }, 'Error in resolution cycle');
>     }
>   }
> 
>   private async fetchActiveMarkets(): Promise<MarketAccount[]> {
>     // Use getProgramAccounts with memcmp filter for status != Resolved/Cancelled
>     const accounts = await this.connection.getProgramAccounts(
>       MARKET_PROGRAM_ID,
>       {
>         filters: [
>           { dataSize: /* Market account size */ },
>           // Filter for Open (0) or Locked (1) status
>           // Status field offset in account data
>         ],
>       }
>     );
>     
>     return accounts.map(({ pubkey, account }) => {
>       // Deserialize using Anchor/Borsh
>       return this.deserializeMarket(account.data);
>     });
>   }
> 
>   private async tryResolveMarket(market: MarketAccount): Promise<void> {
>     const childLogger = this.logger.child({ marketId: market.marketId.toString() });
>     
>     try {
>       // 1. Fetch corresponding match data from oracle
>       const [matchPda] = PublicKey.findProgramAddressSync(
>         [Buffer.from('match'), Buffer.from(market.matchId)],
>         ORACLE_PROGRAM_ID
>       );
>       
>       const matchAccount = await this.connection.getAccountInfo(matchPda);
>       if (!matchAccount) {
>         childLogger.debug('Match data not found yet');
>         return;
>       }
>       
>       const matchData = this.deserializeMatchData(matchAccount.data);
>       
>       // 2. Check resolution conditions
>       const canResolve = 
>         matchData.currentMinute >= market.endMinute ||
>         matchData.status === 3; // Finished
>         
>       if (!canResolve) {
>         childLogger.debug({ 
>           currentMinute: matchData.currentMinute, 
>           endMinute: market.endMinute 
>         }, 'Resolution conditions not met');
>         return;
>       }
>       
>       // 3. Submit resolve transaction
>       childLogger.info('Resolving market');
>       
>       const [marketPda] = PublicKey.findProgramAddressSync(
>         [Buffer.from('market'), this.bigintToLeBytes(market.marketId)],
>         MARKET_PROGRAM_ID
>       );
>       
>       const [configPda] = PublicKey.findProgramAddressSync(
>         [Buffer.from('market_config')],
>         MARKET_PROGRAM_ID
>       );
>       
>       const tx = await this.marketProgram.methods
>         .resolveMarket()
>         .accounts({
>           resolver: this.resolverKeypair.publicKey,
>           marketConfig: configPda,
>           market: marketPda,
>           matchData: matchPda,
>           oracleProgram: ORACLE_PROGRAM_ID,
>         })
>         .rpc();
>         
>       childLogger.info({ tx }, 'Market resolved');
>     } catch (error) {
>       childLogger.error({ error }, 'Failed to resolve market');
>     }
>   }
> 
>   private deserializeMarket(data: Buffer): MarketAccount { /* Borsh deserialize */ }
>   private deserializeMatchData(data: Buffer): MatchDataAccount { /* Borsh deserialize */ }
>   private bigintToLeBytes(num: bigint): Buffer { /* Convert to LE bytes */ }
> }
> ```

### Task 3.6: Assemble Relayer Service Entry Point
**Objective:** Wire up all workers and services into the main relayer application entry point.  
**Target Files:** `apps/relayer/src/index.ts`, `apps/relayer/src/config.ts`

**Prompt for Agentic IDE:**
> Assemble relayer service in `apps/relayer/src/`:
> 
> **config.ts:**
> ```typescript
> import { z } from 'zod';
> import 'dotenv/config';
> 
> const ConfigSchema = z.object({
>   solana: z.object({
>     rpcUrl: z.string().url(),
>     privateKey: z.string().min(1),
>   }),
>   txline: z.object({
>     apiUrl: z.string().url(),
>     apiKey: z.string().min(1),
>   }),
>   programs: z.object({
>     oracleId: z.string().min(32),
>     marketId: z.string().min(32),
>   }),
>   workers: z.object({
>     oraclePollIntervalSeconds: z.number().default(5),
>     maxConcurrentUpdates: z.number().default(3),
>   }),
>   logging: z.object({
>     level: z.enum(['debug', 'info', 'warn', 'error']).default('info'),
>   }),
> });
> 
> export type Config = z.infer<typeof ConfigSchema>;
> 
> export function loadConfig(): Config {
>   return ConfigSchema.parse({
>     solana: {
>       rpcUrl: process.env.SOLANA_RPC_URL,
>       privateKey: process.env.SOLANA_PRIVATE_KEY,
>     },
>     txline: {
>       apiUrl: process.env.TXLINE_API_URL,
>       apiKey: process.env.TXLINE_API_KEY,
>     },
>     programs: {
>       oracleId: process.env.ORACLE_PROGRAM_ID,
>       marketId: process.env.MARKET_PROGRAM_ID,
>     },
>     workers: {
>       oraclePollIntervalSeconds: Number(process.env.ORACLE_POLL_INTERVAL) || 5,
>       maxConcurrentUpdates: Number(process.env.MAX_CONCURRENT_UPDATES) || 3,
>     },
>     logging: {
>       level: process.env.LOG_LEVEL || 'info',
>     },
>   });
> }
> ```
> 
> **index.ts:**
> ```typescript
> import pino from 'pino';
> import { loadConfig } from './config';
> import { createTxLineClient } from './services/txline-client';
> import { SolanaOracleClient } from './services/solana-client';
> import { OracleUpdateWorker } from './workers/oracle-worker';
> import { MarketResolutionWorker } from './workers/resolution-worker';
> 
> async function main() {
>   const config = loadConfig();
>   
>   const logger = pino({
>     level: config.logging.level,
>     transport: {
>       target: 'pino-pretty',
>       options: { colorize: true },
>     },
>   });
>   
>   logger.info('Starting World Cup Oracle Relayer');
>   
>   // Initialize clients
>   const txLineClient = createTxLineClient(logger);
>   const solanaClient = new SolanaOracleClient({
>     rpcUrl: config.solana.rpcUrl,
>     relayerPrivateKey: config.solana.privateKey,
>     logger,
>   });
>   
>   // Initialize workers
>   const oracleWorker = new OracleUpdateWorker(
>     txLineClient,
>     solanaClient,
>     {
>       pollIntervalSeconds: config.workers.oraclePollIntervalSeconds,
>       maxConcurrentUpdates: config.workers.maxConcurrentUpdates,
>       logger,
>     }
>   );
>   
>   const resolutionWorker = new MarketResolutionWorker({
>     rpcUrl: config.solana.rpcUrl,
>     resolverPrivateKey: config.solana.privateKey,
>     logger,
>   });
>   
>   // Start workers
>   await oracleWorker.start();
>   await resolutionWorker.start();
>   
>   // Graceful shutdown
>   const shutdown = async (signal: string) => {
>     logger.info({ signal }, 'Shutting down...');
>     oracleWorker.stop();
>     resolutionWorker.stop();
>     process.exit(0);
>   };
>   
>   process.on('SIGINT', () => shutdown('SIGINT'));
>   process.on('SIGTERM', () => shutdown('SIGTERM'));
>   
>   logger.info('Relayer running. Press Ctrl+C to stop.');
> }
> 
> main().catch(console.error);
> ```

---

## Phase 4: Frontend Core Setup & Startup-Grade UI Components

### Task 4.1: Create Design System Foundation
**Objective:** Set up the design system with theme variables, global styles, and utility functions for consistent UI.  
**Target Files:** `apps/web/app/globals.css`, `apps/web/lib/utils.ts`, `apps/web/tailwind.config.ts`

**Prompt for Agentic IDE:**
> Enhance design system in `apps/web/`:
> 
> **tailwind.config.ts** - Extend with:
> ```typescript
> const config: Config = {
>   darkMode: 'class',
>   content: [
>     './app/**/*.{ts,tsx}',
>     './components/**/*.{ts,tsx}',
>   ],
>   theme: {
>     extend: {
>       colors: {
>         // Dark theme palette
>         background: {
>           DEFAULT: '#0A0A0F',
>           secondary: '#12121A',
>           tertiary: '#1A1A2E',
>           elevated: '#242438',
>         },
>         primary: {
>           DEFAULT: '#3B82F6',
>           hover: '#2563EB',
>           muted: '#1E40AF',
>         },
>         accent: {
>           green: '#10B981',
>           'green-muted': '#065F46',
>           red: '#EF4444',
>           'red-muted': '#991B1B',
>           yellow: '#F59E0B',
>           purple: '#8B5CF6',
>         },
>         text: {
>           primary: '#F8FAFC',
>           secondary: '#94A3B8',
>           muted: '#64748B',
>         },
>         border: {
>           DEFAULT: '#2D2D44',
>           hover: '#3D3D5C',
>         },
>       },
>       fontFamily: {
>         sans: ['var(--font-inter)', 'system-ui', 'sans-serif'],
>         mono: ['var(--font-mono)', 'monospace'],
>       },
>       animation: {
>         'fade-in': 'fadeIn 0.3s ease-out',
>         'slide-up': 'slideUp 0.4s ease-out',
>         'slide-down': 'slideDown 0.3s ease-out',
>         'pulse-glow': 'pulseGlow 2s ease-in-out infinite',
>         'spin-slow': 'spin 3s linear infinite',
>         'bounce-subtle': 'bounceSubtle 1s ease-in-out infinite',
>       },
>       keyframes: {
>         fadeIn: {
>           '0%': { opacity: '0' },
>           '100%': { opacity: '1' },
>         },
>         slideUp: {
>           '0%': { opacity: '0', transform: 'translateY(10px)' },
>           '100%': { opacity: '1', transform: 'translateY(0)' },
>         },
>         slideDown: {
>           '0%': { opacity: '0', transform: 'translateY(-10px)' },
>           '100%': { opacity: '1', transform: 'translateY(0)' },
>         },
>         pulseGlow: {
>           '0%, 100%': { boxShadow: '0 0 0 0 rgba(59, 130, 246, 0.5)' },
>           '50%': { boxShadow: '0 0 20px 5px rgba(59, 130, 246, 0.3)' },
>         },
>         bounceSubtle: {
>           '0%, 100%': { transform: 'translateY(0)' },
>           '50%': { transform: 'translateY(-2px)' },
>         },
>       },
>       boxShadow: {
>         'glow-primary': '0 0 20px rgba(59, 130, 246, 0.3)',
>         'glow-green': '0 0 20px rgba(16, 185, 129, 0.3)',
>         'glow-red': '0 0 20px rgba(239, 68, 68, 0.3)',
>         'card': '0 4px 6px -1px rgba(0, 0, 0, 0.3), 0 2px 4px -2px rgba(0, 0, 0, 0.2)',
>         'card-hover': '0 10px 15px -3px rgba(0, 0, 0, 0.4), 0 4px 6px -4px rgba(0, 0, 0, 0.2)',
>       },
>       backgroundImage: {
>         'gradient-radial': 'radial-gradient(var(--tw-gradient-stops))',
>         'gradient-primary': 'linear-gradient(135deg, #3B82F6 0%, #8B5CF6 100%)',
>         'gradient-card': 'linear-gradient(180deg, #1A1A2E 0%, #12121A 100%)',
>       },
>     },
>   },
>   plugins: [require('tailwindcss-animate')],
> };
> ```
> 
> **globals.css:**
> ```css
> @tailwind base;
> @tailwind components;
> @tailwind utilities;
> 
> @layer base {
>   :root {
>     --font-inter: 'Inter', sans-serif;
>     --font-mono: 'JetBrains Mono', monospace;
>   }
>   * {
>     @apply border-border;
>   }
>   body {
>     @apply bg-background text-text-primary antialiased;
>     font-feature-settings: 'rlig' 1, 'calt' 1;
>   }
>   /* Custom scrollbar */
>   ::-webkit-scrollbar {
>     width: 8px;
>     height: 8px;
>   }
>   ::-webkit-scrollbar-track {
>     @apply bg-background-secondary;
>   }
>   ::-webkit-scrollbar-thumb {
>     @apply bg-border rounded-full hover:bg-border-hover;
>   }
> }
> 
> @layer utilities {
>   .glass {
>     @apply bg-background-secondary/80 backdrop-blur-md;
>   }
>   .glass-border {
>     @apply border border-border/50;
>   }
> }
> ```
> 
> **lib/utils.ts:**
> ```typescript
> import { type ClassValue, clsx } from 'clsx';
> import { twMerge } from 'tailwind-merge';
> 
> export function cn(...inputs: ClassValue[]) {
>   return twMerge(clsx(inputs));
> }
> 
> export function formatLamports(lamports: bigint | number): string {
>   const sol = Number(lamports) / 1_000_000_000;
>   return sol.toLocaleString('en-US', { 
>     minimumFractionDigits: 2, 
>     maximumFractionDigits: 4 
>   });
> }
> 
> export function formatPercentage(value: number): string {
>   return `${(value * 100).toFixed(1)}%`;
> }
> 
> export function shortenAddress(address: string, chars = 4): string {
>   return `${address.slice(0, chars)}...${address.slice(-chars)}`;
> }
> 
> export function formatTimeRemaining(seconds: number): string {
>   if (seconds <= 0) return 'Ended';
>   const mins = Math.floor(seconds / 60);
>   const secs = seconds % 60;
>   return `${mins}:${secs.toString().padStart(2, '0')}`;
> }
> ```

### Task 4.2: Create Core UI Components
**Objective:** Build reusable, animated UI components using shadcn/ui patterns with Framer Motion enhancements.  
**Target Files:** `apps/web/components/ui/button.tsx`, `apps/web/components/ui/card.tsx`, `apps/web/components/ui/badge.tsx`, `apps/web/components/ui/skeleton.tsx`, `apps/web/components/ui/progress.tsx`

**Prompt for Agentic IDE:**
> Create core UI components in `apps/web/components/ui/`:
> 
> **button.tsx - Enhanced button with variants:**
> ```tsx
> 'use client';
> import * as React from 'react';
> import { Slot } from '@radix-ui/react-slot';
> import { cva, type VariantProps } from 'class-variance-authority';
> import { motion, HTMLMotionProps } from 'framer-motion';
> import { cn } from '@/lib/utils';
> 
> const buttonVariants = cva(
>   'inline-flex items-center justify-center whitespace-nowrap rounded-xl text-sm font-semibold ring-offset-background transition-all duration-200 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-primary focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50',
>   {
>     variants: {
>       variant: {
>         default: 'bg-primary text-white hover:bg-primary-hover shadow-glow-primary/50 hover:shadow-glow-primary',
>         destructive: 'bg-accent-red text-white hover:bg-accent-red/90',
>         outline: 'border-2 border-border bg-transparent hover:border-primary hover:text-primary',
>         ghost: 'hover:bg-background-tertiary',
>         link: 'text-primary underline-offset-4 hover:underline',
>         success: 'bg-accent-green text-white hover:bg-accent-green/90 shadow-glow-green/50',
>         yes: 'bg-gradient-to-r from-accent-green to-emerald-400 text-white font-bold hover:shadow-glow-green',
>         no: 'bg-gradient-to-r from-accent-red to-rose-400 text-white font-bold hover:shadow-glow-red',
>       },
>       size: {
>         default: 'h-11 px-6 py-2',
>         sm: 'h-9 rounded-lg px-4',
>         lg: 'h-14 rounded-2xl px-8 text-base',
>         icon: 'h-10 w-10',
>       },
>     },
>     defaultVariants: {
>       variant: 'default',
>       size: 'default',
>     },
>   }
> );
> 
> export interface ButtonProps
>   extends React.ButtonHTMLAttributes<HTMLButtonElement>,
>     VariantProps<typeof buttonVariants> {
>   asChild?: boolean;
>   loading?: boolean;
> }
> 
> const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
>   ({ className, variant, size, asChild = false, loading, children, disabled, ...props }, ref) => {
>     const Comp = asChild ? Slot : 'button';
>     
>     return (
>       <Comp
>         className={cn(buttonVariants({ variant, size, className }))}
>         ref={ref}
>         disabled={disabled || loading}
>         {...props}
>       >
>         {loading ? (
>           <motion.div
>             className="h-4 w-4 border-2 border-current border-t-transparent rounded-full"
>             animate={{ rotate: 360 }}
>             transition={{ duration: 1, repeat: Infinity, ease: 'linear' }}
>           />
>         ) : (
>           children
>         )}
>       </Comp>
>     );
>   }
> );
> Button.displayName = 'Button';
> export { Button, buttonVariants };
> ```
> 
> **card.tsx - Glassmorphism card with animations:**
> ```tsx
> 'use client';
> import * as React from 'react';
> import { motion, HTMLMotionProps } from 'framer-motion';
> import { cn } from '@/lib/utils';
> 
> interface CardProps extends HTMLMotionProps<'div'> {
>   variant?: 'default' | 'elevated' | 'interactive';
> }
> 
> const Card = React.forwardRef<HTMLDivElement, CardProps>(
>   ({ className, variant = 'default', children, ...props }, ref) => {
>     const variants = {
>       default: 'bg-background-secondary border border-border',
>       elevated: 'bg-gradient-card border border-border shadow-card',
>       interactive: 'bg-background-secondary border border-border hover:border-border-hover hover:shadow-card-hover cursor-pointer',
>     };
> 
>     return (
>       <motion.div
>         ref={ref}
>         className={cn(
>           'rounded-2xl p-6',
>           variants[variant],
>           className
>         )}
>         initial={{ opacity: 0, y: 10 }}
>         animate={{ opacity: 1, y: 0 }}
>         transition={{ duration: 0.3 }}
>         whileHover={variant === 'interactive' ? { scale: 1.01 } : undefined}
>         {...props}
>       >
>         {children}
>       </motion.div>
>     );
>   }
> );
> Card.displayName = 'Card';
> 
> const CardHeader = React.forwardRef<HTMLDivElement, React.HTMLAttributes<HTMLDivElement>>(
>   ({ className, ...props }, ref) => (
>     <div ref={ref} className={cn('flex flex-col space-y-1.5 pb-4', className)} {...props} />
>   )
> );
> CardHeader.displayName = 'CardHeader';
> 
> const CardTitle = React.forwardRef<HTMLHeadingElement, React.HTMLAttributes<HTMLHeadingElement>>(
>   ({ className, ...props }, ref) => (
>     <h3 ref={ref} className={cn('text-xl font-bold text-text-primary', className)} {...props} />
>   )
> );
> CardTitle.displayName = 'CardTitle';
> 
> const CardDescription = React.forwardRef<HTMLParagraphElement, React.HTMLAttributes<HTMLParagraphElement>>(
>   ({ className, ...props }, ref) => (
>     <p ref={ref} className={cn('text-sm text-text-secondary', className)} {...props} />
>   )
> );
> CardDescription.displayName = 'CardDescription';
> 
> const CardContent = React.forwardRef<HTMLDivElement, React.HTMLAttributes<HTMLDivElement>>(
>   ({ className, ...props }, ref) => (
>     <div ref={ref} className={cn('', className)} {...props} />
>   )
> );
> CardContent.displayName = 'CardContent';
> 
> export { Card, CardHeader, CardTitle, CardDescription, CardContent };
> ```
> 
> **badge.tsx - Status badges with live indicators:**
> ```tsx
> 'use client';
> import { cva, type VariantProps } from 'class-variance-authority';
> import { motion } from 'framer-motion';
> import { cn } from '@/lib/utils';
> 
> const badgeVariants = cva(
>   'inline-flex items-center gap-1.5 rounded-full px-3 py-1 text-xs font-semibold',
>   {
>     variants: {
>       variant: {
>         default: 'bg-primary/20 text-primary',
>         success: 'bg-accent-green/20 text-accent-green',
>         danger: 'bg-accent-red/20 text-accent-red',
>         warning: 'bg-accent-yellow/20 text-accent-yellow',
>         live: 'bg-accent-red/20 text-accent-red',
>         locked: 'bg-text-muted/20 text-text-muted',
>         resolved: 'bg-accent-purple/20 text-accent-purple',
>       },
>     },
>     defaultVariants: {
>       variant: 'default',
>     },
>   }
> );
> 
> interface BadgeProps extends React.HTMLAttributes<HTMLSpanElement>, VariantProps<typeof badgeVariants> {
>   pulse?: boolean;
> }
> 
> export function Badge({ className, variant, pulse, children, ...props }: BadgeProps) {
>   return (
>     <span className={cn(badgeVariants({ variant }), className)} {...props}>
>       {pulse && (
>         <motion.span
>           className="h-2 w-2 rounded-full bg-current"
>           animate={{ opacity: [1, 0.5, 1] }}
>           transition={{ duration: 1.5, repeat: Infinity }}
>         />
>       )}
>       {children}
>     </span>
>   );
> }
> ```
> 
> Include `skeleton.tsx` and `progress.tsx` for loading states and progress bars with similar modern styling.

### Task 4.3: Create Match Display Components
**Objective:** Build components to display live match information including score, time, and status.  
**Target Files:** `apps/web/components/match/match-card.tsx`, `apps/web/components/match/match-header.tsx`, `apps/web/components/match/live-indicator.tsx`, `apps/web/components/match/score-display.tsx`

**Prompt for Agentic IDE:**
> Create match display components in `apps/web/components/match/`:
> 
> **match-card.tsx:**
> ```tsx
> 'use client';
> import { motion } from 'framer-motion';
> import { Card, CardContent } from '@/components/ui/card';
> import { Badge } from '@/components/ui/badge';
> import { MatchHeader } from './match-header';
> import { ScoreDisplay } from './score-display';
> import { LiveIndicator } from './live-indicator';
> import type { MatchData, MatchStatus } from '@world-cup-oracle/shared';
> 
> interface MatchCardProps {
>   match: MatchData;
>   marketsCount: number;
>   onClick?: () => void;
> }
> 
> export function MatchCard({ match, marketsCount, onClick }: MatchCardProps) {
>   const isLive = match.status === 'Live' || match.status === 'HalfTime';
>   
>   return (
>     <Card 
>       variant="interactive" 
>       onClick={onClick}
>       className="relative overflow-hidden"
>     >
>       {/* Gradient top border for live matches */}
>       {isLive && (
>         <motion.div 
>           className="absolute top-0 left-0 right-0 h-1 bg-gradient-to-r from-accent-green via-primary to-accent-purple"
>           animate={{ backgroundPosition: ['0% 50%', '100% 50%', '0% 50%'] }}
>           transition={{ duration: 3, repeat: Infinity }}
>           style={{ backgroundSize: '200% 200%' }}
>         />
>       )}
>       
>       <CardContent className="pt-6">
>         {/* Status Row */}
>         <div className="flex items-center justify-between mb-4">
>           {isLive ? (
>             <LiveIndicator minute={match.currentMinute} />
>           ) : (
>             <Badge variant={match.status === 'Finished' ? 'resolved' : 'default'}>
>               {match.status}
>             </Badge>
>           )}
>           <Badge variant="default">
>             {marketsCount} markets
>           </Badge>
>         </div>
>         
>         {/* Teams & Score */}
>         <ScoreDisplay
>           homeTeam={match.homeTeam}
>           awayTeam={match.awayTeam}
>           homeScore={match.homeScore}
>           awayScore={match.awayScore}
>           isLive={isLive}
>         />
>         
>         {/* Quick Stats */}
>         <div className="mt-4 pt-4 border-t border-border flex justify-around text-center">
>           <StatItem label="Corners" value={match.totalCorners || 0} />
>           <StatItem label="Cards" value={match.totalCards || 0} />
>           <StatItem label="Events" value={match.events?.length || 0} />
>         </div>
>       </CardContent>
>     </Card>
>   );
> }
> 
> function StatItem({ label, value }: { label: string; value: number }) {
>   return (
>     <div>
>       <p className="text-lg font-bold text-text-primary">{value}</p>
>       <p className="text-xs text-text-muted">{label}</p>
>     </div>
>   );
> }
> ```
> 
> **live-indicator.tsx:**
> ```tsx
> 'use client';
> import { motion } from 'framer-motion';
> 
> interface LiveIndicatorProps {
>   minute: number;
> }
> 
> export function LiveIndicator({ minute }: LiveIndicatorProps) {
>   return (
>     <div className="flex items-center gap-2">
>       <motion.div 
>         className="relative"
>         initial={{ scale: 0.8 }}
>         animate={{ scale: 1 }}
>       >
>         <motion.div
>           className="absolute inset-0 rounded-full bg-accent-red"
>           animate={{ scale: [1, 1.5, 1], opacity: [0.5, 0, 0.5] }}
>           transition={{ duration: 1.5, repeat: Infinity }}
>         />
>         <div className="relative h-3 w-3 rounded-full bg-accent-red" />
>       </motion.div>
>       <span className="text-sm font-semibold text-accent-red">
>         LIVE {minute}'
>       </span>
>     </div>
>   );
> }
> ```
> 
> **score-display.tsx:**
> ```tsx
> 'use client';
> import { motion, AnimatePresence } from 'framer-motion';
> 
> interface ScoreDisplayProps {
>   homeTeam: string;
>   awayTeam: string;
>   homeScore: number;
>   awayScore: number;
>   isLive: boolean;
> }
> 
> export function ScoreDisplay({ homeTeam, awayTeam, homeScore, awayScore, isLive }: ScoreDisplayProps) {
>   return (
>     <div className="flex items-center justify-between gap-4">
>       {/* Home Team */}
>       <div className="flex-1 text-right">
>         <p className="text-lg font-semibold text-text-primary truncate">
>           {homeTeam}
>         </p>
>       </div>
>       
>       {/* Score */}
>       <div className="flex items-center gap-3 px-4 py-2 bg-background-tertiary rounded-xl">
>         <AnimatedScore value={homeScore} isLive={isLive} />
>         <span className="text-text-muted text-xl font-light">:</span>
>         <AnimatedScore value={awayScore} isLive={isLive} />
>       </div>
>       
>       {/* Away Team */}
>       <div className="flex-1 text-left">
>         <p className="text-lg font-semibold text-text-primary truncate">
>           {awayTeam}
>         </p>
>       </div>
>     </div>
>   );
> }
> 
> function AnimatedScore({ value, isLive }: { value: number; isLive: boolean }) {
>   return (
>     <AnimatePresence mode="wait">
>       <motion.span
>         key={value}
>         className="text-3xl font-bold text-text-primary tabular-nums"
>         initial={isLive ? { scale: 1.5, color: '#10B981' } : false}
>         animate={{ scale: 1, color: '#F8FAFC' }}
>         transition={{ duration: 0.3 }}
>       >
>         {value}
>       </motion.span>
>     </AnimatePresence>
>   );
> }
> ```
> 
> **match-header.tsx:**
> // Large header version for match detail page
> // Include team logos/flags, full team names, current status, live minute with progress bar
> // Add background gradient based on match status

### Task 4.4: Create Market Betting Components
**Objective:** Build the core market card and betting interface components with real-time odds display.  
**Target Files:** `apps/web/components/market/market-card.tsx`, `apps/web/components/market/odds-display.tsx`, `apps/web/components/market/bet-slider.tsx`, `apps/web/components/market/market-timer.tsx`

**Prompt for Agentic IDE:**
> Create market betting components in `apps/web/components/market/`:
> 
> **market-card.tsx:**
> ```tsx
> 'use client';
> import { useState } from 'react';
> import { motion, AnimatePresence } from 'framer-motion';
> import { Card, CardContent } from '@/components/ui/card';
> import { Button } from '@/components/ui/button';
> import { Badge } from '@/components/ui/badge';
> import { OddsDisplay } from './odds-display';
> import { BetSlider } from './bet-slider';
> import { MarketTimer } from './market-timer';
> import { formatLamports } from '@/lib/utils';
> import type { MicroMarket } from '@world-cup-oracle/shared';
> 
> interface MarketCardProps {
>   market: MicroMarket;
>   onPlaceBet: (side: 'yes' | 'no', amount: number) => Promise<void>;
>   userBalance?: number;
> }
> 
> export function MarketCard({ market, onPlaceBet, userBalance }: MarketCardProps) {
>   const [selectedSide, setSelectedSide] = useState<'yes' | 'no' | null>(null);
>   const [betAmount, setBetAmount] = useState(0.1);
>   const [isLoading, setIsLoading] = useState(false);
>   
>   const totalPool = Number(market.yesPool) + Number(market.noPool);
>   const yesOdds = totalPool > 0 ? totalPool / Number(market.yesPool) : 2;
>   const noOdds = totalPool > 0 ? totalPool / Number(market.noPool) : 2;
>   const isOpen = market.status === 'Open';
>   
>   const handlePlaceBet = async () => {
>     if (!selectedSide) return;
>     setIsLoading(true);
>     try {
>       await onPlaceBet(selectedSide, betAmount);
>       setSelectedSide(null);
>     } finally {
>       setIsLoading(false);
>     }
>   };
>   
>   return (
>     <Card variant="elevated" className="overflow-hidden">
>       <CardContent className="pt-6">
>         {/* Header */}
>         <div className="flex items-start justify-between mb-4">
>           <div className="flex-1">
>             <p className="text-lg font-semibold text-text-primary mb-1">
>               {market.description}
>             </p>
>             <div className="flex items-center gap-2">
>               <Badge variant={isOpen ? 'live' : market.status === 'Resolved' ? 'resolved' : 'locked'} pulse={isOpen}>
>                 {market.status}
>               </Badge>
>               {isOpen && <MarketTimer locksAt={market.resolvesAt} />}
>             </div>
>           </div>
>         </div>
>         
>         {/* Pool Info */}
>         <div className="bg-background-tertiary rounded-xl p-4 mb-4">
>           <div className="flex justify-between text-sm mb-2">
>             <span className="text-text-secondary">Total Pool</span>
>             <span className="font-semibold text-text-primary">
>               {formatLamports(totalPool)} SOL
>             </span>
>           </div>
>           {/* Pool distribution bar */}
>           <div className="h-2 rounded-full bg-background overflow-hidden flex">
>             <motion.div 
>               className="bg-accent-green"
>               animate={{ width: `${(Number(market.yesPool) / totalPool) * 100}%` }}
>             />
>             <motion.div 
>               className="bg-accent-red"
>               animate={{ width: `${(Number(market.noPool) / totalPool) * 100}%` }}
>             />
>           </div>
>         </div>
>         
>         {/* Betting Buttons */}
>         {isOpen && (
>           <>
>             <div className="grid grid-cols-2 gap-4 mb-4">
>               <OddsDisplay
>                 label="YES"
>                 odds={yesOdds}
>                 pool={market.yesPool}
>                 isSelected={selectedSide === 'yes'}
>                 onClick={() => setSelectedSide(selectedSide === 'yes' ? null : 'yes')}
>                 variant="yes"
>               />
>               <OddsDisplay
>                 label="NO"
>                 odds={noOdds}
>                 pool={market.noPool}
>                 isSelected={selectedSide === 'no'}
>                 onClick={() => setSelectedSide(selectedSide === 'no' ? null : 'no')}
>                 variant="no"
>               />
>             </div>
>             
>             {/* Bet Amount Slider - Shown when side selected */}
>             <AnimatePresence>
>               {selectedSide && (
>                 <motion.div
>                   initial={{ height: 0, opacity: 0 }}
>                   animate={{ height: 'auto', opacity: 1 }}
>                   exit={{ height: 0, opacity: 0 }}
>                   className="overflow-hidden"
>                 >
>                   <BetSlider
>                     value={betAmount}
>                     onChange={setBetAmount}
>                     min={0.01}
>                     max={Math.min(10, userBalance ?? 10)}
>                     selectedSide={selectedSide}
>                   />
>                   <Button
>                     variant={selectedSide === 'yes' ? 'yes' : 'no'}
>                     className="w-full mt-4"
>                     size="lg"
>                     onClick={handlePlaceBet}
>                     loading={isLoading}
>                   >
>                     Place {betAmount.toFixed(2)} SOL on {selectedSide.toUpperCase()}
>                   </Button>
>                 </motion.div>
>               )}
>             </AnimatePresence>
>           </>
>         )}
>         
>         {/* Resolved State */}
>         {market.status === 'Resolved' && market.outcome !== undefined && (
>           <div className={`p-4 rounded-xl ${market.outcome ? 'bg-accent-green/20' : 'bg-accent-red/20'}`}>
>             <p className="text-center font-bold text-lg">
>               Outcome: {market.outcome ? 'YES ✓' : 'NO ✗'}
>             </p>
>           </div>
>         )}
>       </CardContent>
>     </Card>
>   );
> }
> ```
> 
> **odds-display.tsx:**
> ```tsx
> 'use client';
> import { motion } from 'framer-motion';
> import { cn, formatLamports } from '@/lib/utils';
> 
> interface OddsDisplayProps {
>   label: string;
>   odds: number;
>   pool: bigint;
>   isSelected: boolean;
>   onClick: () => void;
>   variant: 'yes' | 'no';
> }
> 
> export function OddsDisplay({ label, odds, pool, isSelected, onClick, variant }: OddsDisplayProps) {
>   const styles = {
>     yes: {
>       bg: 'bg-accent-green/10 hover:bg-accent-green/20',
>       border: 'border-accent-green',
>       text: 'text-accent-green',
>       selected: 'bg-accent-green/30 border-accent-green shadow-glow-green',
>     },
>     no: {
>       bg: 'bg-accent-red/10 hover:bg-accent-red/20', 
>       border: 'border-accent-red',
>       text: 'text-accent-red',
>       selected: 'bg-accent-red/30 border-accent-red shadow-glow-red',
>     },
>   };
>   
>   const style = styles[variant];
>   
>   return (
>     <motion.button
>       onClick={onClick}
>       className={cn(
>         'relative rounded-xl p-4 border-2 transition-all duration-200',
>         isSelected ? style.selected : `${style.bg} border-transparent`,
>       )}
>       whileTap={{ scale: 0.98 }}
>     >
>       <div className="text-center">
>         <p className={cn('text-2xl font-bold mb-1', style.text)}>
>           {label}
>         </p>
>         <p className="text-3xl font-black text-text-primary">
>           {odds.toFixed(2)}x
>         </p>
>         <p className="text-xs text-text-muted mt-1">
>           {formatLamports(pool)} SOL
>         </p>
>       </div>
>     </motion.button>
>   );
> }
> ```
> 
> **bet-slider.tsx:**
> ```tsx
> 'use client';
> import { motion } from 'framer-motion';
> 
> interface BetSliderProps {
>   value: number;
>   onChange: (value: number) => void;
>   min: number;
>   max: number;
>   selectedSide: 'yes' | 'no';
> }
> 
> export function BetSlider({ value, onChange, min, max, selectedSide }: BetSliderProps) {
>   const presets = [0.1, 0.5, 1, 5];
>   const color = selectedSide === 'yes' ? 'accent-green' : 'accent-red';
>   
>   return (
>     <div className="space-y-3 pt-4 border-t border-border">
>       {/* Preset buttons */}
>       <div className="flex gap-2">
>         {presets.map((preset) => (
>           <button
>             key={preset}
>             onClick={() => onChange(Math.min(preset, max))}
>             disabled={preset > max}
>             className={cn(
>               'flex-1 py-2 rounded-lg text-sm font-medium transition-all',
>               value === preset
>                 ? `bg-${color} text-white`
>                 : 'bg-background-tertiary text-text-secondary hover:text-text-primary',
>               preset > max && 'opacity-50 cursor-not-allowed'
>             )}
>           >
>             {preset} SOL
>           </button>
>         ))}
>       </div>
>       
>       {/* Slider */}
>       <div className="relative">
>         <input
>           type="range"
>           min={min}
>           max={max}
>           step={0.01}
>           value={value}
>           onChange={(e) => onChange(parseFloat(e.target.value))}
>           className="w-full h-2 rounded-full appearance-none cursor-pointer bg-background-tertiary accent-primary"
>         />
>         <div className="flex justify-between text-xs text-text-muted mt-1">
>           <span>{min} SOL</span>
>           <span className="font-semibold text-text-primary">{value.toFixed(2)} SOL</span>
>           <span>{max} SOL</span>
>         </div>
>       </div>
>       
>       {/* Potential winnings */}
>       <motion.div
>         className="text-center p-3 bg-background-tertiary rounded-xl"
>         key={value}
>         initial={{ scale: 0.95 }}
>         animate={{ scale: 1 }}
>       >
>         <p className="text-sm text-text-secondary">Potential Win</p>
>         <p className={`text-xl font-bold text-${color}`}>
>           +{(value * 0.95).toFixed(2)} SOL
>         </p>
>       </motion.div>
>     </div>
>   );
> }
> ```
> 
> **market-timer.tsx:**
> ```tsx
> 'use client';
> import { useState, useEffect } from 'react';
> import { motion } from 'framer-motion';
> import { Clock } from 'lucide-react';
> 
> interface MarketTimerProps {
>   locksAt: number; // Unix timestamp
> }
> 
> export function MarketTimer({ locksAt }: MarketTimerProps) {
>   const [timeLeft, setTimeLeft] = useState(0);
>   
>   useEffect(() => {
>     const update = () => {
>       const now = Math.floor(Date.now() / 1000);
>       setTimeLeft(Math.max(0, locksAt - now));
>     };
>     
>     update();
>     const interval = setInterval(update, 1000);
>     return () => clearInterval(interval);
>   }, [locksAt]);
>   
>   const minutes = Math.floor(timeLeft / 60);
>   const seconds = timeLeft % 60;
>   const isUrgent = timeLeft < 60;
>   
>   return (
>     <motion.div 
>       className={cn(
>         'flex items-center gap-1.5 text-sm font-medium',
>         isUrgent ? 'text-accent-red' : 'text-text-secondary'
>       )}
>       animate={isUrgent ? { scale: [1, 1.05, 1] } : undefined}
>       transition={{ duration: 0.5, repeat: isUrgent ? Infinity : 0 }}
>     >
>       <Clock className="h-4 w-4" />
>       <span className="tabular-nums">
>         {minutes}:{seconds.toString().padStart(2, '0')}
>       </span>
>     </motion.div>
>   );
> }
> ```

### Task 4.5: Create Layout Components
**Objective:** Build the app shell including navigation header, wallet connection button, and responsive layout.  
**Target Files:** `apps/web/components/layout/header.tsx`, `apps/web/components/layout/wallet-button.tsx`, `apps/web/components/layout/sidebar.tsx`, `apps/web/components/layout/footer.tsx`

**Prompt for Agentic IDE:**
> Create layout components in `apps/web/components/layout/`:
> 
> **header.tsx:**
> ```tsx
> 'use client';
> import Link from 'next/link';
> import { motion } from 'framer-motion';
> import { WalletButton } from './wallet-button';
> import { Zap, Trophy, BarChart3 } from 'lucide-react';
> 
> export function Header() {
>   return (
>     <header className="fixed top-0 left-0 right-0 z-50 glass glass-border">
>       <div className="container mx-auto px-4">
>         <div className="flex items-center justify-between h-16">
>           {/* Logo */}
>           <Link href="/" className="flex items-center gap-2">
>             <motion.div
>               className="relative"
>               whileHover={{ rotate: [0, -10, 10, 0] }}
>               transition={{ duration: 0.5 }}
>             >
>               <Zap className="h-8 w-8 text-primary fill-primary" />
>               <motion.div
>                 className="absolute inset-0 blur-md bg-primary/50"
>                 animate={{ opacity: [0.5, 0.8, 0.5] }}
>                 transition={{ duration: 2, repeat: Infinity }}
>               />
>             </motion.div>
>             <span className="text-xl font-bold bg-gradient-to-r from-primary to-accent-purple bg-clip-text text-transparent">
>               World Cup Oracle
>             </span>
>           </Link>
>           
>           {/* Navigation */}
>           <nav className="hidden md:flex items-center gap-1">
>             <NavLink href="/" icon={<Trophy className="h-4 w-4" />}>
>               Live Matches
>             </NavLink>
>             <NavLink href="/markets" icon={<BarChart3 className="h-4 w-4" />}>
>               All Markets
>             </NavLink>
>             <NavLink href="/portfolio">
>               My Bets
>             </NavLink>
>           </nav>
>           
>           {/* Wallet */}
>           <WalletButton />
>         </div>
>       </div>
>     </header>
>   );
> }
> 
> function NavLink({ 
>   href, 
>   icon, 
>   children 
> }: { 
>   href: string; 
>   icon?: React.ReactNode; 
>   children: React.ReactNode 
> }) {
>   return (
>     <Link
>       href={href}
>       className="flex items-center gap-2 px-4 py-2 rounded-lg text-text-secondary hover:text-text-primary hover:bg-background-tertiary transition-colors"
>     >
>       {icon}
>       <span className="font-medium">{children}</span>
>     </Link>
>   );
> }
> ```
> 
> **wallet-button.tsx:**
> ```tsx
> 'use client';
> import { useState } from 'react';
> import { useWallet } from '@solana/wallet-adapter-react';
> import { useWalletModal } from '@solana/wallet-adapter-react-ui';
> import { motion, AnimatePresence } from 'framer-motion';
> import { Button } from '@/components/ui/button';
> import { shortenAddress, formatLamports } from '@/lib/utils';
> import { Wallet, ChevronDown, LogOut, Copy, ExternalLink } from 'lucide-react';
> 
> export function WalletButton() {
>   const { publicKey, disconnect, connected } = useWallet();
>   const { setVisible } = useWalletModal();
>   const [isOpen, setIsOpen] = useState(false);
>   
>   // Would fetch from hook
>   const balance = 0; // Placeholder
>   
>   if (!connected) {
>     return (
>       <Button onClick={() => setVisible(true)} className="gap-2">
>         <Wallet className="h-4 w-4" />
>         Connect Wallet
>       </Button>
>     );
>   }
>   
>   return (
>     <div className="relative">
>       <Button
>         variant="outline"
>         onClick={() => setIsOpen(!isOpen)}
>         className="gap-2"
>       >
>         <div className="h-2 w-2 rounded-full bg-accent-green" />
>         <span className="font-mono">{shortenAddress(publicKey!.toBase58())}</span>
>         <ChevronDown className={cn('h-4 w-4 transition-transform', isOpen && 'rotate-180')} />
>       </Button>
>       
>       <AnimatePresence>
>         {isOpen && (
>           <motion.div
>             initial={{ opacity: 0, y: 10, scale: 0.95 }}
>             animate={{ opacity: 1, y: 0, scale: 1 }}
>             exit={{ opacity: 0, y: 10, scale: 0.95 }}
>             className="absolute right-0 top-full mt-2 w-64 rounded-xl bg-background-secondary border border-border shadow-xl overflow-hidden"
>           >
>             {/* Balance */}
>             <div className="p-4 border-b border-border">
>               <p className="text-sm text-text-muted mb-1">Balance</p>
>               <p className="text-2xl font-bold">{formatLamports(balance)} SOL</p>
>             </div>
>             
>             {/* Actions */}
>             <div className="p-2">
>               <DropdownItem 
>                 icon={<Copy className="h-4 w-4" />}
>                 onClick={() => navigator.clipboard.writeText(publicKey!.toBase58())}
>               >
>                 Copy Address
>               </DropdownItem>
>               <DropdownItem 
>                 icon={<ExternalLink className="h-4 w-4" />}
>                 onClick={() => window.open(`https://solscan.io/account/${publicKey!.toBase58()}`, '_blank')}
>               >
>                 View on Solscan
>               </DropdownItem>
>               <DropdownItem 
>                 icon={<LogOut className="h-4 w-4" />}
>                 onClick={() => { disconnect(); setIsOpen(false); }}
>                 variant="danger"
>               >
>                 Disconnect
>               </DropdownItem>
>             </div>
>           </motion.div>
>         )}
>       </AnimatePresence>
>     </div>
>   );
> }
> 
> function DropdownItem({ 
>   icon, 
>   children, 
>   onClick, 
>   variant 
> }: { 
>   icon: React.ReactNode; 
>   children: React.ReactNode; 
>   onClick: () => void;
>   variant?: 'danger';
> }) {
>   return (
>     <button
>       onClick={onClick}
>       className={cn(
>         'w-full flex items-center gap-3 px-3 py-2 rounded-lg text-sm transition-colors',
>         variant === 'danger'
>           ? 'text-accent-red hover:bg-accent-red/10'
>           : 'text-text-secondary hover:text-text-primary hover:bg-background-tertiary'
>       )}
>     >
>       {icon}
>       {children}
>     </button>
>   );
> }
> ```
> Add remaining layout components with consistent styling.

---

## Phase 5: Web3 Integration & Contract Interaction

### Task 5.1: Set Up Solana Providers
**Objective:** Configure Solana wallet adapter and Anchor provider context for the frontend application.  
**Target Files:** `apps/web/providers/solana-provider.tsx`, `apps/web/providers/index.tsx`, `apps/web/app/layout.tsx`

**Prompt for Agentic IDE:**
> Create Solana provider setup in `apps/web/providers/`:
> 
> **solana-provider.tsx:**
> ```tsx
> 'use client';
> import { useMemo } from 'react';
> import { WalletAdapterNetwork } from '@solana/wallet-adapter-base';
> import { ConnectionProvider, WalletProvider } from '@solana/wallet-adapter-react';
> import { WalletModalProvider } from '@solana/wallet-adapter-react-ui';
> import { PhantomWalletAdapter, SolflareWalletAdapter, TorusWalletAdapter } from '@solana/wallet-adapter-wallets';
> import { clusterApiUrl } from '@solana/web3.js';
> 
> // Import wallet adapter styles
> import '@solana/wallet-adapter-react-ui/styles.css';
> 
> interface SolanaProviderProps {
>   children: React.ReactNode;
> }
> 
> export function SolanaProvider({ children }: SolanaProviderProps) {
>   // Can be 'devnet', 'testnet', or 'mainnet-beta'
>   const network = WalletAdapterNetwork.Devnet;
>   
>   // You can also provide a custom RPC endpoint
>   const endpoint = useMemo(() => {
>     // Use custom RPC for better performance in production
>     if (process.env.NEXT_PUBLIC_SOLANA_RPC_URL) {
>       return process.env.NEXT_PUBLIC_SOLANA_RPC_URL;
>     }
>     return clusterApiUrl(network);
>   }, [network]);
>   
>   const wallets = useMemo(
>     () => [
>       new PhantomWalletAdapter(),
>       new SolflareWalletAdapter(),
>       new TorusWalletAdapter(),
>     ],
>     []
>   );
>   
>   return (
>     <ConnectionProvider endpoint={endpoint}>
>       <WalletProvider wallets={wallets} autoConnect>
>         <WalletModalProvider>
>           {children}
>         </WalletModalProvider>
>       </WalletProvider>
>     </ConnectionProvider>
>   );
> }
> ```
> 
> **index.tsx - Combined providers:**
> ```tsx
> 'use client';
> import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
> import { SolanaProvider } from './solana-provider';
> import { useState } from 'react';
> 
> interface ProvidersProps {
>   children: React.ReactNode;
> }
> 
> export function Providers({ children }: ProvidersProps) {
>   const [queryClient] = useState(() => new QueryClient({
>     defaultOptions: {
>       queries: {
>         staleTime: 1000 * 10, // 10 seconds
>         refetchInterval: 1000 * 5, // 5 seconds for live data
>       },
>     },
>   }));
>   
>   return (
>     <QueryClientProvider client={queryClient}>
>       <SolanaProvider>
>         {children}
>       </SolanaProvider>
>     </QueryClientProvider>
>   );
> }
> ```
> 
> **Update app/layout.tsx:**
> ```tsx
> import type { Metadata } from 'next';
> import { Inter, JetBrains_Mono } from 'next/font/google';
> import { Providers } from '@/providers';
> import { Header } from '@/components/layout/header';
> import './globals.css';
> 
> const inter = Inter({ subsets: ['latin'], variable: '--font-inter' });
> const mono = JetBrains_Mono({ subsets: ['latin'], variable: '--font-mono' });
> 
> export const metadata: Metadata = {
>   title: 'World Cup Oracle | Live Prediction Markets on Solana',
>   description: 'Bet on live World Cup micro-markets with instant settlements on Solana.',
> };
> 
> export default function RootLayout({
>   children,
> }: {
>   children: React.ReactNode;
> }) {
>   return (
>     <html lang="en" className="dark">
>       <body className={`${inter.variable} ${mono.variable} font-sans`}>
>         <Providers>
>           <Header />
>           <main className="pt-16 min-h-screen">
>             {children}
>           </main>
>         </Providers>
>       </body>
>     </html>
>   );
> }
> ```

### Task 5.2: Create Program Hooks
**Objective:** Build React hooks for interacting with the Oracle and Prediction Market programs using Anchor.  
**Target Files:** `apps/web/hooks/use-program.ts`, `apps/web/hooks/use-markets.ts`, `apps/web/hooks/use-matches.ts`, `apps/web/hooks/use-user-positions.ts`

**Prompt for Agentic IDE:**
> Create program interaction hooks in `apps/web/hooks/`:
> 
> **use-program.ts - Base Anchor program hook:**
> ```typescript
> import { useMemo } from 'react';
> import { useConnection, useWallet } from '@solana/wallet-adapter-react';
> import { AnchorProvider, Program, Idl } from '@coral-xyz/anchor';
> import { OracleIDL, PredictionMarketIDL, ORACLE_PROGRAM_ID, MARKET_PROGRAM_ID } from '@world-cup-oracle/shared';
> 
> export function useAnchorProvider() {
>   const { connection } = useConnection();
>   const wallet = useWallet();
>   
>   return useMemo(() => {
>     if (!wallet.publicKey) return null;
>     
>     return new AnchorProvider(
>       connection,
>       wallet as any,
>       { commitment: 'confirmed' }
>     );
>   }, [connection, wallet]);
> }
> 
> export function useOracleProgram() {
>   const provider = useAnchorProvider();
>   return useMemo(() => {
>     if (!provider) return null;
>     return new Program(OracleIDL as Idl, ORACLE_PROGRAM_ID, provider);
>   }, [provider]);
> }
> 
> export function useMarketProgram() {
>   const provider = useAnchorProvider();
>   return useMemo(() => {
>     if (!provider) return null;
>     return new Program(PredictionMarketIDL as Idl, MARKET_PROGRAM_ID, provider);
>   }, [provider]);
> }
> ```
> 
> **use-matches.ts - Fetch live match data:**
> ```typescript
> import { useQuery } from '@tanstack/react-query';
> import { useConnection } from '@solana/wallet-adapter-react';
> import { PublicKey } from '@solana/web3.js';
> import { ORACLE_PROGRAM_ID } from '@world-cup-oracle/shared';
> import type { MatchData } from '@world-cup-oracle/shared';
> 
> export function useMatches() {
>   const { connection } = useConnection();
>   
>   return useQuery({
>     queryKey: ['matches'],
>     queryFn: async (): Promise<MatchData[]> => {
>       // Fetch all match accounts from Oracle program
>       const accounts = await connection.getProgramAccounts(
>         ORACLE_PROGRAM_ID,
>         {
>           filters: [
>             { dataSize: /* MatchDataAccount size */ 200 }, // Adjust based on actual size
>           ],
>         }
>       );
>       
>       return accounts.map(({ pubkey, account }) => {
>         // Deserialize account data (Borsh)
>         // Map to MatchData type
>         return deserializeMatchData(account.data);
>       });
>     },
>     refetchInterval: 5000, // Refresh every 5 seconds for live data
>   });
> }
> 
> export function useMatch(matchId: string) {
>   const { connection } = useConnection();
>   
>   return useQuery({
>     queryKey: ['match', matchId],
>     queryFn: async () => {
>       const matchIdBytes = hashMatchId(matchId);
>       const [matchPda] = PublicKey.findProgramAddressSync(
>         [Buffer.from('match'), matchIdBytes],
>         ORACLE_PROGRAM_ID
>       );
>       
>       const account = await connection.getAccountInfo(matchPda);
>       if (!account) return null;
>       
>       return deserializeMatchData(account.data);
>     },
>     enabled: !!matchId,
>     refetchInterval: 3000,
>   });
> }
> 
> function deserializeMatchData(data: Buffer): MatchData {
>   // Implement Borsh deserialization
>   // Match the Rust struct layout
> }
> 
> function hashMatchId(id: string): Uint8Array {
>   // SHA256 hash truncated to 32 bytes
> }
> ```
> 
> **use-markets.ts - Fetch markets for a match:**
> ```typescript
> import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
> import { useConnection, useWallet } from '@solana/wallet-adapter-react';
> import { PublicKey, SystemProgram } from '@solana/web3.js';
> import { BN } from '@coral-xyz/anchor';
> import { useMarketProgram } from './use-program';
> import { MARKET_PROGRAM_ID } from '@world-cup-oracle/shared';
> import type { MicroMarket } from '@world-cup-oracle/shared';
> 
> export function useMarketsForMatch(matchId: string) {
>   const { connection } = useConnection();
>   
>   return useQuery({
>     queryKey: ['markets', matchId],
>     queryFn: async (): Promise<MicroMarket[]> => {
>       const matchIdBytes = hashMatchId(matchId);
>       
>       // Fetch all market accounts that reference this match
>       // Filter by matchId field offset in account data
>       const accounts = await connection.getProgramAccounts(
>         MARKET_PROGRAM_ID,
>         {
>           filters: [
>             { dataSize: /* Market account size */ },
>             {
>               memcmp: {
>                 offset: 8 + 8, // discriminator + market_id
>                 bytes: Buffer.from(matchIdBytes).toString('base64'),
>               },
>             },
>           ],
>         }
>       );
>       
>       return accounts.map(({ pubkey, account }) => 
>         deserializeMarket(pubkey, account.data)
>       );
>     },
>     enabled: !!matchId,
>     refetchInterval: 3000,
>   });
> }
> 
> export function usePlaceBet() {
>   const program = useMarketProgram();
>   const { publicKey } = useWallet();
>   const queryClient = useQueryClient();
>   
>   return useMutation({
>     mutationFn: async ({
>       marketId,
>       side,
>       amountSol,
>     }: {
>       marketId: bigint;
>       side: 'yes' | 'no';
>       amountSol: number;
>     }) => {
>       if (!program || !publicKey) throw new Error('Wallet not connected');
>       
>       const amountLamports = new BN(amountSol * 1_000_000_000);
>       const sideValue = side === 'yes' ? 0 : 1;
>       
>       // Derive PDAs
>       const marketIdBytes = bigintToLeBytes(marketId);
>       const [marketPda] = PublicKey.findProgramAddressSync(
>         [Buffer.from('market'), marketIdBytes],
>         MARKET_PROGRAM_ID
>       );
>       const [configPda] = PublicKey.findProgramAddressSync(
>         [Buffer.from('market_config')],
>         MARKET_PROGRAM_ID
>       );
>       const [positionPda] = PublicKey.findProgramAddressSync(
>         [Buffer.from('position'), marketPda.toBuffer(), publicKey.toBuffer()],
>         MARKET_PROGRAM_ID
>       );
>       const [vaultPda] = PublicKey.findProgramAddressSync(
>         [Buffer.from('vault'), marketPda.toBuffer()],
>         MARKET_PROGRAM_ID
>       );
>       
>       // Get match data PDA from market account
>       const marketAccount = await program.account.market.fetch(marketPda);
>       const [matchPda] = PublicKey.findProgramAddressSync(
>         [Buffer.from('match'), Buffer.from(marketAccount.matchId)],
>         ORACLE_PROGRAM_ID
>       );
>       
>       const tx = await program.methods
>         .placeBet({
>           side: sideValue,
>           amountLamports,
>         })
>         .accounts({
>           bettor: publicKey,
>           marketConfig: configPda,
>           market: marketPda,
>           position: positionPda,
>           marketVault: vaultPda,
>           matchData: matchPda,
>           systemProgram: SystemProgram.programId,
>         })
>         .rpc();
>         
>       return tx;
>     },
>     onSuccess: () => {
>       // Invalidate market queries to refetch updated pools
>       queryClient.invalidateQueries({ queryKey: ['markets'] });
>       queryClient.invalidateQueries({ queryKey: ['positions'] });
>     },
>   });
> }
> 
> function deserializeMarket(pubkey: PublicKey, data: Buffer): MicroMarket {
>   // Borsh deserialization matching Rust struct
> }
> 
> function bigintToLeBytes(num: bigint): Buffer {
>   const buf = Buffer.alloc(8);
>   buf.writeBigUInt64LE(num);
>   return buf;
> }
> ```
> 
> **use-user-positions.ts:**
> ```typescript
> import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
> import { useConnection, useWallet } from '@solana/wallet-adapter-react';
> import { PublicKey } from '@solana/web3.js';
> import { useMarketProgram } from './use-program';
> import { MARKET_PROGRAM_ID } from '@world-cup-oracle/shared';
> 
> interface UserPosition {
>   marketId: string;
>   side: 'yes' | 'no';
>   amountLamports: bigint;
>   shares: bigint;
>   claimed: boolean;
> }
> 
> export function useUserPositions() {
>   const { connection } = useConnection();
>   const { publicKey } = useWallet();
>   
>   return useQuery({
>     queryKey: ['positions', publicKey?.toBase58()],
>     queryFn: async (): Promise<UserPosition[]> => {
>       if (!publicKey) return [];
>       
>       // Fetch all position accounts owned by user
>       const accounts = await connection.getProgramAccounts(
>         MARKET_PROGRAM_ID,
>         {
>           filters: [
>             { dataSize: /* Position account size */ },
>             {
>               memcmp: {
>                 offset: 8, // After discriminator, user pubkey
>                 bytes: publicKey.toBase58(),
>               },
>             },
>           ],
>         }
>       );
>       
>       return accounts.map(({ pubkey, account }) =>
>         deserializePosition(account.data)
>       );
>     },
>     enabled: !!publicKey,
>     refetchInterval: 10000,
>   });
> }
> 
> export function useClaimWinnings() {
>   const program = useMarketProgram();
>   const { publicKey } = useWallet();
>   const queryClient = useQueryClient();
>   
>   return useMutation({
>     mutationFn: async (marketPda: PublicKey) => {
>       if (!program || !publicKey) throw new Error('Wallet not connected');
>       
>       // Derive all necessary PDAs
>       const [configPda] = PublicKey.findProgramAddressSync(
>         [Buffer.from('market_config')],
>         MARKET_PROGRAM_ID
>       );
>       const [positionPda] = PublicKey.findProgramAddressSync(
>         [Buffer.from('position'), marketPda.toBuffer(), publicKey.toBuffer()],
>         MARKET_PROGRAM_ID
>       );
>       const [vaultPda] = PublicKey.findProgramAddressSync(
>         [Buffer.from('vault'), marketPda.toBuffer()],
>         MARKET_PROGRAM_ID
>       );
>       
>       // Fetch config for treasury
>       const config = await program.account.marketConfig.fetch(configPda);
>       
>       const tx = await program.methods
>         .claimWinnings()
>         .accounts({
>           claimer: publicKey,
>           marketConfig: configPda,
>           market: marketPda,
>           position: positionPda,
>           marketVault: vaultPda,
>           treasury: config.treasury,
>           systemProgram: SystemProgram.programId,
>         })
>         .rpc();
>         
>       return tx;
>     },
>     onSuccess: () => {
>       queryClient.invalidateQueries({ queryKey: ['positions'] });
>     },
>   });
> }
> ```

### Task 5.3: Create Main Application Pages
**Objective:** Build the primary application pages: home (live matches), match detail with markets, and user portfolio.  
**Target Files:** `apps/web/app/page.tsx`, `apps/web/app/match/[id]/page.tsx`, `apps/web/app/portfolio/page.tsx`

**Prompt for Agentic IDE:**
> Create main application pages in `apps/web/app/`:
> 
> **page.tsx - Home page with live matches:**
> ```tsx
> 'use client';
> import { motion } from 'framer-motion';
> import { useRouter } from 'next/navigation';
> import { useMatches } from '@/hooks/use-matches';
> import { MatchCard } from '@/components/match/match-card';
> import { Skeleton } from '@/components/ui/skeleton';
> import { Badge } from '@/components/ui/badge';
> import { Zap, Trophy, TrendingUp } from 'lucide-react';
> 
> export default function HomePage() {
>   const router = useRouter();
>   const { data: matches, isLoading, error } = useMatches();
>   
>   const liveMatches = matches?.filter(m => m.status === 'Live' || m.status === 'HalfTime') ?? [];
>   const upcomingMatches = matches?.filter(m => m.status === 'Scheduled') ?? [];
>   const finishedMatches = matches?.filter(m => m.status === 'Finished') ?? [];
>   
>   return (
>     <div className="container mx-auto px-4 py-8">
>       {/* Hero Section */}
>       <motion.div 
>         className="text-center mb-12"
>         initial={{ opacity: 0, y: -20 }}
>         animate={{ opacity: 1, y: 0 }}
>       >
>         <h1 className="text-4xl md:text-5xl font-bold mb-4">
>           <span className="bg-gradient-to-r from-primary via-accent-purple to-accent-green bg-clip-text text-transparent">
>             World Cup Oracle
>           </span>
>         </h1>
>         <p className="text-xl text-text-secondary max-w-2xl mx-auto">
>           Bet on live micro-markets. Win in minutes, not hours.
>         </p>
>         
>         {/* Quick Stats */}
>         <div className="flex justify-center gap-8 mt-8">
>           <StatCard icon={<Zap />} value={liveMatches.length} label="Live Matches" />
>           <StatCard icon={<Trophy />} value="$52K" label="Total Volume" />
>           <StatCard icon={<TrendingUp />} value="~2min" label="Avg Settlement" />
>         </div>
>       </motion.div>
>       
>       {/* Live Matches Section */}
>       {liveMatches.length > 0 && (
>         <section className="mb-12">
>           <div className="flex items-center gap-3 mb-6">
>             <Badge variant="live" pulse className="text-sm">
>               LIVE NOW
>             </Badge>
>             <h2 className="text-2xl font-bold">{liveMatches.length} Matches In Play</h2>
>           </div>
>           
>           <div className="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
>             {liveMatches.map((match, index) => (
>               <motion.div
>                 key={match.matchId}
>                 initial={{ opacity: 0, y: 20 }}
>                 animate={{ opacity: 1, y: 0 }}
>                 transition={{ delay: index * 0.1 }}
>               >
>                 <MatchCard
>                   match={match}
>                   marketsCount={5} // Would come from separate query
>                   onClick={() => router.push(`/match/${match.matchId}`)}
>                 />
>               </motion.div>
>             ))}
>           </div>
>         </section>
>       )}
>       
>       {/* Loading State */}
>       {isLoading && (
>         <div className="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
>           {[1, 2, 3].map((i) => (
>             <Skeleton key={i} className="h-64 rounded-2xl" />
>           ))}
>         </div>
>       )}
>       
>       {/* Upcoming Matches */}
>       {upcomingMatches.length > 0 && (
>         <section className="mb-12">
>           <h2 className="text-2xl font-bold mb-6">Upcoming Matches</h2>
>           <div className="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
>             {upcomingMatches.map((match) => (
>               <MatchCard
>                 key={match.matchId}
>                 match={match}
>                 marketsCount={0}
>                 onClick={() => router.push(`/match/${match.matchId}`)}
>               />
>             ))}
>           </div>
>         </section>
>       )}
>       
>       {/* Empty State */}
>       {!isLoading && matches?.length === 0 && (
>         <motion.div 
>           className="text-center py-20"
>           initial={{ opacity: 0 }}
>           animate={{ opacity: 1 }}
>         >
>           <Trophy className="h-16 w-16 mx-auto text-text-muted mb-4" />
>           <h2 className="text-2xl font-bold mb-2">No Matches Right Now</h2>
>           <p className="text-text-secondary">
>             Check back soon for live World Cup action!
>           </p>
>         </motion.div>
>       )}
>     </div>
>   );
> }
> 
> function StatCard({ icon, value, label }: { icon: React.ReactNode; value: string | number; label: string }) {
>   return (
>     <div className="text-center">
>       <div className="inline-flex items-center justify-center h-12 w-12 rounded-xl bg-primary/20 text-primary mb-2">
>         {icon}
>       </div>
>       <p className="text-2xl font-bold text-text-primary">{value}</p>
>       <p className="text-sm text-text-muted">{label}</p>
>     </div>
>   );
> }
> ```
> 
> **match/[id]/page.tsx - Match detail with markets:**
> ```tsx
> 'use client';
> import { use } from 'react';
> import { motion } from 'framer-motion';
> import { useMatch } from '@/hooks/use-matches';
> import { useMarketsForMatch, usePlaceBet } from '@/hooks/use-markets';
> import { MatchHeader } from '@/components/match/match-header';
> import { MarketCard } from '@/components/market/market-card';
> import { Skeleton } from '@/components/ui/skeleton';
> import { ArrowLeft } from 'lucide-react';
> import Link from 'next/link';
> 
> interface PageProps {
>   params: Promise<{ id: string }>;
> }
> 
> export default function MatchPage({ params }: PageProps) {
>   const { id: matchId } = use(params);
>   const { data: match, isLoading: matchLoading } = useMatch(matchId);
>   const { data: markets, isLoading: marketsLoading } = useMarketsForMatch(matchId);
>   const placeBet = usePlaceBet();
>   
>   const handlePlaceBet = async (marketId: bigint, side: 'yes' | 'no', amount: number) => {
>     await placeBet.mutateAsync({ marketId, side, amountSol: amount });
>   };
>   
>   if (matchLoading) {
>     return (
>       <div className="container mx-auto px-4 py-8">
>         <Skeleton className="h-48 rounded-2xl mb-8" />
>         <div className="grid gap-6 md:grid-cols-2">
>           {[1, 2, 3, 4].map((i) => (
>             <Skeleton key={i} className="h-72 rounded-2xl" />
>           ))}
>         </div>
>       </div>
>     );
>   }
>   
>   if (!match) {
>     return (
>       <div className="container mx-auto px-4 py-8 text-center">
>         <h1 className="text-2xl font-bold">Match not found</h1>
>       </div>
>     );
>   }
>   
>   const openMarkets = markets?.filter(m => m.status === 'Open') ?? [];
>   const resolvedMarkets = markets?.filter(m => m.status === 'Resolved') ?? [];
>   
>   return (
>     <div className="container mx-auto px-4 py-8">
>       {/* Back Button */}
>       <Link 
>         href="/"
>         className="inline-flex items-center gap-2 text-text-secondary hover:text-text-primary mb-6"
>       >
>         <ArrowLeft className="h-4 w-4" />
>         Back to matches
>       </Link>
>       
>       {/* Match Header */}
>       <MatchHeader match={match} />
>       
>       {/* Markets Grid */}
>       <div className="mt-8">
>         {openMarkets.length > 0 && (
>           <section className="mb-8">
>             <h2 className="text-xl font-bold mb-4">
>               Open Markets ({openMarkets.length})
>             </h2>
>             <div className="grid gap-6 md:grid-cols-2">
>               {openMarkets.map((market, index) => (
>                 <motion.div
>                   key={market.marketId}
>                   initial={{ opacity: 0, y: 20 }}
>                   animate={{ opacity: 1, y: 0 }}
>                   transition={{ delay: index * 0.1 }}
>                 >
>                   <MarketCard
>                     market={market}
>                     onPlaceBet={(side, amount) => 
>                       handlePlaceBet(BigInt(market.marketId), side, amount)
>                     }
>                   />
>                 </motion.div>
>               ))}
>             </div>
>           </section>
>         )}
>         
>         {resolvedMarkets.length > 0 && (
>           <section>
>             <h2 className="text-xl font-bold mb-4 text-text-secondary">
>               Resolved Markets ({resolvedMarkets.length})
>             </h2>
>             <div className="grid gap-6 md:grid-cols-2 opacity-75">
>               {resolvedMarkets.map((market) => (
>                 <MarketCard
>                   key={market.marketId}
>                   market={market}
>                   onPlaceBet={() => Promise.resolve()}
>                 />
>               ))}
>             </div>
>           </section>
>         )}
>         
>         {markets?.length === 0 && (
>           <div className="text-center py-12 text-text-secondary">
>             <p>No markets available for this match yet.</p>
>             <p className="text-sm mt-2">Markets are created as the match progresses.</p>
>           </div>
>         )}
>       </div>
>     </div>
>   );
> }
> ```
> 
> **portfolio/page.tsx:**
> ```tsx
> 'use client';
> import { motion } from 'framer-motion';
> import { useWallet } from '@solana/wallet-adapter-react';
> import { useUserPositions, useClaimWinnings } from '@/hooks/use-user-positions';
> import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
> import { Button } from '@/components/ui/button';
> import { Badge } from '@/components/ui/badge';
> import { formatLamports } from '@/lib/utils';
> import { Wallet, TrendingUp, Clock, CheckCircle } from 'lucide-react';
> 
> export default function PortfolioPage() {
>   const { connected, publicKey } = useWallet();
>   const { data: positions, isLoading } = useUserPositions();
>   const claimWinnings = useClaimWinnings();
>   
>   if (!connected) {
>     return (
>       <div className="container mx-auto px-4 py-8">
>         <div className="text-center py-20">
>           <Wallet className="h-16 w-16 mx-auto text-text-muted mb-4" />
>           <h1 className="text-2xl font-bold mb-2">Connect Your Wallet</h1>
>           <p className="text-text-secondary">
>             Connect your Solana wallet to view your betting positions.
>           </p>
>         </div>
>       </div>
>     );
>   }
>   
>   const activePositions = positions?.filter(p => !p.claimed) ?? [];
>   const claimedPositions = positions?.filter(p => p.claimed) ?? [];
>   
>   const totalStaked = activePositions.reduce(
>     (acc, p) => acc + Number(p.amountLamports), 
>     0
>   );
>   
>   return (
>     <div className="container mx-auto px-4 py-8">
>       <h1 className="text-3xl font-bold mb-8">My Portfolio</h1>
>       
>       {/* Summary Cards */}
>       <div className="grid gap-6 md:grid-cols-3 mb-8">
>         <SummaryCard
>           icon={<TrendingUp />}
>           title="Active Bets"
>           value={activePositions.length}
>         />
>         <SummaryCard
>           icon={<Clock />}
>           title="Total Staked"
>           value={`${formatLamports(totalStaked)} SOL`}
>         />
>         <SummaryCard
>           icon={<CheckCircle />}
>           title="Positions Claimed"
>           value={claimedPositions.length}
>         />
>       </div>
>       
>       {/* Active Positions */}
>       {activePositions.length > 0 && (
>         <section className="mb-8">
>           <h2 className="text-xl font-bold mb-4">Active Positions</h2>
>           <div className="space-y-4">
>             {activePositions.map((position) => (
>               <PositionCard
>                 key={position.marketId}
>                 position={position}
>                 onClaim={() => claimWinnings.mutate(new PublicKey(position.marketId))}
>                 isClaiming={claimWinnings.isPending}
>               />
>             ))}
>           </div>
>         </section>
>       )}
>       
>       {/* Empty State */}
>       {!isLoading && positions?.length === 0 && (
>         <div className="text-center py-12">
>           <p className="text-text-secondary">
>             You haven't placed any bets yet. Find a match and start betting!
>           </p>
>         </div>
>       )}
>     </div>
>   );
> }
> 
> function SummaryCard({ icon, title, value }: { icon: React.ReactNode; title: string; value: string | number }) {
>   return (
>     <Card>
>       <CardContent className="pt-6">
>         <div className="flex items-center gap-4">
>           <div className="h-12 w-12 rounded-xl bg-primary/20 flex items-center justify-center text-primary">
>             {icon}
>           </div>
>           <div>
>             <p className="text-sm text-text-muted">{title}</p>
>             <p className="text-2xl font-bold">{value}</p>
>           </div>
>         </div>
>       </CardContent>
>     </Card>
>   );
> }
> 
> function PositionCard({ position, onClaim, isClaiming }: { 
>   position: UserPosition; 
>   onClaim: () => void;
>   isClaiming: boolean;
> }) {
>   // Would need to fetch market details to show full info
>   return (
>     <Card>
>       <CardContent className="pt-6">
>         <div className="flex items-center justify-between">
>           <div>
>             <p className="font-semibold">Market #{position.marketId.slice(0, 8)}...</p>
>             <div className="flex items-center gap-2 mt-1">
>               <Badge variant={position.side === 'yes' ? 'success' : 'danger'}>
>                 {position.side.toUpperCase()}
>               </Badge>
>               <span className="text-text-secondary">
>                 {formatLamports(position.amountLamports)} SOL
>               </span>
>             </div>
>           </div>
>           <Button 
>             variant="success" 
>             size="sm"
>             onClick={onClaim}
>             loading={isClaiming}
>             disabled={position.claimed}
>           >
>             {position.claimed ? 'Claimed' : 'Claim Winnings'}
>           </Button>
>         </div>
>       </CardContent>
>     </Card>
>   );
> }
> ```

---

## Phase 6: Testing, Edge Cases, and Deployment

### Task 6.1: Write Anchor Integration Tests
**Objective:** Create comprehensive integration tests for both Solana programs covering happy paths and edge cases.  
**Target Files:** `packages/contracts/tests/oracle.ts`, `packages/contracts/tests/prediction_market.ts`

**Prompt for Agentic IDE:**
> Create Anchor integration tests in `packages/contracts/tests/`:
> 
> **oracle.ts:**
> ```typescript
> import * as anchor from '@coral-xyz/anchor';
> import { Program } from '@coral-xyz/anchor';
> import { Oracle } from '../target/types/oracle';
> import { Keypair, PublicKey, SystemProgram, SYSVAR_INSTRUCTIONS_PUBKEY } from '@solana/web3.js';
> import { expect } from 'chai';
> import nacl from 'tweetnacl';
> 
> describe('Oracle Program', () => {
>   const provider = anchor.AnchorProvider.env();
>   anchor.setProvider(provider);
>   
>   const program = anchor.workspace.Oracle as Program<Oracle>;
>   const authority = provider.wallet;
>   
>   // Generate a TxLINE signing keypair for tests
>   const txlineKeypair = Keypair.generate();
>   
>   let configPda: PublicKey;
>   let registryPda: PublicKey;
>   
>   before(async () => {
>     [configPda] = PublicKey.findProgramAddressSync(
>       [Buffer.from('oracle_config')],
>       program.programId
>     );
>     
>     [registryPda] = PublicKey.findProgramAddressSync(
>       [Buffer.from('signer_registry')],
>       program.programId
>     );
>   });
> 
>   describe('initialize', () => {
>     it('initializes oracle config and signer registry', async () => {
>       const marketProgram = Keypair.generate().publicKey;
>       const feeReceiver = Keypair.generate().publicKey;
>       
>       await program.methods
>         .initialize({
>           initialSigner: txlineKeypair.publicKey,
>         })
>         .accounts({
>           authority: authority.publicKey,
>           oracleConfig: configPda,
>           signerRegistry: registryPda,
>           marketProgram,
>           feeReceiver,
>           systemProgram: SystemProgram.programId,
>         })
>         .rpc();
>         
>       const config = await program.account.oracleConfig.fetch(configPda);
>       expect(config.authority.toBase58()).to.equal(authority.publicKey.toBase58());
>       expect(config.isPaused).to.be.false;
>       expect(config.totalUpdates.toNumber()).to.equal(0);
>       
>       const registry = await program.account.signerRegistry.fetch(registryPda);
>       expect(registry.authorizedSigners.length).to.equal(1);
>       expect(registry.authorizedSigners[0].toBase58()).to.equal(txlineKeypair.publicKey.toBase58());
>     });
>     
>     it('fails if called twice', async () => {
>       // Attempt reinit should fail
>     });
>   });
> 
>   describe('update_match', () => {
>     const matchId = Buffer.alloc(32).fill(1);
>     let matchPda: PublicKey;
>     
>     before(() => {
>       [matchPda] = PublicKey.findProgramAddressSync(
>         [Buffer.from('match'), matchId],
>         program.programId
>       );
>     });
> 
>     it('creates match account with valid signature', async () => {
>       const timestamp = Math.floor(Date.now() / 1000);
>       
>       const params = {
>         matchId: Array.from(matchId),
>         homeTeam: Array.from(Buffer.from('Brazil'.padEnd(32, '\0'))),
>         awayTeam: Array.from(Buffer.from('Germany'.padEnd(32, '\0'))),
>         homeScore: 1,
>         awayScore: 0,
>         currentMinute: 45,
>         status: 1, // Live
>         totalGoals: 1,
>         totalCorners: 4,
>         totalCards: 2,
>         lastEventType: 1, // Goal
>         lastEventMinute: 42,
>         lastEventTeam: 1, // Home
>         timestamp: new anchor.BN(timestamp),
>         signature: Array(64).fill(0), // Will be replaced with real sig
>         signerPubkey: Array.from(txlineKeypair.publicKey.toBytes()),
>       };
>       
>       // Build message and sign it
>       const message = buildSignedMessage(params);
>       const signature = nacl.sign.detached(
>         Buffer.from(message),
>         txlineKeypair.secretKey
>       );
>       params.signature = Array.from(signature);
>       
>       // Need to prepend Ed25519 verify instruction
>       // ... test implementation
>       
>       const matchAccount = await program.account.matchDataAccount.fetch(matchPda);
>       expect(matchAccount.homeScore).to.equal(1);
>       expect(matchAccount.status).to.deep.equal({ live: {} });
>     });
>     
>     it('rejects invalid signature', async () => {
>       // Test with tampered data
>     });
>     
>     it('rejects unauthorized signer', async () => {
>       // Test with non-whitelisted signer
>     });
>     
>     it('rejects stale data', async () => {
>       // Test with older timestamp than existing
>     });
>   });
> 
>   describe('admin functions', () => {
>     it('pauses and unpauses oracle', async () => {
>       await program.methods
>         .setPaused(true)
>         .accounts({
>           authority: authority.publicKey,
>           oracleConfig: configPda,
>         })
>         .rpc();
>         
>       let config = await program.account.oracleConfig.fetch(configPda);
>       expect(config.isPaused).to.be.true;
>       
>       await program.methods
>         .setPaused(false)
>         .accounts({
>           authority: authority.publicKey,
>           oracleConfig: configPda,
>         })
>         .rpc();
>         
>       config = await program.account.oracleConfig.fetch(configPda);
>       expect(config.isPaused).to.be.false;
>     });
>     
>     it('adds and removes signers', async () => {
>       const newSigner = Keypair.generate().publicKey;
>       
>       await program.methods
>         .addSigner(newSigner)
>         .accounts({
>           authority: authority.publicKey,
>           oracleConfig: configPda,
>           signerRegistry: registryPda,
>         })
>         .rpc();
>         
>       let registry = await program.account.signerRegistry.fetch(registryPda);
>       expect(registry.authorizedSigners.length).to.equal(2);
>       
>       await program.methods
>         .removeSigner(newSigner)
>         .accounts({
>           authority: authority.publicKey,
>           oracleConfig: configPda,
>           signerRegistry: registryPda,
>         })
>         .rpc();
>         
>       registry = await program.account.signerRegistry.fetch(registryPda);
>       expect(registry.authorizedSigners.length).to.equal(1);
>     });
>   });
> });
> 
> function buildSignedMessage(params: any): string {
>   // Match TxLINE's signing format
>   return `${Buffer.from(params.matchId).toString('hex')}|...`;
> }
> ```
> 
> **prediction_market.ts:**
> ```typescript
> import * as anchor from '@coral-xyz/anchor';
> import { Program } from '@coral-xyz/anchor';
> import { PredictionMarket } from '../target/types/prediction_market';
> import { Keypair, PublicKey, SystemProgram, LAMPORTS_PER_SOL } from '@solana/web3.js';
> import { expect } from 'chai';
> 
> describe('Prediction Market Program', () => {
>   const provider = anchor.AnchorProvider.env();
>   anchor.setProvider(provider);
>   
>   const program = anchor.workspace.PredictionMarket as Program<PredictionMarket>;
>   const authority = provider.wallet;
>   
>   let configPda: PublicKey;
>   let marketPda: PublicKey;
>   let vaultPda: PublicKey;
> 
>   describe('initialize', () => {
>     it('initializes market config', async () => {
>       // ... implementation
>     });
>   });
> 
>   describe('create_market', () => {
>     it('creates a new market for an existing match', async () => {
>       // Setup: Ensure oracle match exists
>       // Create market
>       // Verify market state
>     });
>     
>     it('fails for non-existent match', async () => {
>       // Test error handling
>     });
>   });
> 
>   describe('place_bet', () => {
>     let bettor: Keypair;
>     let positionPda: PublicKey;
>     
>     beforeEach(async () => {
>       bettor = Keypair.generate();
>       // Airdrop SOL to bettor
>       await provider.connection.confirmTransaction(
>         await provider.connection.requestAirdrop(bettor.publicKey, 10 * LAMPORTS_PER_SOL)
>       );
>       
>       [positionPda] = PublicKey.findProgramAddressSync(
>         [Buffer.from('position'), marketPda.toBuffer(), bettor.publicKey.toBuffer()],
>         program.programId
>       );
>     });
> 
>     it('places YES bet correctly', async () => {
>       const betAmount = new anchor.BN(0.5 * LAMPORTS_PER_SOL);
>       
>       const marketBefore = await program.account.market.fetch(marketPda);
>       const yesPoolBefore = marketBefore.yesPoolLamports.toNumber();
>       
>       await program.methods
>         .placeBet({
>           side: 0, // YES
>           amountLamports: betAmount,
>         })
>         .accounts({
>           bettor: bettor.publicKey,
>           marketConfig: configPda,
>           market: marketPda,
>           position: positionPda,
>           marketVault: vaultPda,
>           matchData: matchPda, // From oracle
>           systemProgram: SystemProgram.programId,
>         })
>         .signers([bettor])
>         .rpc();
>         
>       const marketAfter = await program.account.market.fetch(marketPda);
>       expect(marketAfter.yesPoolLamports.toNumber()).to.equal(
>         yesPoolBefore + betAmount.toNumber()
>       );
>       
>       const position = await program.account.userPosition.fetch(positionPda);
>       expect(position.user.toBase58()).to.equal(bettor.publicKey.toBase58());
>       expect(position.side).to.deep.equal({ yes: {} });
>       expect(position.amountLamports.toNumber()).to.equal(betAmount.toNumber());
>     });
>     
>     it('fails when market is locked', async () => {
>       // Test betting on locked market
>     });
>     
>     it('fails when bet below minimum', async () => {
>       // Test minimum bet validation
>     });
>   });
> 
>   describe('resolve_market', () => {
>     it('resolves market with YES outcome', async () => {
>       // Setup: Create market, place bets, update oracle to resolution point
>       // Resolve market
>       // Verify outcome
>     });
>     
>     it('fails when resolution conditions not met', async () => {
>       // Test premature resolution attempt
>     });
>   });
> 
>   describe('claim_winnings', () => {
>     it('pays out winner correctly', async () => {
>       // Setup: Resolved market with known outcome
>       // Winner claims
>       // Verify SOL transfer including fee deduction
>     });
>     
>     it('fails for losing position', async () => {
>       // Test loser cannot claim
>     });
>     
>     it('fails for already claimed position', async () => {
>       // Test double-claim prevention
>     });
>   });
> 
>   describe('edge cases', () => {
>     it('handles market with only YES bets', async () => {
>       // All bets on winning side - users get original stake back
>     });
>     
>     it('handles market cancellation and refunds', async () => {
>       // Cancel market, all users can claim refund
>     });
>     
>     it('handles concurrent bets correctly', async () => {
>       // Multiple users betting simultaneously
>     });
>   });
> });
> ```

### Task 6.2: Create Frontend Unit Tests
**Objective:** Set up frontend testing with Vitest and write tests for hooks and utility functions.  
**Target Files:** `apps/web/vitest.config.ts`, `apps/web/__tests__/hooks/use-markets.test.ts`, `apps/web/__tests__/utils.test.ts`

**Prompt for Agentic IDE:**
> Set up frontend testing in `apps/web/`:
> 
> **vitest.config.ts:**
> ```typescript
> import { defineConfig } from 'vitest/config';
> import react from '@vitejs/plugin-react';
> import path from 'path';
> 
> export default defineConfig({
>   plugins: [react()],
>   test: {
>     environment: 'jsdom',
>     globals: true,
>     setupFiles: ['./__tests__/setup.ts'],
>     include: ['__tests__/**/*.test.{ts,tsx}'],
>     coverage: {
>       provider: 'v8',
>       reporter: ['text', 'json', 'html'],
>     },
>   },
>   resolve: {
>     alias: {
>       '@': path.resolve(__dirname, './'),
>     },
>   },
> });
> ```
> 
> **tests/setup.ts:**
> ```typescript
> import '@testing-library/jest-dom';
> import { vi } from 'vitest';
> 
> // Mock Solana wallet adapter
> vi.mock('@solana/wallet-adapter-react', () => ({
>   useWallet: () => ({
>     publicKey: null,
>     connected: false,
>     disconnect: vi.fn(),
>   }),
>   useConnection: () => ({
>     connection: {
>       getAccountInfo: vi.fn(),
>       getProgramAccounts: vi.fn(),
>     },
>   }),
> }));
> ```
> 
> **tests/utils.test.ts:**
> ```typescript
> import { describe, it, expect } from 'vitest';
> import { 
>   formatLamports, 
>   shortenAddress, 
>   formatTimeRemaining,
>   formatPercentage 
> } from '@/lib/utils';
> 
> describe('formatLamports', () => {
>   it('converts lamports to SOL with correct decimals', () => {
>     expect(formatLamports(1_000_000_000)).toBe('1.00');
>     expect(formatLamports(1_500_000_000)).toBe('1.50');
>     expect(formatLamports(123_456_789)).toBe('0.1235');
>   });
>   
>   it('handles bigint input', () => {
>     expect(formatLamports(BigInt(5_000_000_000))).toBe('5.00');
>   });
>   
>   it('handles zero', () => {
>     expect(formatLamports(0)).toBe('0.00');
>   });
> });
> 
> describe('shortenAddress', () => {
>   it('shortens address with default chars', () => {
>     const address = 'EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v';
>     expect(shortenAddress(address)).toBe('EPjF...Dt1v');
>   });
>   
>   it('respects custom char count', () => {
>     const address = 'EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v';
>     expect(shortenAddress(address, 6)).toBe('EPjFWd...TDt1v');
>   });
> });
> 
> describe('formatTimeRemaining', () => {
>   it('formats minutes and seconds', () => {
>     expect(formatTimeRemaining(125)).toBe('2:05');
>     expect(formatTimeRemaining(60)).toBe('1:00');
>     expect(formatTimeRemaining(45)).toBe('0:45');
>   });
>   
>   it('shows Ended for zero or negative', () => {
>     expect(formatTimeRemaining(0)).toBe('Ended');
>     expect(formatTimeRemaining(-10)).toBe('Ended');
>   });
> });
> ```
> 
> **tests/hooks/use-markets.test.ts:**
> ```typescript
> import { describe, it, expect, vi, beforeEach } from 'vitest';
> import { renderHook, waitFor } from '@testing-library/react';
> import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
> import { useMarketsForMatch } from '@/hooks/use-markets';
> 
> // Mock connection
> const mockGetProgramAccounts = vi.fn();
> 
> vi.mock('@solana/wallet-adapter-react', () => ({
>   useConnection: () => ({
>     connection: {
>       getProgramAccounts: mockGetProgramAccounts,
>     },
>   }),
>   useWallet: () => ({
>     publicKey: null,
>     connected: false,
>   }),
> }));
> 
> describe('useMarketsForMatch', () => {
>   let queryClient: QueryClient;
>   
>   beforeEach(() => {
>     queryClient = new QueryClient({
>       defaultOptions: {
>         queries: { retry: false },
>       },
>     });
>     mockGetProgramAccounts.mockReset();
>   });
>   
>   const wrapper = ({ children }: { children: React.ReactNode }) => (
>     <QueryClientProvider client={queryClient}>
>       {children}
>     </QueryClientProvider>
>   );
>   
>   it('fetches markets for a match', async () => {
>     mockGetProgramAccounts.mockResolvedValue([
>       {
>         pubkey: { toBase58: () => 'market1' },
>         account: {
>           data: Buffer.alloc(200), // Mock market data
>         },
>       },
>     ]);
>     
>     const { result } = renderHook(
>       () => useMarketsForMatch('match123'),
>       { wrapper }
>     );
>     
>     await waitFor(() => {
>       expect(result.current.isSuccess).toBe(true);
>     });
>     
>     expect(result.current.data).toHaveLength(1);
>   });
>   
>   it('returns empty array when no markets exist', async () => {
>     mockGetProgramAccounts.mockResolvedValue([]);
>     
>     const { result } = renderHook(
>       () => useMarketsForMatch('match123'),
>       { wrapper }
>     );
>     
>     await waitFor(() => {
>       expect(result.current.isSuccess).toBe(true);
>     });
>     
>     expect(result.current.data).toEqual([]);
>   });
> });
> ```
> Add package.json script: `"test": "vitest"`

### Task 6.3: Create Deployment Scripts and Configuration
**Objective:** Set up deployment configuration for Solana programs to devnet/mainnet and frontend to Vercel.  
**Target Files:** `packages/contracts/deploy/deploy.ts`, `packages/contracts/Anchor.toml`, `apps/web/vercel.json`, `.github/workflows/deploy.yml`

**Prompt for Agentic IDE:**
> Create deployment configuration:
> 
> **packages/contracts/deploy/deploy.ts:**
> ```typescript
> import * as anchor from '@coral-xyz/anchor';
> import { Program } from '@coral-xyz/anchor';
> import { Keypair, PublicKey, SystemProgram } from '@solana/web3.js';
> import fs from 'fs';
> import path from 'path';
> 
> async function main() {
>   // Load environment
>   const cluster = process.env.SOLANA_CLUSTER || 'devnet';
>   console.log(`Deploying to ${cluster}...`);
>   
>   // Configure provider
>   const provider = anchor.AnchorProvider.env();
>   anchor.setProvider(provider);
>   
>   // Load program keypairs
>   const oracleKeypair = loadKeypair('./target/deploy/oracle-keypair.json');
>   const marketKeypair = loadKeypair('./target/deploy/prediction_market-keypair.json');
>   
>   console.log('Oracle Program ID:', oracleKeypair.publicKey.toBase58());
>   console.log('Market Program ID:', marketKeypair.publicKey.toBase58());
>   
>   // Deploy programs (if not already deployed)
>   // `anchor deploy` handles this, but we initialize here
>   
>   // Initialize Oracle
>   const oracle = anchor.workspace.Oracle;
>   const [oracleConfigPda] = PublicKey.findProgramAddressSync(
>     [Buffer.from('oracle_config')],
>     oracle.programId
>   );
>   
>   // Check if already initialized
>   const oracleConfig = await provider.connection.getAccountInfo(oracleConfigPda);
>   if (!oracleConfig) {
>     console.log('Initializing Oracle...');
>     
>     // Generate TxLINE signer (in production, this would be TxLINE's actual key)
>     const txlineSigner = Keypair.generate();
>     console.log('TxLINE Signer (SAVE THIS):', txlineSigner.publicKey.toBase58());
>     
>     await oracle.methods
>       .initialize({
>         initialSigner: txlineSigner.publicKey,
>       })
>       .accounts({
>         authority: provider.wallet.publicKey,
>         oracleConfig: oracleConfigPda,
>         signerRegistry: /* derive */,
>         marketProgram: marketKeypair.publicKey,
>         feeReceiver: provider.wallet.publicKey,
>         systemProgram: SystemProgram.programId,
>       })
>       .rpc();
>       
>     console.log('Oracle initialized!');
>   }
>   
>   // Initialize Market Program
>   const market = anchor.workspace.PredictionMarket;
>   const [marketConfigPda] = PublicKey.findProgramAddressSync(
>     [Buffer.from('market_config')],
>     market.programId
>   );
>   
>   const marketConfig = await provider.connection.getAccountInfo(marketConfigPda);
>   if (!marketConfig) {
>     console.log('Initializing Prediction Market...');
>     
>     await market.methods
>       .initialize({
>         platformFeeBps: 200, // 2%
>         minBetLamports: new anchor.BN(10_000_000), // 0.01 SOL
>         maxBetLamports: new anchor.BN(10_000_000_000), // 10 SOL
>       })
>       .accounts({
>         authority: provider.wallet.publicKey,
>         marketConfig: marketConfigPda,
>         oracleProgram: oracleKeypair.publicKey,
>         treasury: provider.wallet.publicKey,
>         systemProgram: SystemProgram.programId,
>       })
>       .rpc();
>       
>     console.log('Prediction Market initialized!');
>   }
>   
>   // Output deployment info
>   const deploymentInfo = {
>     cluster,
>     programs: {
>       oracle: oracleKeypair.publicKey.toBase58(),
>       predictionMarket: marketKeypair.publicKey.toBase58(),
>     },
>     pdas: {
>       oracleConfig: oracleConfigPda.toBase58(),
>       marketConfig: marketConfigPda.toBase58(),
>     },
>     timestamp: new Date().toISOString(),
>   };
>   
>   fs.writeFileSync(
>     './deployment.json',
>     JSON.stringify(deploymentInfo, null, 2)
>   );
>   
>   console.log('\nDeployment complete!');
>   console.log(deploymentInfo);
> }
> 
> function loadKeypair(filepath: string): Keypair {
>   const absolutePath = path.resolve(filepath);
>   const secretKey = JSON.parse(fs.readFileSync(absolutePath, 'utf-8'));
>   return Keypair.fromSecretKey(Uint8Array.from(secretKey));
> }
> 
> main().catch(console.error);
> ```
> 
> **Update Anchor.toml for multiple clusters:**
> ```toml
> [features]
> seeds = false
> skip-lint = false
> 
> [programs.localnet]
> oracle = "YOUR_LOCAL_ORACLE_ID"
> prediction_market = "YOUR_LOCAL_MARKET_ID"
> 
> [programs.devnet]
> oracle = "YOUR_DEVNET_ORACLE_ID"
> prediction_market = "YOUR_DEVNET_MARKET_ID"
> 
> [programs.mainnet]
> oracle = "YOUR_MAINNET_ORACLE_ID"
> prediction_market = "YOUR_MAINNET_MARKET_ID"
> 
> [registry]
> url = "https://api.apr.dev"
> 
> [provider]
> cluster = "localnet"
> wallet = "~/.config/solana/id.json"
> 
> [scripts]
> test = "yarn run ts-mocha -p ./tsconfig.json -t 1000000 tests/**/*.ts"
> deploy = "ts-node deploy/deploy.ts"
> ```
> 
> **apps/web/vercel.json:**
> ```json
> {
>   "framework": "nextjs",
>   "buildCommand": "cd ../.. && pnpm turbo run build --filter=web",
>   "outputDirectory": ".next",
>   "installCommand": "pnpm install",
>   "env": {
>     "NEXT_PUBLIC_SOLANA_RPC_URL": "@solana_rpc_url",
>     "NEXT_PUBLIC_ORACLE_PROGRAM_ID": "@oracle_program_id",
>     "NEXT_PUBLIC_MARKET_PROGRAM_ID": "@market_program_id"
>   }
> }
> ```
> 
> **.github/workflows/deploy.yml:**
> ```yaml
> name: Deploy
> 
> on:
>   push:
>     branches: [main]
>   workflow_dispatch:
>     inputs:
>       environment:
>         description: 'Deployment environment'
>         required: true
>         default: 'devnet'
>         type: choice
>         options:
>           - devnet
>           - mainnet
> 
> jobs:
>   test:
>     runs-on: ubuntu-latest
>     steps:
>       - uses: actions/checkout@v4
>       - uses: pnpm/action-setup@v3
>         with:
>           version: 8
>       - uses: actions/setup-node@v4
>         with:
>           node-version: '20'
>           cache: 'pnpm'
>       - run: pnpm install
>       - run: pnpm test
> 
>   deploy-contracts:
>     needs: test
>     runs-on: ubuntu-latest
>     if: github.event_name == 'workflow_dispatch'
>     steps:
>       - uses: actions/checkout@v4
>       - uses: pnpm/action-setup@v3
>       - uses: actions/setup-node@v4
>         with:
>           node-version: '20'
>       - name: Install Solana CLI
>         run: |
>           sh -c "$(curl -sSfL https://release.solana.com/v1.18.0/install)"
>           echo "$HOME/.local/share/solana/install/active_release/bin" >> $GITHUB_PATH
>       - name: Install Anchor
>         run: |
>           cargo install --git https://github.com/coral-xyz/anchor anchor-cli --locked
>       - run: pnpm install
>       - name: Build contracts
>         working-directory: packages/contracts
>         run: anchor build
>       - name: Deploy
>         working-directory: packages/contracts
>         env:
>           SOLANA_PRIVATE_KEY: ${{ secrets.SOLANA_PRIVATE_KEY }}
>           SOLANA_CLUSTER: ${{ inputs.environment }}
>         run: |
>           echo "$SOLANA_PRIVATE_KEY" > /tmp/id.json
>           export ANCHOR_WALLET=/tmp/id.json
>           anchor deploy --provider.cluster $SOLANA_CLUSTER
>           pnpm run deploy
> 
>   deploy-frontend:
>     needs: test
>     runs-on: ubuntu-latest
>     steps:
>       - uses: actions/checkout@v4
>       - uses: pnpm/action-setup@v3
>       - uses: actions/setup-node@v4
>         with:
>           node-version: '20'
>       - run: pnpm install
>       - name: Deploy to Vercel
>         uses: amondnet/vercel-action@v25
>         with:
>           vercel-token: ${{ secrets.VERCEL_TOKEN }}
>           vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
>           vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
>           working-directory: apps/web
> ```

### Task 6.4: Create Environment and README Documentation
**Objective:** Document the project setup, environment variables, and deployment process for hackathon submission.  
**Target Files:** `README.md`, `.env.example`, `docs/ARCHITECTURE.md`

**Prompt for Agentic IDE:**
> Create comprehensive documentation:
> 
> **README.md:**
> ```markdown
> # ⚡ World Cup Oracle
> > Live micro-market predictions on Solana with instant settlement
> 
> [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
> [![Solana](https://img.shields.io/badge/Solana-Devnet-green)](https://solscan.io)
> 
> ## 🎯 Overview
> World Cup Oracle revolutionizes sports betting by offering **micro-markets** that resolve in minutes, not hours. Built for the **Prediction Markets & Settlement** hackathon track.
> 
> ### Key Features
> - **⚡ Instant Settlement**: Markets resolve within minutes of the triggering event
> - **🔐 Cryptographic Oracle**: TxLINE-signed match data verified on-chain via Ed25519
> - **🎮 Micro-Markets**: "Goal in next 10 minutes?", "Corner before 70th minute?" — dozens of markets per match
> - **💰 Fair Payouts**: Proportional share system with transparent 2% platform fee
> 
> ## 🏗️ Architecture
> ```text
> ┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
> │   TxLINE API    │────▶│  Relayer Node   │────▶│ Oracle Program  │
> │ (Signed Data)   │     │  (Ed25519 Sig)  │     │    (Solana)     │
> └─────────────────┘     └─────────────────┘     └────────┬────────┘
>                                                          │
>                                                          ▼
> ┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
> │    Frontend     │◀───▶│ Market Program  │◀────│   Match Data    │
> │   (Next.js)     │     │    (Solana)     │     │      PDA        │
> └─────────────────┘     └─────────────────┘     └─────────────────┘
> ```
> 
> ## 🚀 Quick Start
> 
> ### Prerequisites
> - Node.js 20+
> - pnpm 8+
> - Rust & Cargo
> - Solana CLI 1.18+
> - Anchor CLI 0.30+
> 
> ### Installation
> ```bash
> # Clone repository
> git clone https://github.com/your-org/world-cup-oracle
> cd world-cup-oracle
> 
> # Install dependencies
> pnpm install
> 
> # Build contracts
> cd packages/contracts
> anchor build
> 
> # Start local validator
> solana-test-validator
> 
> # Deploy locally
> anchor deploy
> 
> # Start relayer
> cd apps/relayer
> cp .env.example .env
> # Edit .env with your keys
> pnpm dev
> 
> # Start frontend
> cd apps/web
> cp .env.example .env.local
> pnpm dev
> ```
> 
> ## 📁 Project Structure
> ```text
> world-cup-oracle/
> ├── apps/
> │   ├── web/              # Next.js frontend
> │   └── relayer/          # Oracle relayer service
> ├── packages/
> │   ├── contracts/        # Anchor Solana programs
> │   │   ├── programs/
> │   │   │   ├── oracle/   # Oracle program
> │   │   │   └── prediction_market/
> │   │   └── tests/
> │   └── shared/           # Shared types & IDL
> └── docs/
> ```
> 
> ## 🔧 Environment Variables
> 
> ### Relayer (.env)
> ```
> SOLANA_RPC_URL=http://localhost:8899
> SOLANA_PRIVATE_KEY=<base64-encoded-keypair>
> TXLINE_API_URL=https://api.txline.io/v1
> TXLINE_API_KEY=<your-api-key>
> ORACLE_PROGRAM_ID=<deployed-program-id>
> MARKET_PROGRAM_ID=<deployed-program-id>
> ```
> 
> ### Frontend (.env.local)
> ```
> NEXT_PUBLIC_SOLANA_RPC_URL=http://localhost:8899
> NEXT_PUBLIC_ORACLE_PROGRAM_ID=<deployed-program-id>
> NEXT_PUBLIC_MARKET_PROGRAM_ID=<deployed-program-id>
> ```
> 
> ## 🧪 Testing
> ```bash
> # Contract tests
> cd packages/contracts
> anchor test
> 
> # Frontend tests
> cd apps/web
> pnpm test
> 
> # Run all tests
> pnpm test
> ```
> 
> ## 📖 Documentation
> - [Architecture Deep Dive](docs/ARCHITECTURE.md)
> - [Smart Contract Documentation](docs/CONTRACTS.md)
> - [API Reference](docs/API.md)
> 
> ## 🛡️ Security Considerations
> 1. **Ed25519 Signature Verification**: All oracle updates require valid TxLINE signatures
> 2. **PDA-based Access Control**: Markets and positions use deterministic PDAs
> 3. **Reentrancy Protection**: Claim functions update state before transfers
> 4. **Time-bound Markets**: Markets automatically lock based on match minute
> 
> ## 📄 License
> MIT License - see [LICENSE](LICENSE)
> ---
> Built with ⚡ for the Solana Prediction Markets Hackathon
> ```
> 
> **.env.example (root):**
> ```
> # Solana
> SOLANA_RPC_URL=http://localhost:8899
> SOLANA_PRIVATE_KEY=
> 
> # TxLINE Oracle Data
> TXLINE_API_URL=
> TXLINE_API_KEY=
> 
> # Program IDs (set after deployment)
> ORACLE_PROGRAM_ID=
> MARKET_PROGRAM_ID=
> ```
> 
> **docs/ARCHITECTURE.md:**
> ```markdown
> # Architecture Deep Dive
> 
> ## System Overview
> 
> ### 1. Oracle Layer
> The Oracle program ingests cryptographically signed match data from TxLINE and stores it on-chain.
> 
> **Key Components:**
> - `OracleConfig`: Global configuration (authority, fee receiver, pause state)
> - `SignerRegistry`: Whitelist of authorized TxLINE signing keys
> - `MatchDataAccount`: Per-match state (score, minute, events, status)
> 
> **Signature Verification Flow:**
> 1. Relayer receives signed payload from TxLINE API
> 2. Relayer builds transaction with Ed25519 verify instruction + oracle update
> 3. Oracle program checks instructions sysvar for valid signature
> 4. Oracle program verifies signer is in whitelist
> 5. Match data is written/updated on-chain
> 
> ### 2. Market Layer
> The Prediction Market program creates and manages micro-markets tied to oracle data.
> 
> **Key Components:**
> - `MarketConfig`: Platform settings (fees, bet limits)
> - `Market`: Individual market state (pools, outcome, status)
> - `UserPosition`: User's bet on a specific market
> 
> **Market Lifecycle:**
> 1. `create_market`: Anyone creates market for live match
> 2. `place_bet`: Users bet YES/NO while market is open
> 3. `resolve_market`: Triggered when resolution conditions met (match minute >= end minute)
> 4. `claim_winnings`: Winners claim proportional share of losing pool
> 
> ### 3. Relayer Service
> Node.js service that bridges TxLINE API and Solana.
> 
> **Workers:**
> - Oracle Update Worker: Polls TxLINE every 5s, submits signed updates
> - Resolution Worker: Monitors markets, triggers resolution when ready
> 
> ## PDA Derivation Reference
> 
> | Account | Seeds |
> |---------|-------|
> | OracleConfig | `["oracle_config"]` |
> | SignerRegistry | `["signer_registry"]` |
> | MatchData | `["match", match_id[32]]` |
> | MarketConfig | `["market_config"]` |
> | Market | `["market", market_id.to_le_bytes()]` |
> | UserPosition | `["position", market_pubkey, user_pubkey]` |
> | MarketVault | `["vault", market_pubkey]` |
> ```

---

## Summary
This implementation plan provides 42 atomic, executable tasks organized into 6 phases:
- **Phase 1 (5 tasks):** Monorepo setup with Turborepo, Anchor workspace, Next.js app, Node.js relayer, shared types
- **Phase 2A (6 tasks):** Oracle program — state accounts, errors, initialize, update_match with Ed25519 verification, admin functions
- **Phase 2B (10 tasks):** Prediction Market program — state, errors, initialize, create_market, place_bet, resolve_market, claim_winnings, admin functions, IDL generation
- **Phase 3 (6 tasks):** Backend relayer — TxLINE client, signature verification, Solana transaction builder, oracle worker, resolution worker, service assembly
- **Phase 4 (5 tasks):** Frontend — design system, core UI components, match display, market betting interface, layout/navigation
- **Phase 5 (3 tasks):** Web3 integration — Solana providers, program hooks, main application pages
- **Phase 6 (4 tasks):** Testing, deployment scripts, CI/CD, documentation

Each task includes specific file paths and detailed prompts ready for copy-paste into an agentic IDE. The plan covers cryptographic signature verification, PDA derivation, high-frequency settlement logic, and production-grade error handling throughout.
