# SoME Overview

## Glossary
- **Experts**: Specialized sub-networks that perform targeted computations. SoME treats experts as reusable primitives whose activations can be routed and combined rather than retrained wholesale.
- **Keys**: Representation vectors emitted by the model to query and select experts. Keys are learned to expose task-relevant structure and drive routing decisions.
- **Knowledge Gravity**: The tendency of high-value experts to attract more routing mass over time, potentially leading to over-specialization. SoME mitigates this by regularizing routing entropy and enforcing balance constraints.
- **Router Collapse**: Failure mode where the router over-allocates traffic to a small subset of experts, reducing diversity. Avoided through balanced routing objectives, decay controls, and clamp safeguards.
- **Clamp Fix**: Intervention that caps or normalizes router outputs to prevent runaway activations and keep expert selection stable across steps.

## Conceptual Summary
Self-Organizing Mixture of Experts (SoME) posits that continual learning can be achieved by optimizing routing pathways to a fixed library of computational primitives. Instead of retraining every expert, SoME focuses on learning keys and routing policies that unlock compositional reuse. The system monitors routing dynamics for collapse modes (e.g., knowledge gravity) and applies stabilizers such as decay schedules and clamp operations to keep expert usage balanced. Later versions connect routing behaviors to clustering perspectives (k-means and spectral methods), grounding the approach in interpretable geometric structure.

## Guiding Principles
- **Reuse over retrain**: Prefer improving routing and key quality to modifying expert parameters, conserving compute and preserving prior capabilities.
- **Balanced specialization**: Encourage expert diversity through entropy-aware routing and safeguards against router collapse.
- **Stability first**: Apply decay controls and clamp fixes to keep routing signals well-behaved during adaptation.
- **Interpretable structure**: Relate routing patterns to clustering viewpoints (e.g., k-means, spectral links) to diagnose and tune behavior.
- **Composable improvements**: Evolve the system via small, testable fixes (decay adjustments, routing laws, clamp tweaks) that can be rolled forward across versions.
