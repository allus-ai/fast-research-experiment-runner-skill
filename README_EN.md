<div align="right">

English

</div>

![Research Experiment Runner Skill](assets/banner.svg)

<div align="center">

# Research Experiment Runner.skill

**Test new AI models like a small, reproducible research experiment.**

A Codex skill for quickly validating Hugging Face models, GGUF/local LLMs, research claims, training recipes, prompts, and multimodal systems without scattering downloads, virtualenvs, logs, and caches across your laptop.

It is built for the everyday question: **"A new model just came out. Can my machine run it, and is it worth keeping?"**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-Compatible-blueviolet)](https://skills.sh)
[![Local First](https://img.shields.io/badge/local--first-experiments-10B981)](#local-first-by-default)
[![Cleanup Ready](https://img.shields.io/badge/cleanup-ready-2563EB)](#storage--cleanup)

```bash
npx skills add allus-ai/fast-research-experiment-runner-skill
```

</div>

---

> [!NOTE]
> This skill is not a benchmark suite. It is a fast validation loop: resolve the exact target, choose the smallest useful test, scout hardware acceleration, run a bounded smoke test, capture evidence, and clean up safely.

---

## What It Does

Most model tests fail in boring ways: the wrong model ID, a bad runtime choice, an accidental 40GB download, an untracked virtualenv, or a demo that works once but leaves no report.

Research Experiment Runner turns each trial into a compact experiment package:

- resolves exact model, paper, repo, dataset, and revision IDs before download
- checks hardware before choosing PyTorch, MLX, llama.cpp, ONNX Runtime, OpenVINO, or CPU fallback
- estimates download size, memory pressure, venv size, and cleanup cost before expensive work
- runs smoke tests before full evaluation
- captures commands, environment, metrics, qualitative examples, failures, and recommendations
- builds a local Gradio or web demo only when useful
- keeps artifacts under one managed root with a dry-run cleanup path

---

## Core Loop

![Local-first validation loop](assets/validation-loop.svg)

Each run answers four practical questions:

| Question | Evidence captured |
|:---|:---|
| Does it run on this machine? | hardware profile, backend choice, load result |
| Is it fast enough to keep? | latency, tokens/sec, memory, runtime notes |
| Does it pass basic prompts or samples? | examples, metrics, failures |
| Can it be deleted cleanly? | artifact map, dry-run cleanup, reclaim estimate |

---

## Why This Exists

New AI models appear constantly. The tempting path is to copy a snippet, install a random stack, download a checkpoint, run one prompt, and forget what changed.

That creates three problems:

1. The result is not reproducible.
2. The machine fills with hidden caches and abandoned environments.
3. A model that merely starts gets mistaken for a model worth using.

This skill makes local AI experimentation boring in the right way: small first, measured, documented, and reversible.

---

## Five Principles

| # | Principle | Meaning |
|:---|:---|:---|
| 01 | **Local first** | Default to the current machine and only move to remote or paid compute with explicit approval |
| 02 | **Small before expensive** | Smoke test before large downloads, full benchmarks, native builds, or long runs |
| 03 | **Primary sources first** | Prefer official model cards, papers, repos, dataset cards, and runtime docs |
| 04 | **Evidence over vibes** | Record metrics, examples, failures, environment, and exact commands |
| 05 | **Cleanup is part of the experiment** | Every run must have an artifact map and dry-run cleanup path |

---

## Local First By Default

All experiment state lives under:

```text
~/.codex/research-experiment-runner/
```

Default layout:

```text
experiments/   reports, configs, logs, metrics
venvs/         one virtualenv per experiment
cache/         pip, Hugging Face, datasets, torch, npm
downloads/     explicit model/runtime downloads
tmp/           temporary files
```

---

## Quick Start

```bash
npx skills add allus-ai/fast-research-experiment-runner-skill
```

Ask Codex:

```text
Use $research-experiment-runner to test the latest 4B Qwen model locally.
```

---

## Storage & Cleanup

Always preview first:

```bash
python3 ~/.codex/skills/research-experiment-runner/scripts/cleanup.py --dry-run --all --include-cache
```

Delete after review:

```bash
python3 ~/.codex/skills/research-experiment-runner/scripts/cleanup.py --all --include-cache
```

---

## License

MIT

---

<div align="center">

**Try models quickly. Keep evidence. Delete cleanly.**

MIT License © [allus-ai](https://github.com/allus-ai)

</div>
