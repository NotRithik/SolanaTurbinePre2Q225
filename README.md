# SolanaTurbinePre2Q225

This repository contains the Rust code developed for Part 2 of the Turbin3 Solana prerequisite assessment (targeting Q2 2025 cohort program ID). It demonstrates fundamental interactions with the Solana blockchain using the Rust SDK, mirroring the tasks from Part 1 (TypeScript).

The core functionalities implemented include:
*   Generating Solana keypairs.
*   Requesting SOL airdrops on Devnet.
*   Transferring specific amounts of SOL.
*   Calculating fees and transferring the entire remaining balance of an account.
*   Deriving Program Derived Addresses (PDAs).
*   Interacting with a specific on-chain program (`ADcaide4vBtKuyZQqdU689YqEGZMCmS4tL35bdTv9wJa`) via its IDL using the `solana-idlgen` crate to submit enrollment data.
*   Utility functions for converting between wallet file byte arrays and base58 private key formats.

## Prerequisites

*   Rust and Cargo installed (latest stable version recommended). You can install them via [rustup](https://rustup.rs/).

## Setup

1.  **Clone the repository:**
    ```bash
    git clone <your-repo-url> SolanaTurbinePre2Q225
    cd SolanaTurbinePre2Q225
    ```

2.  **Build dependencies:**
    ```bash
    cargo build
    ```
    This command will compile the code and download all necessary dependencies specified in `Cargo.toml`, including fetching the `solana-idlgen` crate from its Git repository.

## Configuration: Wallet Files (IMPORTANT)

This project requires **two** wallet files containing secret keys in the Solana byte array format (e.g., `[12, 34, ..., 56]`).

**These files are NOT included in the repository and should NEVER be committed to Git.** The included `.gitignore` file explicitly ignores `*wallet.json` to help prevent accidental commits.

You need to create these files manually in the root directory (`SolanaTurbinePre2Q225/`):

1.  **`dev-wallet.json`:** This wallet will be used for receiving airdrops and performing initial transfers.
    *   Run the keygen function: `cargo test keygen -- --nocapture`
    *   Copy the byte array output (the line starting with `[` and ending with `]`).
    *   Create a file named `dev-wallet.json` in the project root.
    *   Paste the copied byte array into this file. Make sure it's the only content.

2.  **`Turbin3-wallet.json`:** This wallet **must** be the one you originally registered with Turbin3 for the course application. It will be used for the final enrollment submission (`submit` function).
    *   Retrieve the secret key byte array for the wallet you applied with (you might have saved this from Part 1 of the prerequisites or need to export it securely from your wallet software and convert it if necessary).
    *   Create a file named `Turbin3-wallet.json` in the project root.
    *   Paste the correct byte array into this file.

## Usage

All functionalities are implemented as test functions within `src/lib.rs`. You can run them individually using `cargo test <function_name>`.

The `-- --nocapture` flag is required for tests that print output (`println!`) or require user input (`base58_to_wallet`, `wallet_to_base58`) so that the input/output is correctly handled in the terminal.

*   **Generate a new Keypair:**
    ```bash
    cargo test keygen -- --nocapture
    ```

*   **Airdrop 2 SOL to `dev-wallet.json`:**
    ```bash
    cargo test airdrop -- --nocapture
    ```
    *(Note: Devnet airdrops can sometimes be slow or fail. Check the balance on the explorer or retry if the transaction link doesn't work immediately.)*

*   **Transfer 0.1 SOL from `dev-wallet.json`:** (Ensure `dev-wallet.json` has funds first)
    ```bash
    cargo test transfer_sol -- --nocapture
    ```
    *(Note: This uses a hardcoded recipient address from the tutorial. Modify `src/lib.rs` if needed.)*

*   **Transfer ALL remaining SOL (minus fees) from `dev-wallet.json`:**
    ```bash
    cargo test transfer_all_sol -- --nocapture
    ```

*   **Submit Enrollment to Turbin3 Program:** (Requires `Turbin3-wallet.json` to be funded and correct)
    ```bash
    cargo test submit -- --nocapture
    ```
    *(Note: You **must** edit the `github` variable inside the `submit` function in `src/lib.rs` to use your actual GitHub username you applied with!)*

*   **Convert Base58 Private Key to Wallet Byte Array:**
    ```bash
    cargo test base58_to_wallet -- --nocapture
    ```
    *(Paste Base58 key when prompted)*

*   **Convert Wallet Byte Array to Base58 Private Key:**
    ```bash
    cargo test wallet_to_base58 -- --nocapture
    ```
    *(Paste byte array like `[1,2,...]` when prompted)*

## Key Concepts Covered

*   Keypair generation and management (`solana-sdk`).
*   Connecting to Solana RPC nodes (`solana-client`).
*   Requesting SOL airdrops.
*   Constructing, signing, and sending transactions (`Transaction`).
*   Using the System Program for SOL transfers (`system_instruction::transfer`).
*   Calculating transaction fees (`get_fee_for_message`).
*   Deriving Program Derived Addresses (PDAs).
*   Using `solana-idlgen` to generate a Rust client from an Anchor IDL.
*   Calling instructions on an on-chain program using the generated client.

## Disclaimer

Do not bother looking for my actual wallets' private keys here, you won't find them :)

---
