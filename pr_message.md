## Submission: DRQ-27B TauSynth-GRIPO-Multistage

This submission evaluates DRQ-27B TauSynth-GRIPO-Multistage on the τ-bench / τ³-bench core text domains. The model is a Qwen3.5-27B-based checkpoint trained with a multi-stage agentic post-training pipeline.

### Overview

- **Model**: DRQ-27B TauSynth-GRIPO-Multistage
- **Base model**: Qwen3.5-27B
- **Submitting organization**: SuperQ
- **Submission date**: 2026-06-28
- **Submission type**: Custom text submission
- **Domains**: Airline, Retail, Telecom
- **Contact**: litianyu7@xiaomi.com, ryantianyuli@aliyun.com
- **Submission directory**: `web/leaderboard/public/submissions/drq-27b-tausynth-gripo-multistage_2026-06-28/`

### Results — Core Domains

Pass^k uses the official tau2-bench pass-hat-k formula `C(success_count, k) / C(num_trials, k)`, averaged over tasks per domain.

| Domain | Pass^1 | Pass^2 | Pass^3 | Pass^4 |
|--------|--------|--------|--------|--------|
| airline | 62.50% | 53.00% | 47.50% | 44.00% |
| retail | 85.53% | 80.12% | 75.88% | 71.93% |
| telecom | 23.68% | 18.13% | 14.47% | 11.40% |

Core-domain average:

| Metric | Average |
|--------|--------:|
| Pass^1 | 57.24% |
| Pass^2 | 50.42% |
| Pass^3 | 45.95% |
| Pass^4 | 42.44% |

### Link to Trajectory

https://github.com/AiObserver/DRQ-27B/tree/main/trajectories

Core-domain trajectory files:

- `drq-27b-tausynth-gripo-multistage_airline_gpt-5.5_4trials.json`
- `drq-27b-tausynth-gripo-multistage_retail_gpt-5.5_4trials.json`
- `drq-27b-tausynth-gripo-multistage_telecom_gpt-5.5_4trials.json`

The repository also contains a `banking_knowledge` trajectory file for reference, but this PR submits only the core leaderboard domains.

### Evaluation Setup

- **Trials**: 4 trials per task
- **Task split**: base
- **Task coverage**: all tasks, no task filters
- **Agent scaffold**: `llm_agent`
- **Agent LLM**: `openai/tau27_targeted_sft_merged`, served from a local vLLM OpenAI-compatible endpoint
- **User simulator**: `openai/gpt-5.5`
- **Max steps**: 40
- **Agent temperature**: 0.0
- **User simulator temperature**: 0.0

Domain/task coverage:

| Domain | Tasks | Simulations | Trials per task |
|--------|------:|------------:|----------------:|
| airline | 50 | 200 | 4 |
| retail | 114 | 456 | 4 |
| telecom | 114 | 456 | 4 |

### Methodology Notes

The DRQ-27B TauSynth-GRIPO-Multistage model is based on Qwen3.5-27B and trained through three stages:

1. **GRIPO RL**: agentic reinforcement learning based on tau-style synthetic trajectories and tau-bench-style interaction data, producing the step500 GRIPO post-training checkpoint used as the main initialization.
2. **GRIPO-MOPD two-stage training**: second-stage trajectory and tool-use repair with MOPD-style multi-teacher or multi-checkpoint distillation to reduce seesaw regressions across tau3 domains and consolidate complementary checkpoint behaviors.
3. **GRIPO-SFT OpenAI-format instruction-following training**: supervised fine-tuning on OpenAI-compatible chat and tool-call format data to improve instruction following, function-call formatting, schema-valid arguments, and local vLLM OpenAI-compatible serving behavior.

Additional notes:

- Modified prompts: Yes
- Omitted questions: No
- No infrastructure-error terminations are present in the final core-domain results.
- Reported Pass^k uses `C(c,k)/C(n,k)` per task, followed by arithmetic mean over tasks.
- The submitted `submission.json` uses `litianyu7@xiaomi.com` as the schema-compatible primary contact email; `ryantianyuli@aliyun.com` is the secondary contact.

### Changes

- Adds `web/leaderboard/public/submissions/drq-27b-tausynth-gripo-multistage_2026-06-28/submission.json`
- Updates `web/leaderboard/public/submissions/manifest.json` by adding `drq-27b-tausynth-gripo-multistage_2026-06-28` to `submissions`
