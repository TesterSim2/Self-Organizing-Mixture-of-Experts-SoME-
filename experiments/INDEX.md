# Experiment Index (SoME v2–v7)

## v2 — Professional ablation runs
- **Dataset**: train_subset_size=20,000, val_subset_size=4,000.
- **Config**: num_experts=128; top_k=8; alpha=0.015; beta=0.001; delta=0.001; theta_percentile=0.05; ema_decay=0.995; init_method=sparse.
- **Sequence length**: 1,024 (batch_size=32).
- **Reported metrics**: final val loss=0.5046 (ppl=1.66); middle-layer Gini=0.772; entropy=4.528; expert usage 128/128.
- **Observations**: Baseline run with full expert coverage and stable entropy growth.
- **Source**: [experiments/v2/some-v2-professional-ablation-runs.pdf](v2/some-v2-professional-ablation-runs.pdf).

## v3 — Ablation runs (v1 report)
- **Dataset**: roneneldan/TinyStories with train_subset_size=20,000.
- **Configs sampled**:
  - Sanity/medium/capacity scaling: num_experts from 32→256; top_k=2→16; init_method=default.
  - Alpha/beta/delta, theta_percentile, and ema_decay not explicitly stated in the report (default heuristic settings).
- **Sequence lengths**: 256 for core ablations; 512 for batch-vs-sequence scaling (V7/V8/V9/V10).
- **Reported metrics (highlights)**:
  - Capacity scaling (V3, seq_len=256, num_experts=256, top_k=8): final val loss=2.0542 (ppl=7.80); Gini=0.539; entropy=7.134.
  - Depth vs width (V6, seq_len=256, num_experts=64, top_k=8): final val loss=2.0471 (ppl=7.75); Gini=0.383; entropy=5.635.
  - Batch vs sequence scaling v1 (V7, seq_len=512, batch_size=64): final val loss=1.1134 (ppl=3.04); Gini≈0.661; entropy≈4.454.
- **Observations**: Larger sequence length with moderate batch size improved perplexity; orthogonal initialization (V9) underperformed default due to reduced expert diversity and higher routing concentration.
- **Source**: [experiments/v3/some-v3-ablation-runs-v1.pdf](v3/some-v3-ablation-runs-v1.pdf).

## v3.5 — Decay fix results
- **Dataset**: tokenizer vocab_size=8,192 (dataset name not explicitly logged).
- **Config**: num_experts/top_k/alpha/beta/delta/theta_percentile/ema_decay not stated; continuation of width-fix baseline.
- **Sequence length**: Not specified in the report.
- **Reported metrics**: Epoch 2 val loss=0.7324 (ppl=2.08); middle-layer Gini=0.835; entropy=3.962.
- **Observations**: Decay fix stabilizes training while keeping all experts alive (no pending respawns).
- **Source**: [experiments/v3.5/some-v3-5-decay-fix-results.pdf](v3.5/some-v3-5-decay-fix-results.pdf).

## v4 — Professional ablation series (A/B/C)
- **Dataset**: train_subset_size=10,000; val_subset_size=2,000; vocab_size=8,192.
- **Config**: alpha=0.01; beta=0.005; delta=0.001; theta_percentile=0.05; ema_decay=0.99; top_k=4; init_method=default; MLP router.
- **Sequence length**: 768 (batch_size=32).
- **Series A (A1)**: num_experts=64; final val loss=0.7629 (ppl=2.14); Gini=0.791; entropy=4.085.  
  **Observation**: Balanced utilization—64/64 experts used with improving entropy.
- **Series B (A4 baseline)**: num_experts=64 with larger D_MODEL=512/NUM_LAYERS=10; final val loss=0.7126 (ppl=2.04); Gini=0.851; entropy=3.413.  
  **Observation**: Deeper model increased routing concentration (higher Gini).
- **Series C (B1 baseline)**: num_experts=128; final val loss=0.7602 (ppl=2.14); Gini=0.914; entropy=3.427.  
  **Observation**: Doubling experts raised Gini and slightly reduced entropy, suggesting router capacity pressure.
- **Sources**: [Series A](v4/some-v4-ablation-series-a-results.pdf) | [Series B](v4/some-v4-ablation-series-b-results.pdf) | [Series C](v4/some-v4-ablation-series-c-results.pdf).

## v5 — “8 big fixes” (linear vs MLP router)
- **Dataset**: tokenizer with vocab_size=8,192; train subset size≈10,000.
- **Config**: num_experts/top_k/alpha/beta/delta/theta_percentile/ema_decay not logged; ablation flags show use_alpha/beta/delta=True.
- **Sequence length**: Not specified.
- **Reported metrics**:
  - Linear router: final val loss=2.8337 (ppl=17.01); Gini≈0.372; entropy≈6.675.
  - MLP router: final val loss=2.8424 (ppl=17.16); Gini≈0.530; entropy≈6.279.
- **Observations**: Linear router maintained lower Gini (more even routing) while MLP kept similar perplexity; both runs benefit from the “8 fixes” but retain relatively high perplexity compared to v4.
- **Sources**: [Linear router](v5/some-v5-linear-8-fixes-results.pdf) | [MLP router](v5/some-v5-mlp-8-fixes-results.pdf).

## v6 — Decay fix
- **Dataset**: tokenizer vocab_size=8,192 (subset sizes not logged).
- **Config**: alpha/beta/delta enabled; other hyperparameters not recorded in the report.
- **Sequence length**: Not specified.
- **Reported metrics**: final val=2.6102 (ppl=13.60); middle-layer Gini sequences fall from 0.722→0.710; no dead-expert respawns.
- **Observations**: Decay fix lowers perplexity relative to v5 and prevents expert death without needing respawns.
- **Source**: [experiments/v6/some-v6-decay-fix-results.pdf](v6/some-v6-decay-fix-results.pdf).

## v7 — Clamp fix
- **Dataset**: tokenizer vocab_size=8,192 (subset sizes not logged).
- **Config**: alpha/beta/delta enabled; other hyperparameters not recorded in the report.
- **Sequence length**: Not specified.
- **Reported metrics**: final val=2.6036 (ppl=13.51); middle-layer Gini steps 0.826→0.653 with phoenix respawns dropping to zero.
- **Observations**: Clamp fix improves routing stability over v6, reducing Gini while keeping perplexity slightly better.
- **Source**: [experiments/v7/some-v7-clamp-fix-results.pdf](v7/some-v7-clamp-fix-results.pdf).
