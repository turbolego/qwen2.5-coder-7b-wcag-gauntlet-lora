# Qwen2.5-Coder-7B WCAG Gauntlet Tool-Use LoRA (+ Q4_K_M GGUF)

LoRA adapter that teaches **Qwen2.5-Coder-7B** the agentic tool-routing skill from
[wcag-skill](https://github.com/turbolego/wcag-skill): while running the
[AI-WCAG-Gauntlet](https://github.com/turbolego/AI-WCAG-Gauntlet) benchmark loop, pick the
right validator (`w3c` / `axe` / `pa11y` / `qualweb` / `fix` / `check`) and run a self-check
on your own code **before** the benchmark's official iteration check.

Same training method as [qwen3.5-0.8b-wcag-gauntlet-lora](https://github.com/turbolego/qwen3.5-0.8b-wcag-gauntlet-lora),
scaled to the 7B coder base (fits an RTX 1000 with 6GB VRAM at Q4_K_M).

## Quick facts

| | |
|---|---|
| **Base model** | [Qwen/Qwen2.5-Coder-7B](https://huggingface.co/Qwen/Qwen2.5-Coder-7B) (bf16) |
| **Method** | LoRA, rank 32, α=64, dropout 0.05, gradient checkpointing |
| **Target layers** | `q_proj`, `k_proj`, `v_proj`, `o_proj`, `gate_proj`, `up_proj`, `down_proj` |
| **Optimizer** | AdamW, lr 2e-4, cosine schedule, 5% warmup, batch 1 × grad-accum 16 |
| **Hardware** | Kaggle T4 / P100 kernel (`hummern/lora-tooluse-skill-7b-v1`) |
| **Training data** | 300 fit / 40 val / 155 test rows (495 total, same as 0.8B run) |
| **License** | Apache 2.0 |

## 256K-context successor (for GitHub Copilot BYOK)

Qwen2.5-Coder-7B is capped at ~33K context and is unusable for GitHub Copilot BYOK
(needs >=128K + tool calling). The same gauntlet tool-use skill is rebased onto
**Qwen3-Coder-30B-A3B-Instruct (256K context)** in
[qwen3-coder-30b-a3b-wcag-gauntlet-lora](https://github.com/hummern/qwen3-coder-30b-a3b-wcag-gauntlet-lora) —
publish there first; this repo stays as the 7B artifact.

## Files in this repo

| File | Purpose |
|---|---|
| `lora_tooluse_skill_7b.py` | Training script (LoRA fine-tune on Qwen2.5-Coder-7B, Kaggle-ready, data inlined) |
| `tooluse_fit.jsonl` | Training data — 300 rows |
| `tooluse_val.jsonl` | Validation data — 40 rows |
| `tooluse_test.jsonl` | Held-out test data — 155 rows |
| `tooluse_questions.json` | Vegaml question definitions used in training |
| `LICENSE` | Apache 2.0 |
