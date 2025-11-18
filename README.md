# Mortgage AI Call Center

This repository contains the reference implementation for an AI-powered call center
designed for the mortgage industry. The core idea is to replace Tier-0 / Tier-1
call center workflows with AI agents that can handle inbound calls, answer common
questions, retrieve loan status, and escalate to human operators when needed.

---

## High-Level Goals

- **Mortgage-focused AI agents** that understand call flows, compliance boundaries,
  and common borrower questions.
- **Real-time voice interaction** using low-latency STT (speech-to-text) and TTS
  (text-to-speech).
- **Tool-augmented LLMs** that can safely call into LOS/CRM systems and knowledge
  bases to answer account-specific and general mortgage questions.
- **Compliance-aware architecture** with audit logging, PII handling, and
  configurable policy guardrails.
- **Deployable as a product** into lender environments (cloud or hybrid/on-prem).

---

## Architecture Overview

The system is organized into several logical layers:

1. **Telephony & Call Control**
   - Handles inbound calls from PSTN/SIP.
   - Manages call routing, media streaming, and basic call lifecycle.

2. **Real-Time Speech (STT/TTS)**
   - Streaming speech-to-text to capture caller utterances.
   - Low-latency text-to-speech to respond in natural voice.

3. **Agent Orchestration ("Agent Brain")**
   - Maintains conversation state.
   - Decides what to do next: call LLM, call an integration, ask a clarifying question,
     escalate to a human, or end the call.
   - Enforces high-level policies and guardrails.

4. **LLM + Tools Layer**
   - Large Language Model(s) orchestrated via tool-calling.
   - Tools for:
     - Knowledge retrieval (RAG) over mortgage FAQs, scripts, policies.
     - Loan status and account lookups via LOS/CRM.
     - Scheduling, ticketing, and escalation workflows.

5. **Integrations**
   - Connectors for LOS (e.g., Encompass, MeridianLink), CRM (e.g., Salesforce),
     and case management systems.
   - Abstraction layer so each partner’s systems can be plugged in with minimal
     changes to the core agent logic.

6. **Observability & Compliance**
   - Call recording, transcripts, redacted logs.
   - Audit trail of LLM prompts, tool calls, and decisions.
   - Dashboards for QA teams to review and tag calls.

For full diagrams and more detailed descriptions, see:
[`docs/architecture/high-level-architecture.md`](docs/architecture/high-level-architecture.md).

---

## Repo Structure

```text
services/
  agent-gateway/            # Entry point for telephony + real-time audio
  conversation-orchestrator/# Agent brain / state machine
  llm-router/               # LLM + tools abstraction layer
  integrations/             # LOS / CRM adapters
  observability/            # Logging, audit, QA APIs
docs/
  architecture/
    high-level-architecture.md
infra/
  k8s/                      # (future) Deployment manifests
  terraform/                # (future) Infra-as-code
