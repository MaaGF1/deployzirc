# Deploy ZIRC: The Springboard Architecture

This repository (`deployzirc`) is a headless springboard designed to execute the ZIRC automation routines via GitHub Actions for free, while securely fetching the core source code from the `MaaGF1/ZIRC` repository.

## 1. Generating the Required Token (GH_PAT)

Since the GHA Runner needs to both clone a private repository and recursively trigger itself, the default `GITHUB_TOKEN` is insufficient. You must create a Classic PAT.

1. Go to your GitHub Settings -> `Developer settings` -> `Personal access tokens` -> `Tokens (classic)`.
2. Click `Generate new token (classic)`.
3. Name it something like `ZIRC_SPRINGBOARD_TOKEN`.
4. Set expiration to `No expiration` (or configure as per your security needs).
5. **CRITICAL - Scopes required:**
   * Check **`repo`** (Full control of private repositories). This grants read access to `MaaGF1/ZIRC` and write access to trigger workflows in the current repo.
   * Check **`workflow`** (Update GitHub Action workflows).
6. Generate the token and copy the string starting with `ghp_...`.

## 2. Configuring Repository Secrets

Navigate to your `deployzirc` repository settings: 
`Settings -> Secrets and variables -> Actions -> New repository secret`

You must configure the following **4 Secrets**:

1. **Secret Name:** `GH_PAT`
   * **Value:** The `ghp_...` token you just generated.
2. **Secret Name:** `GFL_CONFIG`
   * **Value:** Nothing Changed.
3. **Secret Name:** `GFL_SIGN_KEY`
   * **Value:** Nothing Changed.
4. **Secret Name:** `GFL_USER_DEVICE`
   * **Value:** Nothing Changed.

## 3. Execution

1. Go to the **Actions** tab of this repository.
2. Select **GFL Auto Farm Workflow**.
3. Click **Run workflow**, choose your desired `mission_type` (e.g., `f2p`, `epa_fifo`), and execute.
4. The runner will automatically authenticate using your `GH_PAT`, download the private engine into the runner's ephemeral memory, execute the farming loop, and restart itself endlessly until a fatal server error breaks the loop.