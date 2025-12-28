# Self-Organizing-Mixture-of-Experts-SoME-
The core thesis of SoME is that continual learning can be achieved not by retraining a model's entire knowledge base, but by optimizing the pathways to a static library of computational primitives. If this is true it could be a way to effectively recycle the computational primitives of older, open-source models that are just sitting on a shelf.

## Documentation
- [Overview](docs/OVERVIEW.md): Glossary, conceptual summary, and guiding principles for SoME.
- [Changelog](docs/CHANGELOG.md): Version-by-version fixes and feature notes.
# Self-Organizing Mixture of Experts (SoME)

## What / Why
- **Goal:** Demonstrate continual learning by optimizing routing pathways to a stable library of computational primitives instead of retraining the primitives themselves.
- **Approach:** A self-organizing mixture-of-experts (MoE) that adapts routing during training to specialize experts while keeping the expert set stable.
- **Outcome:** Enables reuse of existing open models as experts, with routing updates capturing new knowledge while preserving the underlying primitives.

## Version Map
- **Reference implementation:** **v7 (Clamp Fix)** – use this as the baseline for reproductions. Code and results are packaged as PDFs in [`notebooks/v7/some-v7-clamp-fix-code.pdf`](notebooks/v7/some-v7-clamp-fix-code.pdf) and [`experiments/v7/some-v7-clamp-fix-results.pdf`](experiments/v7/some-v7-clamp-fix-results.pdf).
- **Historical context:** Earlier variants (v1–v6) remain available for comparison in their respective folders. See [`VERSIONS.md`](VERSIONS.md) for links to every version’s narrative, notebook, and results artifacts.

## Quickstart
1. **Environment:**
   - Python 3.10+ with virtualenv/venv.
   - Install the dependencies noted in the version notebook you are running (PyTorch with CUDA support, Jupyter, and common scientific Python packages are typically required).
2. **Hardware:** A single modern NVIDIA GPU (16GB+ recommended) for training; CPU-only is sufficient for browsing artifacts.
3. **Dataset:** Use the datasets described in the version-specific docs/results PDFs (see `docs/` and `experiments/`). The PDFs outline the synthetic and benchmark tasks used for each release.
4. **Key hyperparameters to track (defaults documented in the v7 notebook):**
   - Number of experts and hidden sizes.
   - Router temperature/softmax settings and the **clamp bounds** applied in v7 to stabilize routing logits.
   - Learning rate, weight decay, and optimizer settings.
   - Sequence length, batch size, and curriculum schedule.
5. **Run:**
   - Open the version’s code PDF (e.g., `notebooks/v7/some-v7-clamp-fix-code.pdf`) and execute the cells in a Jupyter environment.
   - Log outputs to reproduce the metrics reported in the matching `experiments/<version>/` PDF.

## Repository Layout
- `VERSIONS.md` – index of all SoME releases and their artifacts.
- `SoME-v1` ... `SoME-v7` – per-version folders (v7 is the reference Clamp Fix release).
- `docs/` – papers and narrative PDFs.
- `notebooks/` – code notebooks exported as PDFs for each version.
- `experiments/` – result PDFs for each version.
- `comparisons/` – cross-model benchmarks (e.g., SoME vs GPT-2).

## Key Concepts
- **Self-organizing routing:** The router learns expert specialization without manual assignment.
- **Stable expert library:** Experts remain fixed; learning happens in the routing pathways.
- **Clamp Fix (v7):** Routing logits are clamped to prevent overflow/instability, producing the current reference behavior.

## Experiments & Metrics
- Quantitative results are recorded in `experiments/<version>/` PDFs, with side-by-side code in `notebooks/<version>/`.
- External baselines and comparisons live in `comparisons/`.
- Consult `VERSIONS.md` to jump directly to the artifacts for any release.

## Roadmap
- Maintain v7 (Clamp Fix) as the canonical baseline while iterating on future releases.
- Publish runnable scripts (alongside the PDFs) to streamline reproduction across environments.
- Broaden benchmarks in `comparisons/` to cover additional public models and tasks.

## License
This project is licensed under the MIT License (see [`LICENSE`](LICENSE)).
