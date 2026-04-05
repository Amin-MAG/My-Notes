---
title: OpenFang
tags:
  - ai
  - agents
  - automation
---
# OpenFang Learning Summary

## What is OpenFang?

OpenFang is an **Agent Operating System** — not a chatbot framework. It runs autonomous agents that do work for you on schedules, without you having to prompt them. The entire system compiles to a single ~32MB Rust binary.

**Key mental shift:** Traditional AI tools are reactive (you type, they respond). OpenFang agents are proactive — they run on schedules, monitor things, build knowledge over time, and deliver results to wherever you are (Telegram, dashboard, etc).

**One instance is enough.** A single OpenFang daemon manages all your agents, Hands, channels, workflows, and memory simultaneously.

---

## Why OpenFang Instead of Using the Model Directly?

When you use Claude.ai or call the Anthropic API directly, you get a stateless request/response loop. Every conversation starts from zero. The model has no memory of what you told it yesterday, no ability to run code on your behalf, no way to reach the web, and no way to do anything while you're not actively typing.

OpenFang sits between you and the model and adds everything that's missing.

### 1. Memory That Actually Persists

With a raw API call, when the conversation ends, everything is gone. Next session, you start over. You re-explain your context, your preferences, your project details — every time.

With OpenFang, agents remember. A venue-scouting agent remembers every venue it has already found across all past sessions. A homelab assistant remembers your cluster topology, your constraints, your past decisions. It builds up knowledge over time and gets more useful the longer you run it — not because the model itself learned anything, but because the agent accumulated and retrieves relevant context automatically before every response.

### 2. Agents That Work While You're Not There

A raw API call requires you to be present. You type, it responds, done. If you want it to check something tomorrow morning, you have to remember to ask it tomorrow morning.

OpenFang agents run on schedules. You configure a venue scout once, and every morning it searches for new venues, enriches the data, stores results to memory, and pushes a summary to your Telegram — all without you doing anything. This is the difference between a tool you use and a system that works for you.

### 3. Real Tool Use — Not Simulated

Modern LLMs can describe what they would do to search the web or read a file. OpenFang agents actually do it. When an agent calls `web_search`, it fires a real search engine query. When it calls `web_fetch`, it retrieves the actual live page. When it calls `shell_exec`, it runs the command on your machine. The results come back into the agent's context and it reasons over real data — not hallucinated content.

This is the difference between asking Claude "what mini PCs are on Kijiji right now?" (it will guess) and running an agent that actually fetches Kijiji listing pages and reads them (it will find real listings).

### 4. Multi-Channel Access to the Same Agent

With a raw API, you get one interface — wherever you're making the call. With OpenFang, the same agent is reachable via Telegram, Discord, Slack, the web dashboard, the REST API, and n8n workflows simultaneously. You configure the agent once and access it from anywhere.

More importantly, the agent maintains a **canonical session** across channels — if you tell it something on Telegram, it remembers that context when you continue on the web dashboard.

### 5. Multi-Agent Pipelines

With a raw model, you do one thing per request. If you want to chain tasks — find venues, then research each one, then write outreach emails — you do it manually, copy-pasting between conversations.

With OpenFang, you build a pipeline: a venue-scout agent stores results to shared memory, an outreach-composer agent reads that memory and generates emails, all triggered automatically. The agents coordinate without you being involved at all.

### 6. Security and Control

A raw API call has no guardrails beyond what you put in the prompt. OpenFang adds 16 security layers — a WASM sandbox around every tool call, prompt injection scanning, SSRF protection, capability-based access control (agents can only use the tools you explicitly grant them), and a Merkle hash-chain audit trail of every action taken.

For agents that have shell access and internet connectivity — which is a real attack surface — this matters.

### The Simple Summary

|Raw model (API/Claude.ai)|OpenFang agent|
|---|---|
|Stateless — forgets everything|Persistent memory across sessions|
|You must be present to get anything done|Runs autonomously on schedules|
|Describes what it would do|Actually does it via real tool calls|
|One interface|40 channels simultaneously|
|One task at a time|Multi-agent pipelines|
|No guardrails|16 security layers|

OpenFang is the answer to: _"I want this model to actually do things for me, remember context over time, and work even when I'm not at my keyboard."_

---

## Core Concepts

### Agents

The basic unit of OpenFang. Each agent has:

- A system prompt defining its behavior and expertise
- A set of tools it can use (`web_search`, `web_fetch`, `shell_exec`, etc.)
- Its own persistent memory scoped to that agent
- A model assigned to it (can differ per agent)

