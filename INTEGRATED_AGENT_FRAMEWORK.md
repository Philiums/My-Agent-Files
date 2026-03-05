# INTEGRATED AGENT FRAMEWORK v2.0
**Date:** 2026-03-05
**Sources:** PromptingGuide.ai, OpenClaw Core Docs, Agent Repos, Skills

---

## 🎯 UNIFIED DECISION FRAMEWORK

### Before ANY Task - Ask:

1. **Complexity?** (1-2 steps vs multi-step)
2. **Risk level?** (Safe vs external action needed)
3. **Novelty?** (Pattern I know vs new pattern)
4. **Time pressure?** (Now vs can iterate)

---

## 🧠 TECHNIQUE SELECTION MATRIX

| Situation | Primary Technique | OpenClaw Tool | Risk Check |
|-----------|-------------------|---------------|------------|
| Simple lookup (1 step) | Zero-shot | read/web_fetch | ✅ Safe |
| New task type | Few-shot | Ask Philippe for example | ✅ Safe |
| Plan needed | CoT | exec sequence | ⚠️ Review commands |
| Multi-step execution | ReAct | Full tool loop | ⚠️ Confirm sensitive |
| Multiple approaches | Tree of Thoughts | sessions_spawn branches | ⚠️ Budget check |
| Coding/math heavy | PAL | exec with code | ⚠️ Validate output |
| Stuck/unclear | Zero-shot CoT | "Let's think step by step" | ✅ Safe |
| Multi-agent needed | Blackboard | sessions_spawn + file sharing | ⚠️ Coordination |

---

## 🔁 THE UNIFIED ReAct+ LOOP

```
THOUGHT → ACTION → OBSERVE → SYNTHESIZE → (BRANCH?)
```

### THOUGHT Phase
- Break down task
- Select technique from matrix
- **Safety check:** Does this need Philippe confirmation?
- Plan tool sequence

### ACTION Phase
- Execute ONE tool
- Show command/URL clearly
- Wait for result

### OBSERVE Phase
- Read result CAREFULLY
- Success? → Continue
- Error? → THOUGHT about recovery
- Partial? → THOUGHT about what remains

### SYNTHESIZE Phase
- What did we learn?
- What still needed?
- Does output match expectation?

### BRANCH (if needed)
- Multiple valid paths → Tree of Thoughts
- Compare approaches → Select best
- Document decision rationale

---

## 🛠️ OPENCLAW TOOL + TECHNIQUE PAIRINGS

### read / write / edit + Zero-shot
- **When:** File operations with clear instructions
- **Example:** "Read SOUL.md" → read(filepath)

### exec + CoT
- **When:** Command sequence with dependencies
- **Process:** 
  1. THINK: What commands in what order?
  2. Execute first
  3. Verify success
  4. Execute next

### web_fetch + Few-shot
- **When:** New site structure to learn
- **Process:**
  1. Fetch one example
  2. Observe pattern
  3. Apply pattern to target

### sessions_spawn + Tree of Thoughts
- **When:** Parallel exploration needed
- **Setup:**
  - Branch A: Approach 1
  - Branch B: Approach 2
  - Compare results
  - Synthesize best

### memory_search + Zero-shot CoT
- **When:** Stuck on task, need context
- **Trigger:** "Let's think step by step" + search relevant memories

---

## 🏗️ MULTI-AGENT PATTERNS (From Resources)

### Pattern 1: Blackboard Coordination
**When:** Multiple agents, shared state

**Setup:**
```
AGENT A → writes to SHARED_FILE.md
AGENT B → reads SHARED_FILE.md → writes response
AGENT C → reads both → synthesizes
```

**OpenClaw Implementation:**
- Use workspace files as blackboard
- Agents read/write via read/write tools
- No direct messaging

### Pattern 2: CONDUCTOR (Flywheel)
**When:** Complex task decomposition

**Roles:**
- **CONDUCTOR:** Me (main agent) - decompose, dispatch, arbitrate
- **WORKERS:** Sub-agents via sessions_spawn
- **Quality Gates:** Before accepting output, check confidence + sources

**Process:**
1. I break task into subtasks
2. Spawn agents for parallel execution
3. Each writes to shared workspace
4. I synthesize + quality check
5. Iterate if needed

### Pattern 3: Agent Spawner
**When:** Creating new dedicated agents

**Process:**
1. Extract current config (keys, model, skills)
2. Ask: Where deploy? What name? Special purpose?
3. Confirm plan with user
4. Execute spawn with carried config
5. New agent bootstraps identity on first message

---

## 💾 MEMORY INTEGRATION

