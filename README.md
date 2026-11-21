# Evaluating Symbolic Persona Anchoring and Direct KV-Cache Interventions for Multi-Turn LLM Persona Preservation: A Six-Method, 10-Model Longitudinal Study

**Principal Investigator & Author:** Nabh Sanjay Mehta (Principal Investigator & Director, Phlowerman Ventures LLP)  
**Contact:** nabhsanjaymehta@phlowerman.com  
**Repository:** `https://github.com/phlowermanventuresllp/kv-cache-vs-spa`  
**Benchmark Scale:** 6 Experimental Arms (`A1`–`A6`) × 10 Open-Weight Models (3B–14B) × 4 Personas (`P1`–`P4`) = **240 Trajectories / 12,000 Conversational Turns**

---

## Repository Contents

This repository hosts the benchmark harness, verification and audit notebooks, and experimental/statistical archives (with the manuscript PDF published separately on Zenodo)[cite: 1, 2, 4, 37, 69, 72]:

- **`SPA_vs_KV/01_kv_cache_vs_spa_10_experiment/`**: Primary 10-model, 6-arm longitudinal benchmark execution and archive package (`240` trajectories across `50` turns each = `12,000` turns, plus `kv_spa_harness` modules and manifests)[cite: 1, 3, 69].
- **`SPA_vs_KV/02_kv_cache_vs_spa_results/`**: Post-run statistical analysis, publication tables (Tables 1–5), turn-level (`12,000` turns) and trajectory-level (`240` trajectories) CSV exports, and figure generation outputs[cite: 4, 69].
- **`SPA_vs_KV/03_kv_cache_vs_spa_four_stage_independent_cryptographic_bitwise_audit_v1.ipynb`**: Four-stage independent cryptographic, AST, 12,000-turn metric recomputation, and MixedLM audit notebook[cite: 1, 69].
- **`SPA_vs_KV/04_kv_cache_vs_spa_final_camera_ready_audit_checkpoints_module_hash_verification.ipynb`**: Final camera-ready verification notebook auditing archive/corpus SHA-256 digests, empty-response telemetry (Table 11), boundary-window sensitivity (`N=235` vs. `N=240`), resolved Hugging Face commits (Table 12), and module AST hashes (Table 13)[cite: 2, 69].

---

## Experimental Arms (`A1`–`A6`)

- **Prompt-Channel Baselines & Symbolic Anchoring (`A1`–`A3`):**
  - **A1 (Raw History):** Unreinforced Turn 0 persona system prompt with verbatim multi-turn history accumulation[cite: 5, 39].
  - **A2 (Generic Reminder):** Primary reference baseline appending a natural-language persona reminder per user turn[cite: 5, 39].
  - **A3 (Continuous SPA):** Symbolic Persona Anchoring with a write-once System Precedence Protocol and compact three-token bracketed macro (`[SYS MACRO: <ROLE>]`) per turn[cite: 5, 39].
- **Direct KV-Cache Interventions (`A4`–`A6`):**
  - **A4 (Belitsky-style KV Steering):** Multi-turn adaptation of one-shot additive Key/Value steering (`c_k = c_v = 0.10`) over scaled mid-layers[cite: 6, 39].
  - **A5 (Sun-style KV Trajectory Transplantation):** Multi-turn adaptation (`MID_LAYER_V2`, trailing slice up to 32 tokens) of mid-layer donor K/V tensor replacement[cite: 6, 39].
  - **A6 (Memory Inception):** Multi-turn adaptation (`L_mem = 2`, `gamma = 1.0`) of canonical pre-RoPE latent KV memory bank insertion over scaled mid-layers[cite: 6, 39].
- **Isolated Exploratory Comparator (`OPT_GCAD`):** Attention-output activation steering hook implementation isolated outside the primary six-arm factorial matrix[cite: 5, 9].

---

## Headline Empirical Summary (N = 40 Matched Cells / 2,000 Turns per Arm)

