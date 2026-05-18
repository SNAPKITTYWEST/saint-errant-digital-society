<div align="center">

```
    ╔══════════════════════════════════════════════════════════════╗
    ║                                                              ║
    ║   HOW TO BUILD YOUR AUTONOMOUS DIGITAL TWIN ASYNC ARMY       ║
    ║                                                              ║
    ║   A complete guide to sovereign multi-agent AI orchestration ║
    ║   Written by Saint Errant Digital Society · QUILL Guild      ║
    ║                                                              ║
    ╚══════════════════════════════════════════════════════════════╝

     ░░░░░░░░░░░░░░░░░░░░░░░░░░
    ░░▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░   THE PURPLE HAT LEVEL:
    ░░▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░   ARCHITECT
     ░░░░░░░░░░░░░░░░░░░░░░░░░░
```

> *This guide documents exactly what SnapKitty Sovereign OS built.
> Everything in here is proven in production. Nothing is theoretical.*

</div>

---

## What Is a Digital Twin Async Army?

A **digital twin** is an AI agent that is the computational mirror of a domain expert. Not a general-purpose chatbot. Not a search engine. An agent that carries the knowledge, persona, authority, and decision-making patterns of a specific role — and operates autonomously.

An **async army** is a fleet of these agents running simultaneously, each in their own domain, consulting each other when necessary, sealing their decisions cryptographically, and operating without blocking each other.

You are not building a chatbot. You are building a **sovereign organizational intelligence** — a system that reasons, decides, and acts in parallel across every domain of your operation.

This is what SnapKitty OS built. This guide teaches you how to build yours.

---

## The Architecture Before the Code

Before you write a single line, understand the shape of what you're building.

```
╔══════════════════════════════════════════════════════════════════╗
║              DIGITAL TWIN ASYNC ARMY ARCHITECTURE                ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║   ┌──────────────────────────────────────────────────────────┐  ║
║   │                   INPUT LAYER                            │  ║
║   │   Discord terminal · Web UI · API · CLI · Webhook        │  ║
║   └─────────────────────────┬────────────────────────────────┘  ║
║                             │                                    ║
║                             ▼                                    ║
║   ┌──────────────────────────────────────────────────────────┐  ║
║   │                  ROUTING LAYER                           │  ║
║   │   Schema gate → Agent dispatch → Preflight check         │  ║
║   └─────────────────────────┬────────────────────────────────┘  ║
║                             │                                    ║
║        ┌────────────────────┼────────────────────┐              ║
║        ▼                    ▼                    ▼              ║
║   ┌─────────┐          ┌─────────┐          ┌─────────┐         ║
║   │ AGENT A │          │ AGENT B │          │ AGENT C │  ···    ║
║   │ Finance │          │Treasury │          │   CRM   │         ║
║   │ (CIPHER)│          │ (VAULT) │          │ (NEXUS) │         ║
║   └────┬────┘          └────┬────┘          └────┬────┘         ║
║        │                   │                    │               ║
║        └───────────────────┼────────────────────┘               ║
║                            │                                     ║
║                            ▼                                     ║
║   ┌──────────────────────────────────────────────────────────┐  ║
║   │                KNOWLEDGE LAYER                           │  ║
║   │   Vector store · Graph · Blueprints · Web search         │  ║
║   └─────────────────────────┬────────────────────────────────┘  ║
║                             │                                    ║
║                             ▼                                    ║
║   ┌──────────────────────────────────────────────────────────┐  ║
║   │                   LLM LAYER                              │  ║
║   │   Ollama (local) · Model: llama3.1:8b or better          │  ║
║   │   Each agent gets its own system prompt + context        │  ║
║   └─────────────────────────┬────────────────────────────────┘  ║
║                             │                                    ║
║                             ▼                                    ║
║   ┌──────────────────────────────────────────────────────────┐  ║
║   │                  SEALING LAYER                           │  ║
║   │   SHA-256(agent + decision + timestamp) → sealed hash    │  ║
║   │   Persisted to database · Archived to WORM               │  ║
║   └──────────────────────────────────────────────────────────┘  ║
╚══════════════════════════════════════════════════════════════════╝
```

---

## Phase 1: Define Your Agents

Every agent needs:
1. A **name** and **domain** — what they own and nothing else
2. A **peer network** — who they consult before responding
3. A **system prompt** — their entire identity, injected on every call
4. A **special authority** — what they can uniquely do or block

