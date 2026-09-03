# GPU-Scale Experiments on Random 3-SAT

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.18451652.svg)](https://doi.org/10.5281/zenodo.18451652)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

GPU-accelerated computational experiments on randomly generated 3-SAT instances — 13.55 billion assignment evaluations on a single consumer laptop GPU (NVIDIA RTX 5070): exhaustive-enumeration benchmarks up to 2³⁰ assignments, sampled solution-space statistics across clause densities, and feature-based instance classification.

## Scope

These experiments benchmark a GPU exhaustive enumerator and characterize random 3-SAT instance structure. Measurements of particular algorithms on finite instances characterize implementations and instance distributions; they carry no implications for the P vs NP question. The paper's "Scope" section ([main.pdf](main.pdf), Section 5) spells out exactly what such experiments can and cannot show.

## Key results

| Experiment | Measurement | Finding |
|---|---|---|
| Enumeration throughput | n = 26–30, exhaustive | Peak 979M assignments/sec at n=29; 6.94× drop at n=30 coinciding with smaller memory chunks (2²⁴) and early-termination effects |
| Exhaustive UNSAT | n=26, all 67,108,864 assignments | Decided UNSAT in 2.05 s — exact ground truth at 2²⁶ scale in seconds |
| Solution-graph statistics | Cycle rank β₁ of median-thresholded graphs over ≤500 sampled satisfying assignments, 3,000 instances | β₁ tracks clause density: sparse formulas (2.0 clauses/var) yield large, high-variance β₁ (many solutions → big sampled graphs); near-threshold formulas yield β₁ ≈ 0 |
| Feature distances | L2 distances between 128-dim feature vectors, 441,000 instance pairs | Heterogeneous instance distribution (min 4.0, median 25.0, max 110.5), driven by size and density spread |
| Size-category classifier | 128→512→256→128→2 network, 5,692 training instances | 77.8% accuracy predicting instance-size category; top features are variable count (0.83) and clause density (0.76) |
| Aggregate scale | All experiments | 13,551,672,064 assignments evaluated in ~40 minutes of GPU time |

The five instance categories (tagged `random` / `hierarchical` / `algebraic` / `planted` / `phase_transition` in the code and result files) all draw uniformly random 3-clauses and differ only in clause-to-variable ratio (4.2 / 3.0 / 2.0 / 5.0 / 4.267) — the tags are historical, and differences between categories are density effects.

## Papers

- **Main paper:** [main.pdf](main.pdf) — experiments, results, interpretation, and scope
- **Supplement:** [supplement.pdf](supplement.pdf) — implementation details, full result tables, training details, reproducibility

## Installation

### Requirements
- **GPU:** NVIDIA with CUDA support (compute capability ≥ 7.0)
- **Memory:** 8+ GB GPU RAM
- **CUDA:** 11.0+
- **Python:** 3.9+

```bash
git clone https://github.com/big-brain-zaru/PvsNP.git
cd PvsNP
pip install -r requirements.txt
```

### Dependencies
```
cupy-cuda12x>=13.0.0
numpy>=1.24.0
matplotlib>=3.7.0
scipy>=1.10.0
```

## Usage

```bash
# Main analysis: instance generation, enumeration, solution-graph
# statistics, feature distances, classifier (~25 min on RTX 5070)
python gpu_pnp_breakthrough.py

# Extreme-scale enumeration benchmarks, n=26..30 (~15 min)
python gpu_formalization.py

# Aggregation of the JSON outputs (~3 min)
python ultimate_proof.py

# Regenerate figures
python generate_figures.py
```

Outputs: `gpu_pnp_breakthrough.json`, `formalization_results.json`, `ultimate_proof_results.json`, and `figures/`.

Note on script output: `gpu_formalization.py` also prints an "oracle relativization" section whose values come from a hardcoded simulation stub (fixed multipliers on a baseline constant), and `ultimate_proof.py` prints heuristic verdict labels and an aggregate score. Neither is a measurement and neither is reported in the paper; the scripts are kept as-is so the published JSON files remain exactly reproducible.

## Benchmark summary

- **Total assignments evaluated:** 13,551,672,064
- **Peak throughput:** 979M assignments/sec (n=29; satisfiable instances terminate at the first solution, so per-instance throughput mixes hardware behavior with instance luck)
- **Largest exhaustive instance:** n=30 (1.07B assignments, 7.61 s)
- **GPU:** NVIDIA GeForce RTX 5070 Laptop (36 SM, 8.5 GB)
- **Total runtime:** ~40 minutes

## Repository layout

```
.
├── README.md
├── LICENSE                        # MIT
├── requirements.txt
├── main.tex / main.pdf            # Paper
├── supplement.tex / supplement.pdf# Supplement
├── figures/                       # Generated figures (PDF + PNG)
├── gpu_pnp_breakthrough.py        # Main analysis
├── gpu_formalization.py           # Extreme-scale benchmarks
├── ultimate_proof.py              # JSON aggregation
├── revolutionary_proof.py         # Exploratory script
├── generate_figures.py            # Figure generation
├── gpu_pnp_breakthrough.json      # Main results
├── formalization_results.json     # Benchmark results
└── ultimate_proof_results.json    # Aggregation output
```

## Citation

```bibtex
@misc{zaro2026sat,
  title={GPU-Scale Experiments on Random 3-SAT: Throughput Benchmarks,
         Solution-Space Sampling, and Learned Instance Features},
  author={Zaro, Nadim Faris Nadim},
  year={2026},
  doi={10.5281/zenodo.18451652}
}
```

## Author

**Nadim Faris Nadim Zaro**
Independent Researcher
📧 zaronadim@gmail.com
🔗 [LinkedIn](https://www.linkedin.com/in/nadim-zaro/)

## License

MIT License — see [LICENSE](LICENSE).
