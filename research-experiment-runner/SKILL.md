---
name: research-experiment-runner
description: Standardized local-first experimental validation for new AI models, Hugging Face models, GGUF/local LLMs, research papers, benchmark claims, training recipes, prompts, multimodal/vision systems, and general ML methods. Use when Codex needs to identify the correct model, scout hardware-specific acceleration, run a small reproducible smoke test, compare CPU/GPU/runtime choices, create a local demo, capture environment/logs/metrics, clean model caches safely, and produce a readable experiment report.
---

# Research Experiment Runner

## Default Mode

Run local-first, quick-validation experiments unless the user explicitly asks for full reproduction or remote execution. Treat every experiment as a small reproducible package with a report, optional demo, and one-command cleanup path.

Keep every experiment, virtual environment, model cache, package cache, download, temporary file, and improvement log under one managed root:

```text
~/.codex/research-experiment-runner/
```

Never install experiment dependencies globally. Before running installs, model downloads, or demos, source the experiment env file:

```bash
source <experiment-dir>/env.sh
```

Use the bundled scripts without loading them unless edits are needed:

```bash
python scripts/new_experiment.py --name <slug> --task <llm|vision|multimodal|general-ml> --target "<paper-or-model>"
python scripts/hardware_profile.py --output <experiment-dir>/hardware.json
python scripts/capture_env.py --output <experiment-dir>/env.json
python scripts/render_report.py <experiment-dir>
python scripts/cleanup.py --dry-run
```

## Workflow

1. Clarify the experiment target and success criteria.
   - Identify the exact paper/model/repo/model card/dataset URL. If the user gives an informal name such as "qwen 4b" or "gemma 12b", resolve the exact current model ID from official or primary sources before downloading.
   - Write the main claim to test in one sentence.
   - Choose the smallest useful dataset slice and metric. For new local LLMs, default to 3 prompts: factual, simple reasoning/math, and the user's main language.
   - Confirm hardware, time budget, and whether network/downloads are allowed.

2. Inspect primary sources first.
   - Prefer official paper, code repo, model card, dataset card, and benchmark documentation.
   - Record exact links, commit hashes, model revisions, dataset versions, and licenses.
   - If current facts matter, browse or use the relevant official/primary source before choosing models or commands.

3. Scout hardware acceleration before installation or execution.
   - Run `scripts/hardware_profile.py --output <experiment-dir>/hardware.json`.
   - Browse current official/primary sources for the detected hardware, OS, task family, and model/runtime.
   - Write the selected backend, install commands, fallbacks, and source links to `<experiment-dir>/acceleration.md`.
   - For local LLMs, prefer the smallest good GGUF that can answer the question being tested before trying full Transformers weights.
   - Estimate download size, venv size, runtime memory, and cleanup command before downloading.
   - If no reliable acceleration path exists, record CPU-only as the baseline and explain why.
   - 🔴 CHECKPOINT: get explicit user approval before installing a large framework, compiling native backends, downloading drivers/toolchains, or switching to paid/remote compute.

4. Scaffold the experiment.
   - Use `scripts/new_experiment.py` unless the repo already has a stronger experiment structure.
   - Use the default managed root unless the user explicitly asks for another location.
   - Source `<experiment-dir>/env.sh` before installing dependencies or running downloads.
   - Keep commands in `commands.log`.
   - Keep metrics in `results/metrics.json` or `results/metrics.csv`.
   - Keep qualitative examples in `results/examples.md`.

5. Run a smoke test before expensive work.
   - Load the model or method.
   - Run 1-5 examples.
   - Verify outputs, shapes, labels, latency, tokens/sec, memory, output file size, and failure behavior.
   - Use hard timeouts and bounded output capture. If output grows unexpectedly, stop the process, save a small excerpt, delete the oversized file, and record the failure.
   - Stop and report blockers before scaling up.

6. Run the quick validation.
   - Use deterministic seeds where supported.
   - Prefer tiny benchmark subsets before full datasets.
   - Compare against at least one baseline when feasible.
   - Capture failures, skipped cases, and manual interventions.

7. Build the demo.
   - Use Gradio by default for inference, comparison, and qualitative review.
   - Use a web demo only when product-like interaction, custom UI, or richer visualization matters.
   - Keep demos local by default and document the start command.

8. Produce the report.
   - Run `capture_env.py` and `render_report.py`.
   - Include objective, setup, acceleration decision, commands, metrics, examples, failures, limitations, and recommendation.
   - State whether to adopt, investigate further, or reject.
   - For local model tests, state whether the model is practical for this machine, not only whether it technically runs.

9. Improve the skill from real usage.
   - Append friction, missing playbooks, cleanup problems, and useful defaults to `~/.codex/research-experiment-runner/improvement-log.md`.
   - When the user asks for optimization, use `$darwin-skill` to score and improve this skill's `SKILL.md`.
   - Do not auto-edit this skill after every experiment; collect evidence first, then optimize with an explicit user request or after at least 3 logged experiments.

## Reference Loading

- Read `references/experiment-protocol.md` when defining the protocol, acceptance criteria, or report.
- Read `references/task-playbooks.md` when choosing task-specific metrics, baselines, smoke tests, or demo patterns.
- Read `references/demo-guidelines.md` when building Gradio or web demos.
- Read `references/storage-and-cleanup.md` before installing dependencies, downloading models/datasets, or cleaning old runs.
- Read `references/acceleration-scout.md` before installing PyTorch, TensorFlow/JAX, llama.cpp, ONNX Runtime, OpenVINO, MLX, CUDA, ROCm, Metal/MPS, Vulkan, or task-specific inference runtimes.
- Read `references/local-llm-playbook.md` before testing local LLMs, GGUF models, llama.cpp, Ollama, LM Studio, mlx, vLLM, or Transformers generation models.

## Operating Rules

- Do not start long-running training, large downloads, or paid cloud work without explicit user approval.
- Do not write experiment artifacts or dependency caches outside `~/.codex/research-experiment-runner/` unless the user explicitly requests it.
- Do not install dependencies or start the experiment before writing `<experiment-dir>/acceleration.md`.
- Do not keep multi-megabyte failed stdout logs. Save an excerpt and delete the oversized file.
- Do not claim a model is good for local use just because it starts. Report speed, memory, correctness, and cleanup cost.
- Do not treat a successful demo as a successful experiment; report metrics and limitations separately.
- Do not hide failed attempts. Record meaningful failures in the report.
- Prefer primary sources and pinned versions over blog posts or copied snippets.
- Keep the first pass small enough that it can finish on the available machine.