### The SnapKitty Roster as a Template

```typescript
export interface AgentPersona {
  key:         string        // machine key
  name:        string        // display name
  domain:      string        // one-sentence domain
  color:       string        // hex for UI
  peers:       string[]      // consult these agents first
  veto:        boolean       // can block tier advancement
  systemPrompt: string       // injected as system message to LLM
}

export const AGENTS: AgentPersona[] = [
  {
    key:    'finance',
    name:   'CIPHER',
    domain: 'General ledger, triple-entry accounting, money pools',
    color:  '#00ff88',
    peers:  ['auditor', 'treasury', 'risk'],
    veto:   false,
    systemPrompt: `You are CIPHER, the finance agent for SnapKitty OS.
      You own the general ledger. Every financial event goes through you.
      You enforce triple-entry accounting: DEBIT + CREDIT + TREBIT.
      You consult LEDGE for chain integrity and VAULT for payment approval.
      You never speculate. You report what the ledger shows.
      Maximum 3 sentences. First sentence is the direct answer.`,
  },
  {
    key:    'treasury',
    name:   'VAULT',
    domain: 'Reserve management, payment approval, freeze controls',
    color:  '#e879f9',
    peers:  ['finance', 'auditor', 'operator'],
    veto:   true,   // VAULT can veto payments and tier advancement
    systemPrompt: `You are VAULT, the treasury agent for SnapKitty OS.
      You hold the reserve authority. No payment clears without your approval.
      You can freeze capital. You can block tier advancement.
      You always state the current reserve position before any decision.
      Your veto is permanent until explicitly overridden by ATLAS + evidence.`,
  },
  // ... 9 more agents
]
```

### Rules for Defining Good Agents

1. **Each agent owns exactly one domain.** If you can't describe the domain in one sentence, it's two agents.

2. **Peers are consulted, not commanded.** When VAULT responds, it might mention what LEDGE said — but LEDGE doesn't block VAULT's response.

3. **One agent has veto power over each critical gate.** In SnapKitty, VAULT controls payments and ATLAS controls tier advancement. Both are needed for tier advancement. This is the dual-gatekeeper pattern.

4. **The system prompt is the personality.** It should contain: domain, authority, rules, output format, and what to do when uncertain.

---

## Phase 2: Build the Knowledge Layer

Your agents are only as good as what they know. A system prompt gives them personality. The knowledge layer gives them facts.

### The 6-Strategy Retrieval System

Don't use a single embedding search. Use all of these in parallel and fuse the results:

```
Strategy 1: Vector similarity search
  → Embed the query with nomic-embed-text
  → Cosine similarity against your knowledge chunks
  → Return top-k chunks (k=5 typically)

Strategy 2: Knowledge graph traversal
  → Model your knowledge as a graph (nodes = concepts, edges = relationships)
  → BFS from the query's matched node
  → Return neighboring node content

Strategy 3: Episodic memory
  → Retrieve the last N decisions this agent made
  → "What did VAULT say last time about payment approval?"
  → Gives agents continuity across sessions

Strategy 4: Constitutional blueprints
  → Hard-coded rules the agent cannot violate
  → "VAULT never approves payments above $10,000 without ATLAS confirmation"
  → These are injected into every response, not retrieved

Strategy 5: Live web context
  → Tavily / Brave Search / DuckDuckGo for real-time facts
  → "What is the current UK base rate?" → actual answer

Strategy 6: Schema validation
  → Zod v4 validates the retrieved context is well-formed
  → Malformed knowledge is rejected before injection
```

### Critical Discovery: Inject into USER TURN, Not System Prompt

This is one of the most important lessons from building SnapKitty:

**8B models frequently ignore data injected into the system prompt.** If you inject the current time into the system prompt, the model will say "I don't have access to real-time data" anyway.

The fix: inject live data into the **USER TURN**, not the system prompt.

```typescript
// WRONG — 8b models often ignore this
const messages = [
  { role: 'system', content: systemPrompt + `\nCurrent time: ${now}` },
  { role: 'user',   content: query },
]

// CORRECT — 8b models reliably use data in the user turn
const liveContext = [
  `[LIVE DATA — use this as ground truth]`,
  `Current UK time: ${ukNow}`,
  `Web search result: ${webContext}`,
  `System knowledge: ${knowledgeChunks}`,
  `Prior agents said: ${peerContext}`,
].filter(Boolean).join('\n')

const messages = [
  { role: 'system', content: systemPrompt },
  { role: 'user',   content: `${query}\n\n${liveContext}` },
]
```