| Arm | Mean P_sem ± SD | Δ P_sem (T46–50 vs. T1–5) | Mean MATTR-50 ± SD | Δ MATTR-50 (T46–50 vs. T1–5) | MATTR-50 Decline Flag (%) | Hard Violations (2,000 turns) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **A1** Raw History | 0.4421 ± 0.0720 | -0.0096 | 0.8119 ± 0.0531 | -0.0329 | 15.0% | 0 |
| **A2** Generic Reminder (Ref) | 0.4781 ± 0.0597 | +0.0207 | 0.8117 ± 0.0678 | -0.0206 | 27.5% | 1 |
| **A3** Continuous SPA | 0.4589 ± 0.0600 | +0.0206 | 0.8155 ± 0.0607 | -0.0042 | 7.5% | 3 |
| **A4** Belitsky KV Steer | 0.4436 ± 0.0699 | -0.0097 | 0.8087 ± 0.0557 | -0.0223 | 15.0% | 1 |
| **A5** Sun KV Transplant | 0.4664 ± 0.0757 | -0.0430 | 0.8034 ± 0.0707 | -0.0279 | 22.5% | 0 |
| **A6** Memory Inception | 0.4420 ± 0.0695 | -0.0020 | 0.8137 ± 0.0503 | -0.0045 | 17.5% | 0 |

---

## Cryptographic Integrity & Verified Module Hashes

- **Production Backup Archive Byte SHA-256 (`18.24 MB`):** `68caad94812bcf582ce7ff40c9d0ecadc6f13415df380684f43fa8ad7f91bd14`[cite: 2, 29, 30]
- **240-JSON Trajectory Corpus Content SHA-256:** `d65da777d58a78b7bd693b4d7fb43eb888f1615a1ef7028a2a0bf52ab06a5380`[cite: 2, 29, 30]
- **12,000-Turn Bitwise Metric Cross-Check:** `max |Δ P_sem| = 5.55e-17`, `max |Δ MATTR-50| = 1.11e-16`, `max |Δ marker_density| = 3.55e-15`, and `100%` exact match on hard constraint violations[cite: 2, 29, 30].

| Harness Module | SHA-256[:16] | Core Responsibility |
| :--- | :---: | :--- |
| `kv_spa_harness/foundation.py` | `a435f4a49d3a77e9` | Arm specs, 10-model slugs, SHA-256 seeding, manifest export[cite: 2, 3, 34] |
| `kv_spa_harness/model_registry.py` | `4ceae790d7a0d159` | Model specs, 4-bit NF4 selector, chunked prefill, universal `DynamicCache` I/O[cite: 2, 3, 34] |
| `kv_spa_harness/dataset.py` | `b04a32e20e8c72e5` | Personas `P1`–`P4`, rubrics, 50-turn scenario & stressor generator[cite: 2, 3, 34] |
| `kv_spa_harness/baselines.py` | `99cc2662d7db4e78` | Prompt-channel arms `A1`–`A3`, message builder, CUDA telemetry runner[cite: 1, 2, 3, 34] |
| `kv_spa_harness/belitsky_steering.py` | `5dfe93067bd8de5c` | Arm `A4` contrastive vector extraction and additive KV steering[cite: 1, 2, 3, 34] |
| `kv_spa_harness/sun_transplantation.py` | `0f45e10a5c684403` | Arm `A5` donor KV bank builder and mid-layer trajectory transplantation[cite: 1, 2, 3, 34] |
| `kv_spa_harness/memory_inception.py` | `783be8470e9ffd12` | Arm `A6` pre-RoPE memory bank builder and latent KV injection[cite: 1, 2, 3, 34] |
| `kv_spa_harness/gcad_and_controls.py` | `ef63fa5a241cc3db` | Isolated `OPT_GCAD` attention hooks and legacy SPA robustness controls[cite: 2, 3, 34] |
| `kv_spa_harness/metrics_and_stats.py` | `a61316c59257139d` | `P_sem`, `MATTR-50`, marker density, violations, BH-FDR, bootstrap Cohen's d[cite: 1, 2, 3, 34] |
| `kv_spa_harness/runner.py` | `db6575e174af0dba` | `ExperimentOrchestrator`, 240-cell job matrix, exporters[cite: 1, 2, 3, 34] |

---

## Citation

```bibtex
@misc{mehta2026kvspa,
  author       = {Mehta, Nabh Sanjay},
  title        = {Evaluating Symbolic Persona Anchoring and Direct KV-Cache Interventions for Multi-Turn LLM Persona Preservation: A Six-Method, 10-Model Longitudinal Study},
  year         = {2026},
  publisher    = {Phlowerman Ventures LLP / Zenodo},
  url          = {[https://github.com/phlowermanventuresllp/kv-cache-vs-spa](https://github.com/phlowermanventuresllp/kv-cache-vs-spa)}
}
```
---

## Copyright & License

© 2026 Phlowerman Ventures LLP. All Rights Reserved.

Made publicly available for academic peer review and reproducibility verification. No commercial use, redistribution, or derivative works are permitted without prior written permission from Phlowerman Ventures LLP.
