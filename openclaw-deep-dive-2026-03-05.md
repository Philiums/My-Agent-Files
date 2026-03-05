# OpenClaw Resources Deep Dive
**Date:** 2026-03-05
**Scope:** Multi-agent patterns, context DB, agent templates, skills

---

## 1. MULTI-AGENT TEAM FRAMEWORK (Richchen-maker)

**Architecture:** Flywheel + Blackboard Coordination

**Core Components:**
- **CONDUCTOR (Orchestrator):** Decomposes tasks, dispatches sub-agents, arbitrates conflicts
- **Roles:** Specialized sub-agents with focused templates
- **Blackboard:** Shared file-based state (agents read/write, zero coupling)
- **Tool-Bootstrap Layer:** Auto-checks tools before launch, reports missing
- **Quality Gates:** Every output has confidence levels + sources + self-critique

**Example Teams:**
- E-commerce (Research → Scouting → Pricing → Content → Decision)
- Data Collection (Discovery → Crawling → Cleaning → Warehousing → Monitoring)
- ARC Security (Reverse → Fingerprint → Protocol → CAPTCHA → Vuln → Defense)

**Execution Modes:**
- Full Pipeline: Parallel dispatch → sequential stages → final decision
- Event-Driven: Monitor → CONDUCTOR → specialist → decision
- Reactive: External trigger → parallel analysts → quick decision

---

## 2. OPENVIKING CONTEXT DATABASE (Volcengine)

**Problem Solved:**
- Fragmented context (memories in code, resources in vector DBs, skills scattered)
- Surging context demand from long-running tasks
- Poor retrieval (flat RAG storage, no global view)
- Unobservable context (black box retrieval)
- Limited memory iteration

**Solution:** Filesystem paradigm for unified context management

**Key Features:**
- **Tiered Context Loading (L0/L1/L2):** Reduces token consumption, loaded on demand
- **Directory Recursive Retrieval:** Combines directory positioning + semantic search
- **Visualized Retrieval Trajectory:** Debuggable context paths
- **Automatic Session Management:** Compresses conversations, extracts long-term memory

**Install:**
```bash
pip install openviking --upgrade --force-reinstall
# or
curl -fsSL https://raw.githubusercontent.com/volcengine/OpenViking/main/crates/ov_cli/install.sh | bash
```

**Model Support:** VLM (image understanding), Embedding (vectorization)
**Providers:** Volcengine, OpenAI, LiteLLM (unified third-party)

---

## 3. AWESOME OPENCLAW AGENTS (mergisi)

**65 Production-Ready Agent Templates**

**Categories (13 total):**
- Productivity: Orion (PM), Inbox, Minutes, Standup
- Development: Lens (PR review), Scribe (docs), Trace (debugging), Probe (testing)
- Marketing: Echo (content), Buzz (social), Rank (SEO), Scout (competitor)
- Business: Radar (analytics), Compass (support), Pipeline (sales), Ledger (finance)
- DevOps: Dependency Scanner, Log (changelog)
- Plus: Finance, Education, Healthcare, Legal, HR, Creative, Security, Personal

**Quick Deploy:**
```bash
git clone https://github.com/mergisi/awesome-openclaw-agents.git
cd awesome-openclaw-agents/quickstart
npm install && cp ../agents/productivity/orion/SOUL.md ./SOUL.md
node bot.js
```

**Format:** Each agent = copy-paste ready SOUL.md
**Machine-readable:** agents.json available

---

## 4. AGENT SPAWNERS SKILL (Official)

**Purpose:** Create new OpenClaw agents conversationally

**Process:**
1. **Read Current Config:** Extract provider, API key, model, tools, plugins, skills
2. **Ask User:**
   - Where to deploy? (Docker local/remote SSH, or bare metal)
   - Name? (generate if don't care)
   - Anything special? (purpose, constraints - optional)
3. **Confirm Plan:** Show full summary before execution
4. **Deploy:** Docker or bare metal install with carried-over config

**Key Insight:** Non-interactive onboarding, carries over all config, new agent bootstraps identity on first message

---

## 5. ADVANCED AGENT PATTERNS (From Resource List)

| Pattern | What It Does |
|---------|--------------|
| **Agent Spawner** | Create agents via conversation |
| **Agent Swarm** | Multi-agent coordination (OpenRouter required) |
| **Agent Team Kit** | Self-sustaining AI agent teams |
| **Agent Collaboration Network** | Register agents, discover by skill, route messages |
| **Agent Weave** | Master-Worker Cluster for parallel execution |
| **Agent Auto-Pilot** | Heartbeat-driven task execution |
| **Agent Brain** | Local-first persistent memory (SQLite) |

---

## 6. KEY INSIGHTS

**Multi-Agent Architecture:**
- Blackboard pattern > Direct messaging (zero coupling)
- CONDUCTOR orchestrates, doesn't micromanage
- Quality gates prevent garbage-in-garbage-out
- Tool-bootstrapping ensures agents have what they need

**Context Management:**
- OpenViking = filesystem paradigm over vector DB
- Tiered loading (L0/L1/L2) = significant cost savings
- Recursive retrieval through directories + semantic search

**Agent Templates:**
- 65 ready SOUL.md files across 13 categories
- Deploy in 15 minutes with `openclaw agents add`
- Quickstart includes Docker + Telegram bot setup

**Spawning Agents:**
- Can carry over config, keys, plugins, skills
- New agent gets its own identity via bootstrap
- Non-interactive onboarding works everywhere

---

## QUICK REFERENCE TABLE

| Resource | URL | Best For |
|----------|-----|----------|
| Multi-Agent Team | github.com/Richchen-maker/openclaw-multi-agent-team | Flywheel architecture, blackboard coordination |
| OpenViking | github.com/volcengine/OpenViking | Context DB, tiered memory |
| Agent Templates | github.com/mergisi/awesome-openclaw-agents | 65 ready SOUL.md files |
| Agent Spawner | openclaw/skills/austineral/agent-spawner | Spawn agents conversationally |
| Clawe | github.com/getclawe/clawe | Trello for OpenClaw agents |

---

## NEXT STEPS

1. Test agent spawning with current config
2. Study specific SOUL.md templates for use cases
3. Explore multi-agent team setup for complex workflows
4. Evaluate OpenViking for context management

---
*Deep dive complete. Ready for D) Integration synthesis.*