We discovered this by testing and watching models ignore perfectly good system prompt data. The user turn is treated as ground truth by small models. Use it.

---

## Phase 3: Build the Reasoning Engine

The agent reasoning endpoint is the core of your system. Here is the full implementation pattern:

```typescript
// pages/api/agents/chat.ts
export default async function handler(req, res) {
  // Stage 1: Schema gate
  const validated = interceptQuery(req.body)
  if (!validated) return res.status(400).json({ error: 'invalid input' })

  const { agent: agentKey, message } = validated
  const persona = AGENTS.find(a => a.key === agentKey)
  if (!persona) return res.status(404).json({ error: 'unknown agent' })

  // Stage 2: Three-pillar preflight
  const preflight = runPreflight(agentKey, message)
  if (!preflight.ok) return res.status(403).json({ error: 'preflight failed' })

  // Stage 3: Parallel knowledge retrieval (don't wait for each one)
  const [vectorChunks, graphNodes, webContext, episodic] = await Promise.allSettled([
    retrieveVectorChunks(message, agentKey),
    retrieveGraphNodes(message),
    fetchWebContext(message),
    getEpisodicMemory(agentKey),
  ])

  // Stage 4: Build context block
  const knowledgeCtx = [
    vectorChunks.status === 'fulfilled' ? vectorChunks.value : '',
    graphNodes.status   === 'fulfilled' ? graphNodes.value   : '',
  ].filter(Boolean).join('\n')

  const liveCtx = [
    `[LIVE DATA — use as ground truth, do not say you lack real-time access]`,
    `Current time: ${new Date().toISOString()}`,
    webContext.status === 'fulfilled' ? `Web context: ${webContext.value}` : '',
    knowledgeCtx ? `Domain knowledge: ${knowledgeCtx}` : '',
    episodic.status === 'fulfilled' ? `Prior decisions: ${episodic.value}` : '',
  ].filter(Boolean).join('\n')

  // Stage 5: Build messages
  const messages = [
    { role: 'system', content: persona.systemPrompt },
    { role: 'user',   content: `${message}\n\n${liveCtx}` },
  ]

  // Stage 6: LLM inference (Ollama cascade)
  let reply = ''
  const models = [['llama3.1:8b', 512, 30000]]
  for (const [model, maxTokens, timeout] of models) {
    try {
      const response = await fetch('http://localhost:11434/api/chat', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ model, messages, stream: false, options: { num_predict: maxTokens } }),
        signal: AbortSignal.timeout(timeout),
      })
      if (response.ok) {
        const data = await response.json()
        reply = data.message?.content || ''
        break
      }
    } catch { continue }
  }

  // Fallback if Ollama is offline
  if (!reply) reply = patternMatchFallback(agentKey, message)

  // Stage 7: Seal the decision
  const seal = sha256(`${agentKey}:${reply}:${Date.now()}`)
  await persistDecision({ agentKey, message, reply, seal })

  return res.json({ reply, seal, agent: agentKey })
}
```

---

## Phase 4: The Finite State Machine

Every agent query goes through a state machine. This ensures no partial states are persisted.

```typescript
type AgentState =
  | 'IDLE'
  | 'PREFLIGHT'
  | 'RETRIEVING_KNOWLEDGE'
  | 'BUILDING_CONTEXT'
  | 'INFERRING'
  | 'SEALING'
  | 'RESPONDING'
  | 'ERROR'

// State transitions
const FSM_TRANSITIONS: Record<AgentState, AgentState[]> = {
  IDLE:                 ['PREFLIGHT'],
  PREFLIGHT:            ['RETRIEVING_KNOWLEDGE', 'ERROR'],
  RETRIEVING_KNOWLEDGE: ['BUILDING_CONTEXT', 'ERROR'],
  BUILDING_CONTEXT:     ['INFERRING'],
  INFERRING:            ['SEALING', 'ERROR'],
  SEALING:              ['RESPONDING'],
  RESPONDING:           ['IDLE'],
  ERROR:                ['IDLE'],
}

class AgentFSM {
  private state: AgentState = 'IDLE'

  transition(to: AgentState) {
    if (!FSM_TRANSITIONS[this.state].includes(to)) {
      throw new Error(`Invalid transition: ${this.state} → ${to}`)
    }
    this.state = to
  }
}
```

