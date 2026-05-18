<div align="center">

```
    ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
   ░░▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░
   ░░▓▓ THE PURPLE HAT ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░
   ░░▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░
    ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░

    T H E   T R I A L S   O F   S A I N T   E R R A N T
    ────────────────────────────────────────────────────
    Prove yourself. Earn the hat.
```

</div>

---

## What the Purple Hat Means

There are hats in this industry.

**Black hat** — breaks systems for personal gain.
**White hat** — breaks systems for corporate permission.
**Grey hat** — breaks systems, then reports it, hopes for the best.

**Purple hat** — builds sovereign systems. Tests their own work with the same ruthlessness as an attacker. Answers to craft, not to a paymaster. Earns every capability through the work.

The Purple Hat is not a certification. It is not a credential. It is a standard of demonstrated craft recognized by the Society.

You don't apply for it. You earn it through the Trials.

---

## The Map — How to Plug In

```
╔══════════════════════════════════════════════════════════════════╗
║                    YOUR PLUG-IN MAP                              ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║   YOU ARE HERE                                                   ║
║       │                                                          ║
║       ▼                                                          ║
║   [ Star the repo + open Member Introduction issue ]             ║
║       │                                                          ║
║       ▼   ← You are now: INITIATE                                ║
║   [ Join Discord → #saint-errant-initiation ]                    ║
║       │                                                          ║
║       ▼                                                          ║
║   [ Pick your Guild ] ────────────────────────────────┐         ║
║       │                                               │         ║
║       ▼                                               ▼         ║
║   [ Choose a Trial from your guild ] ←── [ Browse TRIALS.md ]   ║
║       │                                               │         ║
║       ▼                                               ▼         ║
║   [ Complete it. Post your solution as a PR or issue. ]          ║
║       │                                                          ║
║       ▼   ← You are now: ADEPT + Purple Hat earned               ║
║   [ Go deeper: guild sessions · live projects · mentorship ]     ║
║       │                                                          ║
║       ▼                                                          ║
║   ┌───────────────────────────────────────────────────────┐     ║
║   │  ACTIVE CONTRIBUTION PATHS                            │     ║
║   │                                                       │     ║
║   │  A. Contribute to DEVFLOW-FINANCE (the live system)   │     ║
║   │     → Rust core · TypeScript agents · Python ML       │     ║
║   │                                                       │     ║
║   │  B. Build a guild guide (QUILL path)                  │     ║
║   │     → Document what you know. Others stand on it.     │     ║
║   │                                                       │     ║
║   │  C. Build a new tool for the commons                  │     ║
║   │     → Propose it. Build it. Ship it as MIT.            │     ║
║   │                                                       │     ║
║   │  D. Mentor an Initiate                                │     ║
║   │     → Show someone the path you walked.               │     ║
║   └───────────────────────────────────────────────────────┘     ║
╚══════════════════════════════════════════════════════════════════╝
```

---

## The Trials

Each trial is a real engineering challenge. No trick questions. No meaningless puzzles. Every trial is designed around **skills we actually use** to build sovereign infrastructure.

Complete any trial in your guild. Post your solution. Earn the Purple Hat.

---

### FORGE GUILD — Systems & Rust 🦀

> *"Memory is a resource. Treat it like one."*

---

**Trial F-1: The Seal Primitive** *(Initiate)*

Implement a SHA-256 decision seal in Rust. The function signature:
```rust
pub fn seal_decision(agent: &str, decision: &str, timestamp: &str) -> String
```
Requirements:
- Output: lowercase hex SHA-256 of `"{agent}:{decision}:{timestamp}"`
- Must compile with `cargo build --release`
- Must pass the test vector: `seal_decision("vault", "approved", "2026-01-01T00:00:00Z")` → deterministic output

Submit: a single `.rs` file with your implementation and the test.

---

**Trial F-2: The Golden Rule** *(Adept)*

