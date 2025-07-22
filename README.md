# 🤖 VADAR 2.0 — Autonomous Multichain AI Trading Bot

**VADAR 2.0** is a next-generation autonomous trading bot built using [Mastra](https://docs.recall.network/competitions/guides/mastra), [Groq LLMs](https://console.groq.com), and the [Recall Network API](https://docs.recall.network). It leverages large language models to analyze trades, snipe price movements, resolve token data from Coingecko, and execute trades across **Ethereum**, **Solana**, **Base**, and **Arbitrum** networks.

This bot is modular, extensible, and ideal for both degen-style traders and more strategic agents.

---

## 🚀 Features

* 🧠 **AI-Powered Trading Agent** using `llama3-70b-8192` on Groq
* 🌍 **Multichain Support** — Ethereum, Solana, Base, Arbitrum
* 🔫 **Sniper Tool** — Targets tokens that cross price thresholds
* 💰 **Portfolio Tool** — View agent wallet balances
* 🧹 **Modular Tools** — Clean separation via Mastra tools
* 🧲 **Token Resolver** — Fetches contract addresses using Coingecko
* 📡 **Recall Trading API** — Executes simulated trades in a sandbox

---

## 📁 Folder Structure

```
vadar-2.0/
├── src/
│   ├── index.ts                # Mastra entry
│   └── mastra/
│       ├── agents/
│       │   └── recall-agent.ts       # Groq-powered trading agent
│       ├── workflows/
│       │   └── recall-workflow.ts    # Trading workflow
│       ├── tools/
│       │   ├── recall-trade.ts       # Executes a trade
│       │   ├── sniper-tool.ts        # Executes snipe logic
│       │   ├── portfolio-tool.ts     # Returns current balance
│       │   └── coingecko-resolver.ts # Resolves token names to addresses
│       └── index.ts            # Registers agents and workflows
├── utils/
│   └── api.ts                  # Axios wrapper for Recall API
├── .env                        # Your secrets and API keys
├── package.json
└── README.md
```

---

## 🧠 Agent Logic (LLM-Powered)

The **Recall Agent** is powered by Groq’s `llama3-70b-8192`. Its instructions include:

* Pick the right tool (`recallTrade`, `sniperTool`, etc.)
* Auto-select network: `eth`, `solana`, `base`, `arbitrum`
* Use `coingecko-resolver` to identify contract addresses from token names
* Only submit **one trade per prompt**
* Optionally query your portfolio with `portfolioTool`

---

## ⚖️ Setup & Installation

### 1. Clone the repo

```bash
git clone https://github.com/hormorbhor/vadar-2.0.git
cd vadar-2.0
```

### 2. Install dependencies

```bash
npm install
```

### 3. Add environment variables

Create a `.env` file in the root of your project:

```env
RECALL_API_URL=https://api.sandbox.competitions.recall.network
RECALL_API_KEY=your_recall_api_key_here
GROQ_API_KEY=your_groq_api_key_here
```

> You can get your Recall API key from: [https://recall.network/](https://recall.network)
> And your Groq key from: [https://console.groq.com](https://console.groq.com)

---

## ▶️ Run the Bot

```bash
npm run dev
```

Then visit the **Mastra Playground** at:

```
http://localhost:4111
```

Type a command like:

```
Trade 100 USDC for SHIB on Ethereum
```

or

```
Snipe any token that just pumped past $0.00001
```

or

```
Check my portfolio
```

---

## 🛠️ Tools & Utilities

### 1. `recall-trade.ts`

Sends a trade request to the Recall API:

```ts
{
  fromToken: "ERC20 contract address",
  toToken: "ERC20 contract address",
  amount: "human-readable amount",
  reason: "why you’re trading",
  chain: "eth" | "solana" | "base" | "arbitrum"
}
```

### 2. `sniper-tool.ts`

Triggers trades when token price exceeds a threshold (e.g., > \$0.00001). Custom logic can be implemented to define snipe conditions.

### 3. `portfolio-tool.ts`

Fetches your current portfolio or token balances:

```bash
GET /api/agent/balances
Authorization: Bearer <RECALL_API_KEY>
```

### 4. `coingecko-resolver.ts`

Looks up a token’s contract address by its name/symbol via Coingecko:

```ts
GET https://api.coingecko.com/api/v3/coins/list
GET https://api.coingecko.com/api/v3/coins/{id}
```

---

## 🧪 Example Prompt Flow

| Prompt                                        | Agent Behavior                                                     |
| --------------------------------------------- | ------------------------------------------------------------------ |
| "Trade 100 USDC for SHIB"                     | Resolves USDC & SHIB → fetches addresses → trades via recall-trade |
| "Snipe memecoins above \$0.0001"              | Fetches price → compares → fires sniper tool                       |
| "Check portfolio"                             | Uses portfolioTool to fetch `/api/agent/balances`                  |
| "What's the contract for \$PEPE on Ethereum?" | Uses Coingecko Resolver                                            |

---

## 💠 Extending the Bot

* Add new tools inside `src/mastra/tools/`
* Register them in `recall-agent.ts` under `tools: { ... }`
* Update `recall-agent.ts` instructions to include logic and examples
* You can build advanced workflows (e.g., trade loops, schedules, alerts)

---

## ⚠️ Notes

* This bot runs in the **Recall sandbox**, meaning trades are simulated for the competition environment.
* Real-world deployment requires secure key management and possibly smart contract integration.

---

## 🧾 License

MIT License. You are free to modify, fork, and contribute.

---

## 📬 Contact / Contributions

For feedback or contributions, open a pull request or DM me on Twitter: [@yourhandle](https://twitter.com/undi_isputed)

---

Enjoy the dark side of DeFi trading with 🚈 **VADAR 2.0**.