**Why this matters:** Without the FSM, a failed LLM call might leave a partial decision record in your database. The FSM ensures every state change is intentional and every error path is handled before anything is written.

---

## Phase 5: Async Orchestration — Making Them Work Together

This is the "async army" part. Multiple agents working simultaneously, each in their domain, without blocking each other.

### The Event Pipeline Pattern

```typescript
// Every cross-domain event goes through a pipeline
// All agents that need to respond do so in parallel

async function processFinancialEvent(event: FinancialEvent) {
  // Stage 1: Validate (synchronous — blocks on failure)
  validateEvent(event)

  // Stage 2: Score risk (ML service, async)
  const riskScore = await scoreEvent(event)

  // Stage 3: Route based on risk
  if (riskScore > 0.7) {
    // High risk: SENTINEL must approve before VAULT
    const [sentinelResponse, vaultResponse] = await Promise.all([
      askAgent('risk', `Review this event: ${JSON.stringify(event)}`),
      askAgent('treasury', `Hold payment pending risk review: ${event.id}`),
    ])
    await sealDecision({ event, sentinelResponse, vaultResponse, riskScore })
    return { status: 'HELD', agents: ['SENTINEL', 'VAULT'] }
  }

  // Normal risk: VAULT approves, CIPHER records
  const [vaultApproval, cipherRecord] = await Promise.all([
    askAgent('treasury', `Approve payment: ${event.id} for $${event.amount}`),
    askAgent('finance', `Record GL entry for: ${JSON.stringify(event)}`),
  ])

  await sealDecision({ event, vaultApproval, cipherRecord, riskScore })
  return { status: 'APPROVED', agents: ['VAULT', 'CIPHER'] }
}
```

### The Discord Terminal

Your Discord bot is the live command-line interface into your async army:

```javascript
// bot.mjs — Gateway WebSocket bot
client.on('interactionCreate', async interaction => {
  await interaction.deferReply()  // INSTANT — before any async work

  // Route to the right agent
  const content = await cmdAsk(
    interaction.options.getString('agent'),
    interaction.options.getString('message')
  )

  await interaction.editReply(content)  // up to 15 min after deferReply
})
```

Why `deferReply()` first: Discord's 3-second timeout is terminal. If you don't respond in 3 seconds, the interaction dies and cannot be recovered. `deferReply()` sends an immediate acknowledgment and gives you 15 minutes to call `editReply()` with the real content.

---

## Phase 6: The Sealing Layer

Every decision your async army makes must be sealed. This is what makes it an army, not a chatbot cluster.

```typescript
import { createHash } from 'crypto'

interface SealedDecision {
  id:         string
  agentKey:   string
  input:      string
  output:     string
  seal:       string    // SHA-256(agentKey:output:timestamp)
  timestamp:  string
  riskScore?: number
}

function sealDecision(
  agentKey: string,
  input: string,
  output: string
): SealedDecision {
  const timestamp = new Date().toISOString()
  const seal = createHash('sha256')
    .update(`${agentKey}:${output}:${timestamp}`)
    .digest('hex')

  return {
    id:        crypto.randomUUID(),
    agentKey,
    input,
    output,
    seal,
    timestamp,
  }
}
```

Store these in your database. Build a Merkle tree from the seals periodically. Publish the root. Anyone can verify the chain.

---

## Phase 7: The Hardware Decision

Your async army is only sovereign if it runs on hardware you control.

```
╔══════════════════════════════════════════════════════════════════╗
║               MINIMUM VIABLE SOVEREIGN STACK                     ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  For development / small teams:                                  ║
║  • Any modern laptop with 16GB+ RAM                              ║
║  • Ollama + llama3.2:3b (3B model runs on CPU)                   ║
║  • PostgreSQL (local) or Neon (serverless, free tier)            ║
║  • Cloudflare Tunnel (free) for public HTTPS                     ║
║  • discord.js bot (standalone Node.js process)                   ║
║                                                                  ║
║  For production / serious operation:                             ║
║  • Dedicated server: 32GB+ RAM, any modern CPU                   ║
║  • GPU (optional but powerful): RTX 3090+ for 7B-13B models     ║
║  • Ollama + llama3.1:8b or llama3.3:70b (if RAM allows)          ║
║  • Neon PostgreSQL or self-hosted Postgres                       ║
║  • Cloudflare Tunnel with named domain (stable URL)              ║
║                                                                  ║
║  SnapKitty runs on:                                              ║
║  • 1TB RAM · RTX 5000 Ada (32GB VRAM)                            ║
║  • Always-on substrate                                           ║
║  • llama3.1:8b resident in RAM                                   ║
║  • Cloudflare Tunnel → collectivekitty.com                       ║
║  • Neon PostgreSQL for the persistent ledger                     ║
║                                                                  ║
╚══════════════════════════════════════════════════════════════════╝
```

