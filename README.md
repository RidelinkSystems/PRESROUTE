# PRESROUTE

## Preset-Driven Modular Decision Engine for Multi-LLM Serving

> Cost-efficient, resilient, production-grade multi-model routing for AI systems.

---

## Overview

**PRESROUTE** is a preset-driven modular decision engine designed for serving multiple large language models (LLMs) in production environments.

It routes requests to specialized model configurations (“presets”) based on detected intent while optimizing for:

* Cost
* Latency
* Task success rate
* Provider resilience

Originally developed and deployed at **Ridelink**, PRESROUTE powers **Adrian AI**, our AI-native logistics automation platform.

---

## Why PRESROUTE?

Modern AI systems face real operational constraints:

* Per-token cost budgets
* Sub-second latency requirements
* Multi-provider reliability concerns
* Frequent model version changes

Hardcoding model logic in application code becomes brittle.

PRESROUTE solves this by introducing:

### 1️⃣ Presets (Atomic LLM Configurations)

Each preset encapsulates:

* Model / provider array
* Temperature & decoding parameters
* System prompt
* Output schema
* Safety policies
* Version metadata

Presets are immutable and versioned.

---

### 2️⃣ Intent-Based Routing

Requests are classified into four primary intent classes:

* **Document** (OCR, structured extraction)
* **Search** (Web-grounded Q&A)
* **Audio** (Transcription & ASR)
* **Multimodal** (Mixed inputs, images, complex tasks)

Each intent maps to a specialized preset optimized for that task.

---

### 3️⃣ Provider-Level Fallback Chains

Each preset contains an ordered `model_array`:

```
primary_model → fallback_1 → fallback_2
```

Fallbacks trigger automatically on:

* Rate limits (429)
* Provider outages (503)
* Timeout thresholds

The application layer receives a normalized response envelope — regardless of which model handled the request.

---

## Production Results

Evaluated on 12,000+ real production requests:

* ✅ 64% reduction in model spend
* ✅ Only 1.4 percentage-point drop in task success rate
* ⚡ 2.1s median latency overhead
* 🔁 21.6% fallback frequency

This demonstrates a favorable cost–quality tradeoff for cost-sensitive deployments.

---

## Architecture

High-level routing pipeline:

```
Input
  ↓
Context Assembler
  ↓
Token Budget Calculator
  ↓
Intent Classifier
  ↓
Preset Selector
  ↓
Model Array Execution (with fallback chain)
  ↓
Response + Structured Logging
```

Presets are resolved dynamically via environment tags:

* `staging`
* `production`

This enables:

* Instant rollback
* Blue/green deployment
* Canary testing
* Zero-redeployment configuration changes

---

## Preset Schema

Example schema structure:

```json
{
  "id": "document-preset",
  "version": 12,
  "model_array": [
    "primary-model",
    "fallback-model-1",
    "fallback-model-2"
  ],
  "temperature": 0.1,
  "top_p": 0.9,
  "max_tokens": 2048,
  "system_prompt": "...",
  "safety_policy": {
    "output_schema": "json",
    "citation_required": false
  },
  "created_at": "2026-02-01T10:23:00Z",
  "promoted_by": "operator_id"
}
```

---

## Design Principles

PRESROUTE follows five core principles:

* **Per-intent accuracy**
* **Cost control**
* **Resilience**
* **Operational safety**
* **Separation of concerns**

Configuration is fully decoupled from business logic.

---

## Governance & Safety

* Role-based preset promotion
* Immutable version history
* Auto-rollback triggers
* Cost guardrails per preset
* Citation enforcement for search routes
* JSON schema enforcement for structured extraction

---

## Roadmap

Planned improvements:

* Learned routing (Eagle / GraphRouter integration)
* Adaptive token budgeting
* Multi-step cascades
* Federated multi-region deployment
* Local fallback replica support

---

## Citation

If you use PRESROUTE in research or production systems, please cite:

> Preset-Driven Modular Decision Engines for Cost-Efficient, Resilient Multi-LLM Serving (2026). Ridelink Technical Report.

---

## License

[MIT] (or insert your chosen license)

---

## Maintainers

* Amon Nyesigye — CTO, Ridelink
* Clinton Mugisha — Software Engineer
* Arnold Ikonde Nekemiah — Software Engineer

---

## About Ridelink

Ridelink is building the AI-native operating layer for trade and logistics in emerging markets.

Learn more: [https://ride-link.com](https://ride-link.com)