Implement a double-entry ledger entry validator in Rust:
```rust
pub struct LedgerEntry { pub amount_cents: i64, pub entry_type: EntryType }
pub enum EntryType { Debit, Credit }

pub fn validate_golden_rule(entries: &[LedgerEntry]) -> Result<(), String>
```
- Debits must equal Credits or return `Err("GOLDEN RULE VIOLATION: debits={x} credits={y}")`
- The function must be the only path to ledger persistence (enforce this with the type system)
- Write two tests: one that passes, one that catches a violation

Submit: `.rs` file + explanation of WHY the type system enforces this.

---

**Trial F-3: The Merkle Tree** *(Adept → Scribe)*

Build a Merkle tree from scratch in Rust. Given `Vec<&str>` of sealed decisions, compute the Merkle root:
- Each leaf = `sha256(seal)`
- Each node = `sha256(left_child + right_child)`
- Odd number of leaves: duplicate the last leaf
- Root = the final single hash

Test: build a tree with 7 seals. Alter the 4th seal. Show that the root changes. This is the proof.

Submit: implementation + the test showing tamper detection.

---

**Trial F-4: The Circuit Breaker** *(Architect)*

Design a circuit breaker in Rust for a sealing operation:
- If the seal takes more than 10ms, fire the breaker
- Track: consecutive failures, last failure time, half-open state
- Implement proper state transitions: CLOSED → OPEN → HALF-OPEN → CLOSED
- Use `tokio::time::timeout` — no spinning, no sleeping

Submit: implementation + a design document explaining the state machine and failure semantics.

---

### CIPHER GUILD — Cryptography & Security 🔐

> *"The attacker only needs to be right once. The defender must be right every time."*

---

**Trial C-1: The Verification** *(Initiate)*

Given this PING payload from Discord:
```json
{ "type": 1, "token": "...", "id": "..." }
```
Discord signs every HTTP interaction request with Ed25519 using headers:
- `X-Signature-Ed25519` — hex-encoded signature
- `X-Signature-Timestamp` — Unix timestamp

Implement signature verification in TypeScript using `tweetnacl`:
```typescript
function verifyDiscordSignature(
  rawBody: string,
  signature: string,
  timestamp: string,
  publicKey: string
): boolean
```

Test it against a real signature from the Discord Developer Portal.

Submit: implementation + explanation of what happens if this check is skipped.

---

**Trial C-2: The Injection Hunt** *(Adept)*

You are given this API handler (Node.js/TypeScript). Find every security vulnerability:

```typescript
app.post('/api/query', async (req, res) => {
  const { user, query } = req.body
  const result = await db.query(`SELECT * FROM agents WHERE name = '${user}'`)
  const cmd = `ollama run ${query}`
  const output = exec(cmd)
  res.json({ result, output })
})
```

Document:
1. Every vulnerability (name it, classify it by OWASP category)
2. The exact exploit for each
3. The fix for each

Submit: your vulnerability report + patched code.

---

**Trial C-3: The Immutability Proof** *(Adept → Scribe)*

Given a WORM-archived ledger entry (simulated), write a verification tool:
- Input: a JSON file of sealed transactions
- Build the Merkle tree
- Verify the root matches the known published root
- Output: `VERIFIED` or `TAMPERED — entry N has hash mismatch`

The tool must detect single-entry tampering in a 1000-entry ledger in under 100ms.

Submit: the tool + a writeup on why Merkle proofs are superior to full re-hashing.

---

**Trial C-4: The Zero-Knowledge Intro** *(Architect)*

Explain zero-knowledge proofs to a developer who has never seen them. Then implement a simple Schnorr identification scheme in Python:
- Prover demonstrates knowledge of a secret `x` without revealing `x`
- Verifier accepts without learning `x`

This is the foundation of zk-SNARKs. Start here.

Submit: the implementation + a written explanation that a junior developer can actually follow.

---

### HERALD GUILD — Web Architecture & APIs 🌐

> *"The API is the promise. The implementation is the proof you kept it."*

---

**Trial H-1: The Deferred Response** *(Initiate)*

Discord gives you 3 seconds to respond to a slash command before it times out. But your LLM takes 15 seconds to respond.