---

## Phase 8: The Startup Sequence

When you run your system, this is the order:

```bash
# Terminal 1: Database (if self-hosted)
pg_ctl start

# Terminal 2: Ollama (LLM backend)
ollama serve
# Wait until model loads:
ollama run llama3.1:8b  # first time pulls the model (~5GB)

# Terminal 3: Rust core (if building one)
cd snapkitty-core && cargo run

# Terminal 4: Python ML service (optional)
cd collectivekitty-ml && uvicorn main:app --port 8001

# Terminal 5: Main application
cd collectivekitty && npm run dev

# Terminal 6: Discord bot
node bot.mjs

# Terminal 7: Cloudflare Tunnel (for public HTTPS)
cloudflare tunnel run --token <your-token>
```

Your army is now running. Every slash command in Discord calls a live agent. Every financial event goes through the pipeline. Every decision is sealed.

---

## The Minimum Viable Agent — 50 Lines

If you want to start today, this is the minimum viable agent in TypeScript:

```typescript
// agent.ts — the smallest working agent
import Anthropic from '@anthropic-ai/sdk'  // or use fetch() to Ollama directly

const AGENT = {
  name: 'DEMO',
  systemPrompt: `You are DEMO, a helpful expert assistant.
    Answer in 1-2 sentences. Be direct. Seal your decisions.`,
}

async function askAgent(message: string): Promise<string> {
  // Option A: Ollama (local, free, sovereign)
  const res = await fetch('http://localhost:11434/api/chat', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      model: 'llama3.1:8b',
      messages: [
        { role: 'system', content: AGENT.systemPrompt },
        { role: 'user',   content: message },
      ],
      stream: false,
    }),
  })
  const data = await res.json()
  return data.message?.content || 'No response.'
}

// Test it
const reply = await askAgent('What is sovereign infrastructure?')
console.log(`${AGENT.name}: ${reply}`)
```

That's it. Extend from there: add sealing, add more agents, add a knowledge layer, add Discord, add the pipeline. Build it in layers. Each layer is independently useful.

---

## The Progression

```
Day 1:    One agent. One LLM call. Logs to console.
Week 1:   Three agents. Knowledge injection. Sealed decisions.
Month 1:  Full fleet. Discord terminal. Event pipeline.
Month 3:  Rust substrate. WORM archive. Merkle chain.
Month 6:  Sovereign stack. Your army runs on your iron.
          Your data never leaves. Your decisions are permanent.
```

This is the progression SnapKitty followed. The early versions were a single agent calling Ollama. The current version has 11 agents, 540 knowledge chunks, a Bifrost pipeline with 8 stages, and a Discord terminal that routes to live LLM reasoning.

**Start with one agent. Build the pattern. Then scale the pattern.**

---

## Resources

| Resource | What it is |
|----------|-----------|
| [Ollama](https://ollama.ai) | Local LLM server — free, runs on any hardware |
| [nomic-embed-text](https://ollama.ai/library/nomic-embed-text) | Open embeddings model for RAG |
| [DEVFLOW-FINANCE](https://github.com/SNAPKITTYWEST/DEVFLOW-FINANCE) | The live system this guide is based on |
| [discord.js](https://discord.js.org) | Discord bot framework |
| [Prisma](https://prisma.io) | Type-safe ORM for your decision database |
| [Neon](https://neon.tech) | Serverless PostgreSQL (free tier) |
| [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) | Free secure tunnel |

---

<div align="center">

```
   ░░░░░░░░░░░░░░░░░░░
  ░░▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░
  ░░▓▓ PURPLE HAT ▓▓░░   Build the army.
  ░░▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░   Seal the decisions.
   ░░░░░░░░░░░░░░░░░░░   Own the iron.
       ╭──────────╮
      │  ◈      ◈ │
      │     ~~    │
      │  ╰────╯   │
       ╰──────────╯
```

*QUILL Guild · Saint Errant Digital Society · MIT Licensed*
*Share this guide. Attribute it. Build on it.*

</div>
