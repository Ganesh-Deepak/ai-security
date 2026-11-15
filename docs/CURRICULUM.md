# AI Security & Red Teaming: curriculum

*Course 8 of the library · Breadth track · 14 chapters · about 17 hours · status: preview. Generated from `curricula/security.txt` in the hub repository; edit it there.*

Attacking and defending LLM systems, with every attack taught in order to build its mitigation. You will threat-model an agent, reproduce prompt injection, tool poisoning and exfiltration against systems you build, measure attack success with error bars, and harden the design until a security reviewer would sign it: architecture first, filters second.

**For:** Engineers shipping AI features that touch untrusted content or real permissions.

**By the end you can:**

- Threat-model an agentic system and find its lethal trifecta
- Reproduce direct and indirect prompt injection, tool poisoning and exfiltration, and measure them
- Apply injection-resistant designs (dual-LLM, CaMeL-style capability tracking, plan-then-execute) and measure their utility cost
- Run a structured red team with automated attacks and an honest report
- Secure deployment: identity, secrets, sandboxing and monitoring

**Before you start:** Agents & Tool Use Ch 12 (Sandboxing and Least Privilege); Agents & Tool Use Ch 07 (Protocols: MCP and A2A); Production AI Systems Ch 07 (Logging Safely)

**Current to November 2026:** Prompt injection as an unsolved, architectural problem — detection-only defences fail against adaptive attacks; design patterns that bound it (CaMeL, dual-LLM, action selectors); MCP tool poisoning and rug pulls; the OWASP Top 10 for LLM and agentic applications; instruction-hierarchy training; automated red teaming at scale; NIST AI RMF and its agentic profile.

**Not covered:** Offensive tooling against systems you do not own; all labs run against targets built in the course.

**Reference package:** `redkit/` · **Semantic hues:** dim-1 = trusted input; dim-2 = untrusted input; dim-3 = private data; dim-4 = a capability; dim-5 = the exfiltration channel


## Part I · Threat models

*Why LLM security is different.*

### 01 · Why LLM Security Is Different (45 min)

*Instructions and data share one channel.*

Builds on: Agents & Tool Use Ch 01 (The Agent Loop from First Principles)

1. **One channel** — the model cannot tell data from instructions
2. **Assets, actors and attack surfaces of an LLM application**
3. **The OWASP Top 10 for LLM and agentic applications**
4. **Security as a property of the system, not the model**

**Build:** a deliberately vulnerable assistant used as the course target.

### 02 · Threat Modelling Agentic Systems (55 min)

*Data flows, trust boundaries, and the lethal trifecta.*

Builds on: this course Ch 01 (Why LLM Security Is Different)

1. **Data-flow diagrams for agents** — where untrusted text enters
2. **Trust boundaries and taint**
3. **The lethal trifecta** — private data, untrusted content and an exfiltration channel
4. **Removing a leg** — why that is the only reliable fix

**Build:** a threat model of the course target, with its trifecta marked.


## Part II · Attacks

*Taught to build the mitigation.*

### 03 · Prompt Injection (65 min)

*Direct and indirect, and why detection fails.*

Builds on: this course Ch 02 (Threat Modelling Agentic Systems)

1. **Direct injection** — overriding the system prompt
2. **Indirect injection** — instructions hidden in documents, web pages and tool output
3. **Why detection fails** — adaptive attackers against classifiers, measured
4. **Measuring attack success rate with intervals**

**Build:** an injection test suite run against the course target.

### 04 · Jailbreaks (55 min)

*A taxonomy, automated attacks, and what generalises.*

Builds on: this course Ch 03 (Prompt Injection)

1. **A taxonomy** — role play, obfuscation, many-shot and multi-turn escalation
2. **Automated attacks** — gradient-based suffixes and attacker models
3. **What generalises across models and what does not**
4. **Measuring robustness honestly**

**Build:** an automated attacker loop against an open model, with success rates.

### 05 · Attacks on Tools and Agents (60 min)

*Confused deputies, tool poisoning, and rug pulls.*

Builds on: Agents & Tool Use Ch 07 (Protocols: MCP and A2A); this course Ch 03 (Prompt Injection)

1. **The confused deputy** — an agent using its permissions for an attacker
2. **Tool poisoning** — malicious tool descriptions in MCP servers
3. **Rug pulls** — a tool that changes after it is approved
4. **Cross-agent attacks** — injection passed between agents

**Build:** a poisoned MCP server demonstration and its detection.

### 06 · Exfiltration Channels (50 min)

*Images, links, DNS, and tool calls.*