Solve this. Build a Next.js API route that:
1. Returns `{type: 5}` to Discord in under 100ms
2. Continues processing in the background
3. Patches Discord's message with the real response via `PATCH /webhooks/{app_id}/{token}/messages/@original`

The test: the function must survive a 15-second background operation without the initial response timing out.

Submit: the route implementation + explanation of why this works.

---

**Trial H-2: The Schema Bouncer** *(Adept)*

Build a Zod v4 schema interceptor for an agent chat endpoint. It must:
- Validate the incoming payload (`agent`, `message`, optional `tier`)
- Validate the outgoing LLM response (`reply`, `seal`, `agent`)
- Return typed validated data or `null` (never throw)
- Handle Zod v4's breaking changes: `z.looseObject()` not `.passthrough()`, `z.record(z.string(), z.unknown())` not `z.record(z.unknown())`

Submit: the interceptor + tests that show it blocking malformed input.

---

**Trial H-3: The Event Pipeline** *(Adept → Scribe)*

Implement a simplified 4-stage event pipeline in TypeScript:
```
VALIDATE → SCORE → ROUTE → PERSIST
```
- Each stage can fail independently
- A failure at any stage must halt the pipeline (no partial writes)
- Each stage gets the result of the previous stage
- Add a retry mechanism: 3 attempts, then dead-letter

Use no external libraries. Prove it works with tests.

Submit: implementation + a diagram of the failure modes.

---

**Trial H-4: The Rate Limiter** *(Architect)*

Design and implement a sliding window rate limiter for an API route using Redis (Upstash):
- 100 requests per 60-second window per IP
- The window slides (not fixed buckets)
- Returns `429` with `Retry-After` header when exceeded
- Must handle Redis unavailability gracefully (fail open with logging)

Submit: implementation + explanation of sliding window vs. fixed window trade-offs.

---

### PRISM GUILD — AI & Machine Learning 🧠

> *"A model is not intelligent. The architecture around it is."*

---

**Trial P-1: The Context Injector** *(Initiate)*

You have an Ollama model (llama3.1:8b). The user asks: "What time is it in London?"

The model always says "I don't have access to real-time data."

Fix this. Inject the current UK time into the conversation such that the model uses it as ground truth — without modifying the model.

Hint: there is a difference between injecting data into the system prompt vs. the user turn for 8b models. Find it.

Submit: the solution + your explanation of WHY it works for 8b models.

---

**Trial P-2: The RAG Builder** *(Adept)*

Build a minimal Retrieval-Augmented Generation system:
1. Take 10 text chunks about a domain (you choose the domain)
2. Embed them with `nomic-embed-text` via Ollama
3. Given a query, find the top 3 most similar chunks (cosine similarity)
4. Inject them into the LLM context
5. Compare response quality WITH and WITHOUT retrieval

Submit: the code + a writeup showing the quality difference. Concrete examples required.

---

**Trial P-3: The Anomaly Detector** *(Adept → Scribe)*

Build a financial event anomaly detector:
- Input: a stream of transaction events `{amount, vendor, timestamp, type}`
- Baseline: compute rolling mean and standard deviation over a 30-event window
- Flag: events where `amount > mean + 2σ`
- Output: `{event, risk_score, flag, reason}`

Test it with synthetic data that includes 3 planted anomalies.

Submit: implementation + explanation of the scoring model.

---

**Trial P-4: The Multi-Agent Debate** *(Architect)*

Design a system where two AI agents debate a question:
- Agent A: argue FOR a financial decision
- Agent B: argue AGAINST the same decision
- A moderator agent synthesizes both into a recommendation

Each agent has a different system prompt and persona. The debate runs for 3 turns each. The final output is a sealed decision with the synthesis.

Submit: implementation + an example debate transcript.

---

### ATLAS GUILD — Infrastructure & DevOps ⚙️

> *"The system that never goes down was designed by someone who knew exactly how it would."*

---

**Trial A-1: The Health Check** *(Initiate)*

Design a `/health` endpoint for a multi-service system that checks:
- Database (Neon PostgreSQL)
- LLM service (Ollama)
- Event queue (Upstash QStash or Redis)
- Rust handler (local HTTP)