Agents are **persistent processes** — not request/response. They stay alive, maintain state, and can run background tasks.

### Hands

Agents on full autopilot. Unlike regular agents that wait for you to type, Hands:

- Run on schedules (e.g. every morning at 9am)
- Execute multi-phase operational playbooks autonomously
- Build knowledge graphs over time
- Report metrics and results to the dashboard

**7 built-in Hands:** Researcher, Collector, Predictor, Lead, Clip, Twitter, Browser

Each Hand bundles a `HAND.toml` manifest, a 500+ word expert system prompt, a `SKILL.md` knowledge file, and dashboard metrics — all compiled into the binary.

### Tools

What agents can _do_. Every tool call runs inside a **WASM sandbox** with fuel metering — a security layer that prevents runaway code. Key tools:

|Tool|What it does|
|---|---|
|`web_search`|Search the web|
|`web_fetch`|Fetch a specific URL|
|`memory_store`|Persist a fact to long-term memory|
|`memory_recall`|Recall relevant facts from past sessions|
|`knowledge_add_entity`|Add a node to the knowledge graph|
|`knowledge_add_relation`|Link two knowledge graph nodes|
|`knowledge_query`|Query the knowledge graph|
|`shell_exec`|Run shell commands (sandboxed)|
|`event_publish`|Trigger another agent|
|`schedule_create`|Create a scheduled task|

### Channels

How agents receive and send messages from the outside world. 40 adapters built in — Telegram, Discord, Slack, Gotify, webhooks, email, and more. Each channel routes messages to a configured default agent. Per-channel model overrides are supported.

**One channel = one default agent.** To have multiple agents accessible via the same platform, use multiple bot tokens (one per agent) or a router agent that delegates via `event_publish`.

### Skills

Markdown knowledge files injected into an agent's context at runtime. They are **not tools** — they are reference knowledge that shapes how the agent reasons.

Per-agent skill files:

|File|Purpose|
|---|---|
|`SOUL.md`|Agent personality and core values|
|`IDENTITY.md`|Agent's role and identity|
|`USER.md`|Info about the user — name, location, preferences|
|`TOOLS.md`|Instructions for how to use tools correctly|
|`MEMORY.md`|What to persist across sessions|
|`AGENTS.md`|Info about other agents it can collaborate with|
|`HEARTBEAT.md`|Instructions for autonomous background ticks|

60 expert skills ship bundled with OpenFang (Kubernetes, Docker, GitHub, security audit, etc). You can write custom skills and publish them to FangHub.
---

## Key CLI Reference

```bash
# Daemon
openfang start                    # start daemon
openfang stop                     # graceful stop
pkill -f openfang                 # force kill — use this when config changes don't apply
openfang status                   # check daemon status
openfang config show              # show currently loaded config
openfang logs --follow            # tail live logs
openfang doctor                   # run diagnostics

# Agents
openfang agent list
openfang agent spawn <file.toml>
openfang agent kill <ID>
openfang agent chat <ID>

# Hands
openfang hand list
openfang hand activate <hand>
openfang hand deactivate <hand>
openfang hand status <hand>
openfang hand pause <hand>
openfang hand resume <hand>

# Triggers / Scheduling
openfang trigger list
openfang trigger create --agent <name> --cron "<cron>" --message "<msg>"
openfang trigger delete <ID>

# Channels
openfang channel list
openfang channel setup <channel>  # interactive wizard

# Skills
openfang skill list
openfang skill install <source>
openfang skill search <query>
```

---

## See Also
- [Artificial Intelligence](Artificial-Intelligence.md) — OpenFang is built on top of LLMs (Claude, GPT, etc.) and adds autonomy, memory, and tool use on top of foundation models
- [Computer Science](Computer-Science.md) — Core CS concepts underpin OpenFang: scheduling, process management, sandboxing (WASM), embedding-based vector search, and knowledge graphs
- [Distributed Systems](Distributed-Systems.md) — Agent-to-agent communication, event-driven pipelines, and the A2A protocol follow distributed systems patterns
- [Automation](Automation.md) — OpenFang turns reactive AI into proactive automation via cron-based scheduling and autonomous Hands
- [Architecture Patterns](Architecture-Patterns.md) — Multi-agent pipelines, fan-out workflows, and event-publish patterns align with microservice and event-driven architectures
- [LLM Streaming](LLM-STREAMING.md) — Relevant to how agents stream responses back through channels
- [Kubernetes](Kubernetes.md) — OpenFang ships bundled Kubernetes skills and can manage cluster operations via agents with `shell_exec`