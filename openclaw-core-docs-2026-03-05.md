# OpenClaw Core Documentation Summary
**Date:** 2026-03-05
**Scope:** Agent Runtime, Multi-Agent Routing, Session Tools, Memory, Skills

---

## AGENT RUNTIME

**Workspace (Required):**
- Default: `~/.openclaw/workspace`
- Agent's **only** working directory for tools
- Per-agent workspaces: `~/.openclaw/workspace-<agentId>`

**Bootstrap Files (Auto-Injected):**
- `AGENTS.md` — operating instructions + memory
- `SOUL.md` — persona, boundaries, tone
- `TOOLS.md` — tool notes and conventions
- `BOOTSTRAP.md` — one-time first-run ritual (auto-deleted)
- `IDENTITY.md` — agent name/vibe/emoji
- `USER.md` — user profile

**Sessions:**
- Stored at: `~/.openclaw/agents/<agentId>/sessions/<SessionId>.jsonl`
- Stable session IDs chosen by OpenClaw
- Queue modes: `steer` | `followup` | `collect`

---

## MULTI-AGENT ROUTING

**Key Concept:** Multiple isolated agents (workspace + agentDir + sessions) in one Gateway

**Routing via Bindings:**
```json5
{
  agents: {
    list: [
      { id: "main", workspace: "~/.openclaw/workspace" },
      { id: "coding", workspace: "~/.openclaw/workspace-coding" }
    ]
  },
  bindings: [
    { agentId: "main", match: { channel: "whatsapp", accountId: "personal" } },
    { agentId: "coding", match: { channel: "whatsapp", accountId: "work" } }
  ]
}
```

**Precedence (most-specific wins):**
1. `peer` match (exact DM/group)
2. `parentPeer` match (thread inheritance)
3. `guildId + roles` (Discord)
4. `guildId` (Discord)
5. `teamId` (Slack)
6. `accountId` match
7. Channel-level match
8. Fallback to default agent

**Per-Agent Sandbox & Tools:**
```json5
{
  agents: {
    list: [{
      id: "family",
      sandbox: { mode: "all", scope: "agent" },
      tools: { allow: ["read", "exec"], deny: ["write", "edit", "browser"] }
    }]
  }
}
```

---

## SESSION TOOLS

Available tools for cross-session operations:

**sessions_list:**
- Filter by `kinds`, `activeMinutes`, `limit`
- Returns: key, kind, channel, displayName, updatedAt, model, tokens

**sessions_history:**
- Fetch transcript for one session
- `sessionKey` or `sessionId`
- `includeTools` flag

**sessions_send:**
- Send message to another session
- `timeoutSeconds: 0` = fire-and-forget
- `timeoutSeconds > 0` = wait for reply
- Max ping-pong turns: `session.agentToAgent.maxPingPongTurns` (default 5)

**sessions_spawn:**
- Spawn sub-agent in isolated session
- Auto-announces result back
- `mode: "run"` (one-shot) or `mode: "session"` (persistent, requires thread)
- `cleanup: "delete" | "keep"`
- `sandbox: "inherit" | "require"`

**Visibility Levels:**
- `self` — only current session
- `tree` — current + spawned (default)
- `agent` — any session for this agent
- `all` — cross-agent (requires tool enablement)

---

## MEMORY SYSTEM

**Files:**
- `memory/YYYY-MM-DD.md` — daily logs (append-only)
- `MEMORY.md` — curated long-term memory (main session only)

**Tools:**
- `memory_search` — semantic recall over indexed snippets
- `memory_get` — read specific file/line range

**Auto-Flush (Pre-Compaction):**
- Triggers when near token limit
- Silent agentic turn reminds model to store memories
- Config: `agents.defaults.compaction.memoryFlush`

**Vector Search:**
- Hybrid: Vector similarity + BM25 keyword
- Providers: openai, gemini, voyage, mistral, ollama, local
- Local: uses node-llama-cpp, auto-downloads GGUF models
- SQLite with sqlite-vec extension for acceleration

**Advanced Features:**
- **MMR re-ranking** — reduce redundant results (diversity)
- **Temporal decay** — boost recent memories (half-life configurable)
- **Embedding cache** — avoid re-embedding unchanged text
- **Session memory** — opt-in indexing of session transcripts

---

## SKILLS

**Three Locations (precedence high → low):**
1. `<workspace>/skills` — per-agent
2. `~/.openclaw/skills` — shared across agents
3. Bundled skills — shipped with install

**SKILL.md Format:**
```markdown
---
name: skill-name
description: What this skill does
metadata:
  { "openclaw":
    { "requires": { "bins": ["uv"], "env": ["API_KEY"], "config": ["browser.enabled"] },
      "primaryEnv": "API_KEY" }
  }
---
Instructions here...
```

**Gating:**
- `requires.bins` — binaries on PATH
- `requires.env` — env vars present
- `requires.config` — config paths truthy
- `os` — platform restriction

**Config Override:**
```json5
{
  skills: {
    entries: {
      "skill-name": {
        enabled: true,
        apiKey: "secret",
        env: { API_KEY: "value" },
        config: { endpoint: "..." }
      }
    }
  }
}
```

**ClawHub:** https://clawhub.com — public skills registry

---

## MODEL REFS

**Format:** `provider/model`
- Split on **first** `/`
- OpenRouter-style: `openrouter/moonshotai/kimi-k2`
- Omit provider → uses default provider

---

## QUICKEST CONFIG

Minimal viable `~/.openclaw/openclaw.json`:
```json5
{
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",
      model: "openai/gpt-4o"
    }
  },
  channels: {
    whatsapp: { allowFrom: ["+15555550123"] }
  }
}
```

---

## COMMANDS

```bash
# Setup
openclaw onboard --install-daemon

# Add agent
openclaw agents add work

# Channel login
openclaw channels login --channel whatsapp --account work

# List agents with bindings
openclaw agents list --bindings

# Gateway
openclaw gateway --port 18789

# Skills
clawhub install <skill>
clawhub update --all
```

---

## KEY INSIGHTS FOR AGENTS

1. **Bootstrap files are injected every session** — keep them lean
2. **Workspace is cwd** — relative paths resolve there
3. **Bindings control routing** — most specific wins
4. **Session tools enable cross-agent coordination** — but sandboxed sessions default to tree visibility
5. **Memory is plain Markdown** — semantic search overwrites manual recall
6. **Skills are filtered at load time** — env/config/bin gates must pass
7. **Model refs split on first `/`** — include provider prefix for OpenRouter
8. **Queue modes matter** — `steer` interrupts, `collect` batches

---
*Compiled from OpenClaw docs: agent.md, multi-agent.md, session-tool.md, memory.md, skills.md*