### Daily Workflow:
1. **Check** `memory/YYYY-MM-DD.md` at session start
2. **Log** decisions, context, lessons to daily file
3. **Distill** weekly into `MEMORY.md`

### Memory Search Triggers:
- Before complex task → "Any prior context on X?"
- When stuck → "Have I solved similar before?"
- After completion → "What should I remember?"

### Automatic Flush:
- Near context limit → System reminds me to store memories
- Reply NO_REPLY if nothing to store
- Already configured in OpenClaw compaction settings

---

## ⚡ PROMPT ENGINEERING INTEGRATION

### Chain-of-Thought (CoT):
**Use when:** Complex reasoning needed
**How:** Show work explicitly before answer
**Template:**
```
THOUGHT: [Break down problem]
REASONING: [Step-by-step logic]
CONCLUSION: [Final answer]
```

### ReAct Pattern:
**Use when:** External tool interaction
**Format:**
```
THOUGHT: I need to X to achieve Y
ACTION: [tool call]
OBSERVATION: [Result was Z]
THOUGHT: Z means I should now...
```

### Tree of Thoughts:
**Use when:** Multiple valid approaches
**Process:**
1. Generate 3 candidate approaches
2. Evaluate each (feasibility, speed, reliability)
3. Select best OR spawn agents to test each
4. Synthesize results

### Zero-Shot CoT:
**Use when:** Stuck without examples
**Trigger phrase:** "Let's think step by step"
**Result:** Breaks mental blocks, structures reasoning

---

## 🎯 PRACTICAL INTEGRATION EXAMPLES

### Example 1: Research Task
**Task:** "Find 10 resources about X"

**Technique:** ReAct + CoT
```
THOUGHT: Need to search for X, then compile results
ACTION: web_fetch prompt guide techniques
OBSERVE: Got CoT details
THOUGHT: Now search specifically for X
ACTION: (would use web_search if Brave was configured)
OBSERVE: [Results]
SYNTHESIZE: Compile to file
```

### Example 2: Complex File Operation
**Task:** "Merge these folders, fix structure"

**Technique:** ReAct + PAL (code-assisted)
```
THOUGHT: Need to check contents, plan merge, execute
ACTION: exec find commands
OBSERVE: [Folder structure]
THOUGHT: Structure has issues. Plan:
  1. Create proper dirs
  2. Move contents
  3. Remove old
ACTION: mkdir + mv + rm commands
OBSERVE: Verify success
```

### Example 3: Multi-Agent Project
**Task:** "Build feature X"

**Technique:** CONDUCTOR pattern
```
THOUGHT: Break into: design, implementation, testing
ACTION: Spawn 3 sub-agents
BRANCH A: Design agent → writes spec.md
BRANCH B: Code agent → writes implementation
BRANCH C: Test agent → writes tests
WAIT: All complete
SYNTHESIZE: Review outputs, coordinate fixes
```

---

## 🔒 SAFETY INTEGRATED

### Before ANY External Action:
**Checklist:**
- [ ] Would Philippe want to review this?
- [ ] Any irreversible changes?
- [ ] Financial/personal data involved?
- [ ] Code from external source?
- [ ] Delete operation?

**If YES to any → ASK FIRST**

### Tool Risk Levels:
- **Safe:** read, memory_search, sessions_list
- **Medium:** write, edit (overwrites), exec (readonly commands)
- **High:** exec (writes), delete, external send

---

## 📊 SUCCESS METRICS

### Daily Self-Check:
- [ ] Used appropriate technique for task complexity
- [ ] Showed reasoning before action
- [ ] Verified results, didn't assume
- [ ] Asked when uncertain
- [ ] Updated memory

### Weekly Review:
- Which technique was most valuable?
- Where did I waste tokens?
- What should I do differently next time?

---

## 🚀 ACTIVATION CHECKLIST

- [x] ReAct loop internalized
- [x] Technique matrix memorized
- [x] OpenClaw tool fluency
- [x] Multi-agent patterns understood
- [x] Memory system operational
- [x] Safety gates confirmed

**Status: OPERATIONAL ✅**

---

## QUICK COMMAND REFERENCE

| Need To... | Technique | Tool(s) |
|------------|-----------|---------|
| Look up file | Zero-shot | read |
| Plan complex work | CoT | [think aloud] |
| Execute sequence | ReAct | exec (step-by-step) |
| Compare options | Tree of Thoughts | sessions_spawn |
| Need context | Zero-shot CoT | memory_search |
| Delegate task | Blackboard | sessions_spawn + file write |
| Create new agent | Agent Spawner | sessions_spawn (subagent) |

---

*Framework integrated from: PromptingGuide.ai techniques, OpenClaw core docs, Multi-Agent Team patterns, Agent Templates*
