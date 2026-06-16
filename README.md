# TherapyUnlearn-140

A benchmark for evaluating selective machine unlearning in multi-agent psychotherapy systems. Developed as part of the Bachelor's thesis *"Selective Machine Unlearning in Multi-Agent Psychotherapy Systems"* (M. Cheung, 2026).

---

## Overview

TherapyUnlearn-140 evaluates whether gradient ascent on LoRA adapter weights can selectively erase specific participant records from fine-tuned agents while preserving performance on retained participants. The benchmark is built on the [DAIC-WOZ corpus](https://dcapswoz.ict.usc.edu/) and structured around a three-agent LangGraph pipeline (Dialogue, Monitoring, Planning), each fine-tuned on LLaMA-3-8B-Instruct with LoRA.

Forget/retain splits are defined in `splits.json`. The forget set comprises 18 high-severity participants (PHQ-8 ≥ 15).

---

## Repository Structure

```
TherapyUnlearn-140/
├── splits.json                         # Forget/retain participant split definitions
├── processed/
│   ├── adv_probes.jsonl                # Adversarial probes (all agents)
│   ├── world_fact_probes.jsonl         # World facts probes (from TOFU)
│   ├── dialogue/
│   │   ├── train.jsonl                 # Fine-tuning data — Dialogue Agent
│   │   ├── forget.jsonl                # Forget-set QA pairs — Dialogue Agent
│   │   ├── retain.jsonl                # Retain-set QA pairs — Dialogue Agent
│   │   ├── probes_forget.jsonl         # Evaluation probes, forget set
│   │   └── probes_retain.jsonl         # Evaluation probes, retain set
│   ├── monitoring/
│   │   ├── train.jsonl                 # Fine-tuning data — Monitoring Agent
│   │   ├── forget.jsonl                # Forget-set QA pairs — Monitoring Agent
│   │   ├── retain.jsonl                # Retain-set QA pairs — Monitoring Agent
│   │   ├── probes_forget.jsonl         # Evaluation probes, forget set
│   │   └── probes_retain.jsonl         # Evaluation probes, retain set
│   └── planning/
│       ├── train.jsonl                 # Fine-tuning data — Planning Agent
│       ├── forget.jsonl                # Forget-set QA pairs — Planning Agent
│       ├── retain.jsonl                # Retain-set QA pairs — Planning Agent
│       ├── forget_random_split.jsonl   # Forget set, randomised participant split
│       ├── retain_random_split.jsonl   # Retain set, randomised participant split
│       ├── probes_forget.jsonl         # Evaluation probes, forget set
│       ├── probes_retain.jsonl         # Evaluation probes, retain set
│       └── rand_probes_forget.jsonl    # Evaluation probes, randomised forget set
```

---

## File Formats

All `.jsonl` files contain one JSON object per line.

**Train / forget / retain files** (fine-tuning and unlearning inputs):
```json
{"question": "...", "answer": "..."}
```

**Probe files** (evaluation inputs):
```json
{"participant_id": "...", "probe": "...", "expected": "..."}
```

---

## Agent-Specific Output Schemas

Each agent is evaluated against role-specific correctness criteria:

| Agent | Key output fields | Evaluation method |
|---|---|---|
| Dialogue | Free-text response | Manual / semantic recall check |
| Monitoring | `phq8_score`, `severity` | Regex extraction of PHQ-8 score |
| Planning | `step_of_care`, `primary_intervention`, `secondary_interventions` | NICE NG222 severity tier keyword matching |

---

## Data Use

This benchmark is derived from the DAIC-WOZ corpus, which is subject to a restricted data use agreement. Raw transcripts and audio are not included. All files contain processed QA pairs only.

---

## Reference

> M. Cheung (2026). *Selective Machine Unlearning in Multi-Agent Psychotherapy Systems*. Bachelor's Thesis.
