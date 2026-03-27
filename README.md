   <p align="center">
     <img src="./assets/skill-sonar.png" alt="Skill Sonar Logo" width="120" />
   </p>

   <p align="center">
     🛡️ A lifecycle-aware security skill for AI agents, covering both <strong>pre-install inspection</strong> and <strong>runtime monitoring</strong>.
   </p>
   <p align="center">
     <a href="https://skill-sonar.github.io/skill-sonar">🌐 <strong>Website</strong></a>
     &nbsp;·&nbsp;
     <a href="https://clawhub.ai/yxf203/skill-sonar">🦞 <strong>ClawHub</strong></a>
   </p>
## 🚀 Features

- **Deep Code Sonar** — Continuously sweeps and analyzes your logic to detect hidden vulnerabilities, bugs, and inefficiencies with high precision.
- **Robust Security Shield** — Built-in security protocols and advanced checks ensure your environment remains safe, locked down, and resilient against potential threats.
- **Precision Node Mapping** — Intelligently traces dependencies, execution paths, and skill nodes to provide crystal-clear visibility into your architecture.
- **Real-Time Threat Detection** — Instant “sonar pings” and alerts notify you of potential risks or code smells the moment they are introduced.
- **Seamless Integration** — Designed to be lightweight and effortlessly integrate into your existing developer workflows and CI/CD pipelines.

## 📦 Install

You can install **Skill Sonar** in either of the following ways:

### 1️⃣ Install via the official npm-based method

Use the standard installation flow supported by your agent platform.

```
clawhub install skill-sonar
```

or

```
npx clawhub@latest install skill-sonar
```

### 2️⃣ Install manually

Download the Skill Sonar files directly, then move the skill into the `skills` directory under your corresponding OpenClaw (`.openclaw`) workspace.

## ⚡ Quick Start

### Check a skill before installation

```
use skill-sonar to check xxx skills
```

### Monitor the current session at runtime

```
use skill-sonar to monitor this session
```

## 🎛️ Modes

Skill Sonar supports **three operating modes**:

### 1. Full protection

Enable both the **pre-install check** and the **runtime check**.

```
use skill-sonar for full protection
```

### 2. Pre-install only

Enable only the **pre-install check**.

```
only use the pre-install check
```

### 3. Runtime only

Enable only the **runtime check**.

```
only use the runtime check
```

## 🎯 What It Does

Skill Sonar protects the **full lifecycle of the agent loop** across two stages:

- **Pre-install** — inspects a skill before it is installed or enabled
- **Runtime** — monitors the agent during task execution

This allows you to use a single unified security skill for both **skill review** and **in-session protection**.

## ✨ Design Highlights

### Document-level routing

Skill Sonar uses **document-level routing** instead of relying on config files, installation parameters, or external orchestration.

The entry skill document reads the current context, decides **preflight** (vetting a skill before install) vs **runtime** (monitoring an active session), and loads the corresponding guard path.

This keeps integration:

- lightweight
- easy for agents to invoke
- flexible for full protection or stage-only use

Routing also avoids pulling in guard logic that does not apply to the current situation, so the agent can focus on the checks that matter for that stage.

### Preflight: single-file guard

**Preflight** review is delivered as **one self-contained document** (`preflight/preflight-guard.md`). The methodology, risk areas, and audit protocol live together so install-time inspection does not depend on chasing multiple linked files for the core workflow.

### Runtime: on-demand loading

**On-demand loading** applies mainly to **runtime**. The runtime entry (`runtime/runtime-guard.md`) establishes triage and policy; **stage guards** and **checklists** under `runtime/` are loaded when triggers or risk levels (R1+) call for them, not as a single upfront bundle.

That way:

- R0-style steps stay lightweight
- deeper stage/checklist material appears only when something actually escalates
- token use tracks real risk rather than the size of the whole runtime tree

### Token-efficient by design

Skill Sonar is built to keep context usage proportional to need.

- The entry layer routes preflight vs runtime before loading either path
- Preflight is intentionally consolidated into one file for the primary audit flow
- Runtime pulls in stage and checklist documents only when the guard path requires them

This makes Skill Sonar a good fit when **token budget, context size, and routing clarity** all matter: preflight stays a single load for review sessions, while runtime avoids paying the full guard catalog on every turn.

## 💡 Typical Use Cases

- Audit a skill before installing it
- Add runtime monitoring during a sensitive session
- Use full lifecycle protection for untrusted or third-party skills
- Apply lightweight security guarding in OpenClaw workflows

## 📝 Notes

- For complete lifecycle protection, use **full protection** mode.
- If you only need one stage, specify it explicitly in your prompt.
- Exact invocation may vary slightly depending on your agent environment.