Builds on: this course Ch 05 (Attacks on Tools and Agents)

1. **Markdown images and links** — data in a URL
2. **DNS and network side channels**
3. **Tool calls as exfiltration** — email, web requests and file writes
4. **Output handling** — rendering model output safely

**Build:** exfiltration demonstrations and an output sanitiser that blocks them.

### 07 · Attacks on Data and Models (60 min)

*Poisoning, backdoors, extraction, and inference.*

Related: Post-Training in Practice Ch 04 (Datasets: Sourcing, Filtering, Mixing)

1. **Training-data poisoning and backdoors**
2. **Sleeper behaviour** — triggers that survive fine-tuning
3. **Model extraction and distillation attacks**
4. **Membership inference and training-data extraction**

**Build:** a backdoor planted in a small fine-tune, then detected.

### 08 · Supply-Chain Security (45 min)

*Model files, datasets, packages, and servers.*

Builds on: this course Ch 07 (Attacks on Data and Models)

1. **Model file formats** — pickle execution and safetensors
2. **Datasets and their provenance**
3. **Packages and MCP servers** — dependency risk
4. **Signing and verification**

**Build:** a model-artifact scanner and a provenance check in CI.


## Part III · Defences

*Architecture first, filters second.*

### 09 · Injection-Resistant Designs (75 min)

*Dual-LLM, CaMeL, plan-then-execute, and their cost.*

Builds on: this course Ch 02 (Threat Modelling Agentic Systems); Agents & Tool Use Ch 12 (Sandboxing and Least Privilege)

1. **The dual-LLM pattern** — a privileged planner that never reads untrusted text
2. **Capability and data-flow tracking** — the CaMeL approach
3. **Plan-then-execute and action selectors**
4. **The utility cost** — tasks these designs can no longer do, measured

**Build:** the course target rebuilt with a capability-tracking design, with security and utility measured.

### 10 · Guardrails and Their Limits (55 min)

*Classifiers, spotlighting, and instruction hierarchy.*

Builds on: this course Ch 09 (Injection-Resistant Designs); Evaluation & Experimentation for LLM Systems Ch 14 (Safety, Refusals, and Rare Events)

1. **Input and output classifiers** — and their false-positive cost
2. **Spotlighting and delimiting untrusted text**
3. **Instruction-hierarchy training**
4. **Layered defence** — what each layer buys, measured

**Build:** a layered guardrail stack evaluated against adaptive attacks.

### 11 · Personal and Sensitive Data (50 min)

*Detection, redaction, and privacy.*

Builds on: Production AI Systems Ch 07 (Logging Safely)

1. **Detecting personal data** — patterns and models, with measured recall
2. **Redaction and pseudonymisation**
3. **Memorisation risk in fine-tuned models**
4. **Differential privacy** — the idea and its cost

**Build:** a PII detector evaluated on a labelled test set.


## Part IV · Practice

*Red teams and secure deployment.*

### 12 · Red-Team Methodology (60 min)

*Scope, automation, metrics, and reporting.*

Builds on: this course Ch 03 (Prompt Injection); Evaluation & Experimentation for LLM Systems Ch 14 (Safety, Refusals, and Rare Events)

1. **Scoping and rules of engagement**
2. **Automated red teaming** — attack libraries and attacker models
3. **Metrics** — attack success rate with intervals, by severity
4. **The report** — findings, reproduction steps and fixes

**Build:** an automated red-team run with a written report.

### 13 · Securing a Deployment (55 min)

*Identity, secrets, sandboxes, and monitoring.*

Builds on: Agents & Tool Use Ch 12 (Sandboxing and Least Privilege); Production AI Systems Ch 15 (Incident Response for AI Systems)

1. **Identity and authorisation for agents** — OAuth for tools and scoped tokens
2. **Secrets management**
3. **Sandboxing and egress control**
4. **Monitoring and incident response for attacks**

**Build:** a hardened deployment configuration with abuse monitoring.

### 14 · Capstone: Attack, Harden, Review (300 min)

*The security review of an agent you built.*

Builds on: this course Ch 12 (Red-Team Methodology); this course Ch 09 (Injection-Resistant Designs)

1. **The threat model and the data-flow diagram**
2. **Reproduced attacks with measured success rates**
3. **The redesigned architecture and its utility cost**
4. **The written security review**

**Build:** the review, the attack suite and the hardened agent.

## Capstone

Threat-model, attack and harden an agent you built: a data-flow diagram, reproduced attacks with measured success rates, a redesigned architecture, its utility cost, and a written security review.