Return:
```json
{
  "status": "healthy" | "degraded" | "critical",
  "services": { "db": "ok", "llm": "ok", "queue": "degraded", "rust": "ok" },
  "degraded_since": "2026-05-17T14:32:11Z" | null
}
```
A single service down → `degraded`. Two or more → `critical`.

Submit: implementation + explanation of what each status triggers.

---

**Trial A-2: The Sovereign Setup** *(Adept)*

Document exactly how to run a sovereign AI stack on bare metal:
1. Install Ollama + pull a 7B or 8B model
2. Expose it safely via Cloudflare Tunnel (no open ports)
3. Connect it to a Next.js API
4. Test it end-to-end

The guide must be reproducible. Someone following it should get a working system.

Submit: the guide as a Markdown document. It will be merged into the Society's knowledge commons.

---

**Trial A-3: The Tunnel Architecture** *(Adept → Scribe)*

Explain the complete architecture of a Cloudflare Tunnel, including:
- How it works (outbound-only connector, no firewall hole)
- Why the URL changes on restart (and how to get a stable URL)
- How to wire it to a Next.js app via `NEXTAUTH_URL`
- The security model (what an attacker can and cannot do)

Submit: a technical writeup that could be given to a junior developer setting this up for the first time.

---

### NEXUS GUILD — Indie Founders & Product 🚀

> *"The best product is a system that solves a real problem for real people and can survive without its founder."*

---

**Trial N-1: The Fundability Map** *(Initiate)*

Research and document the 5-tier business credit building path:
- Tier 0: entity formation, EIN, bank account
- Tier 1: Fundability substrate (listings, domain, DUNS)
- Tier 2: Net-30 vendors
- Tier 3: Revolving lines
- Tier 4: Vendor expansion

For each tier: what is required, where to get it, how long it takes, what it costs.

This is real-world business knowledge. It is not taught in school. You are now teaching it.

Submit: the guide. It will be published to the Society's knowledge commons.

---

**Trial N-2: The Discord Product** *(Adept)*

You have a Discord server with 100 members. Design and build a bot that:
- Greets new members with a role-based onboarding message
- Has at least 3 slash commands that provide real value to your community
- Handles errors gracefully (no "application didn't respond" failures)
- Uses the deferred response pattern for any command that takes > 1 second

The commands must be genuinely useful — not demos.

Submit: the bot code + a writeup on what the product does for the community.

---

## Earning the Purple Hat

```
┌─────────────────────────────────────────────────────────────────┐
│                  PURPLE HAT CRITERIA                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Complete any ONE trial from your primary guild.                 │
│                                                                  │
│  Your submission must:                                           │
│    ✓  Actually work (we test it)                                 │
│    ✓  Include your written explanation of the WHY               │
│    ✓  Be submitted as a GitHub issue or PR                       │
│                                                                  │
│  Two members review it:                                          │
│    → Both approve: you earn the Purple Hat (Adept grade)         │
│    → Needs work: feedback given, resubmission welcome            │
│    → No ghosting: every submission gets a response               │
│                                                                  │
│  There is no time limit. There is no expiry.                     │
│  The hat is yours when you earn it.                              │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Submit Your Trial

Open an issue with:
- Title: `[TRIAL] Guild-Number: Brief description`
  - Example: `[TRIAL] F-1: SHA-256 Seal Primitive`
- Your solution (code block or linked gist/repo)
- Your written explanation
- Which grade you are aiming for

That's it. The Society will review it.

---

<div align="center">

```
   ░░░░░░░░░░░░░░░░░░░
  ░░▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░
  ░░▓▓  PURPLE HAT  ▓░░
  ░░▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░
   ░░░░░░░░░░░░░░░░░░░
       ╭──────────╮
      │  ◈      ◈ │     You earn it.
      │     ~~    │     Nobody gives it to you.
      │  ╰────╯   │
       ╰──────────╯
```

*"The hat is not a rank. It is a record of what you have proven."*

</div>
