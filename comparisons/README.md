# SoME vs GPT-2 (Transformer Baseline) on TinyStories

This directory documents a side-by-side TinyStories run comparing the SoME candidate model with a GPT-2–style Transformer baseline. The accompanying notebooks were exported to PDF for reproducibility and reference.

## Experiment setup
- **Dataset:** [`roneneldan/TinyStories`](https://huggingface.co/datasets/roneneldan/TinyStories) with subsets of 20,000 training and 5,000 validation examples.
- **Tokenizer:** A universal 8,192-vocab BPE tokenizer trained on 20k streaming TinyStories samples with `[UNK]`, `[PAD]`, and `[EOS]` tokens. The same tokenizer is reused for both models; inputs are padded/truncated to 512 tokens with an appended EOS marker.
- **Models:** Shared Transformer backbone with 8 layers, `d_model=512`, 8 attention heads, and `seq_len=512`.
  - *SoME candidate:* Replaces the feed-forward block with a SoME layer configured with 128 experts, `d_ffn=1536`, `top_k=8`, sparse initialization, `alpha=0.015`, `beta=0.001`, `delta=0.001`, `theta_percentile=0.05`, warmup of 400 steps, and `ema_decay=0.995` (all ablations enabled). Experts are frozen; routing and attention remain trainable (≈18.9M of 1.63B parameters).
  - *Transformer baseline (GPT-2–style):* Uses a standard GELU MLP feed-forward block.
- **Training:** 4 epochs, batch size 24, AdamW with learning rate `4e-4` (betas 0.9/0.95, weight decay 0.1), cosine LR schedule, gradient clipping at 1.0, mixed precision (`torch.cuda.amp`), training temperature 1.0 (eval temperature 0.5).
- **Hardware expectations:** Designed for GPU runs (Colab-style). The script auto-enables TF32 on GPUs with compute capability ≥ 8 (e.g., A100) and turns on `torch.backends.cudnn.benchmark`.

## Reproducing the runs
1. Install dependencies: `pip install torch datasets transformers huggingface_hub tokenizers matplotlib`.
2. Ensure access to a CUDA GPU (A100 recommended for TF32); TF32 is enabled automatically when available.
3. From `some-vs-gpt2-code.pdf`, select the configuration in Cell 5 (e.g., `get_config(architecture='some_candidate', dataset_name='tinystories')` or `'transformer_baseline'`) and run `main(config)`.
4. The script will train or load the shared BPE tokenizer, download the TinyStories splits, tokenize to length 512, and train for 4 epochs.
5. Monitor training/validation loss and validation perplexity each epoch. For the SoME run, also track middle-layer expert Gini and Shannon entropy; a metrics PNG and `summary.txt` are written to the run directory.

## Observed results (TinyStories)
- **SoME candidate:** Best validation loss 1.0187 (perplexity 2.77); middle-layer expert metrics logged per epoch (e.g., Gini ≈ 0.669, entropy ≈ 5.438 in early epochs).
- **Transformer baseline:** Best validation loss 0.8720 (perplexity 2.39).

## Artifacts
- Experiment code (notebook export): [`some-vs-gpt2-code.pdf`](some-vs-gpt2-code.pdf)
- Logged training/output: [`some-vs-gpt2-results.pdf`](some-vs-gpt2-results.pdf)
