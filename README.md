# Compressing DNABERT-2 via Knowledge Distillation for Lightweight DNA Promoter Classification

**CSE 472 — Machine Learning Sessional, BUET**

**Authors:** Anup Halder Joy (2005005), Sheikh Rahat Mahmud (2005048)  
**Supervisor:** Md. Roqunuzzaman Sojib, Lecturer, Dept. of CSE, BUET

---

## Overview

This project applies **Knowledge Distillation (KD)** to compress the 117M-parameter DNABERT-2 genomic language model into lightweight student models for DNA promoter classification. We explore two families of student architectures:

1. **DNABERT-2-initialized students** — inherit pretrained encoder layers
2. **Pure architectures** — trained entirely from scratch

### Key Results

| Model | Params | Binary Acc | Mouse Acc | GPU Speedup | CPU Speedup |
|---|---|---|---|---|---|
| Teacher (DNABERT-2) | 117.1M | 96.71% | 79.66% | 1.00x | 1.00x |
| Hybrid CNN-3L (KD) | 26.3M | **93.31%** | **82.61%** | 1.71x | **2.70x** |
| 4L-Pure-Transformer (KD) | 32.5M | 93.01% | 82.24% | 1.37x | 2.12x |

- Distilled students **outperform the teacher** on cross-species mouse promoters
- KD consistently improves pure architectures by ~1% even without pretrained weights
- Data augmentation is critical for effective KD on small multiclass datasets

---

## Repository Structure

```
.
├── paper/
│   ├── ieee/                  # IEEE conference format paper
│   │   ├── IEEE_paper.tex
│   │   ├── IEEE_paper.pdf
│   │   └── IEEEtran.cls
│   ├── neurips/               # NeurIPS format paper
│   │   ├── paper_neurips.tex
│   │   ├── paper_neurips.pdf
│   │   └── neurips_2024.sty
│   └── figures/               # Shared visualization figures
│
├── notebooks/
│   ├── binary_kd/             # Binary promoter KD experiments
│   │   ├── 01_teacher_training.ipynb
│   │   ├── 02_alpha_temp_grid_hybrid.ipynb
│   │   ├── 03_alpha_temp_grid_part3.ipynb
│   │   ├── 04_alpha_temp_grid_part4.ipynb
│   │   └── 05_alpha_temp_4layer_standard.ipynb
│   ├── multiclass_kd/         # 5-class multiclass KD experiments
│   │   ├── 01_multiclass_distillation.ipynb
│   │   ├── 02_multiclass_run_output.ipynb
│   │   └── 03_multiclass_augmented_run_output.ipynb
│   ├── pure_architecture/     # Pure architecture (from scratch) experiments
│   │   ├── 01_pure_arch_baseline_and_kd.ipynb
│   │   └── 02_pure_arch_run_output.ipynb
│   └── inference_and_generalization/  # Speed benchmarks & cross-species eval
│       ├── 01_hybrid_generalization_speed.ipynb
│       ├── 02_hybrid_inference_run_output.ipynb
│       ├── 03_pure_arch_generalization_speed.ipynb
│       └── 04_pure_arch_inference_run_output.ipynb
│
├── results/
│   ├── binary_kd/             # Binary KD result visualizations
│   ├── multiclass_kd/         # Multiclass result visualizations
│   ├── pure_architecture/     # Pure architecture results & JSON
│   └── inference_and_generalization/  # Cross-species & speed charts
│
├── proposal/
│   └── CSE_472_project_proposal.pdf
│
└── README.md
```

---

## Experiments

### 1. Binary Promoter Classification (Human)

- **Dataset:** InstaDeepAI Nucleotide Transformer Downstream Tasks — `promoter_all` (53K train, 5.9K test, 300bp)
- **Grid search:** alpha in {0.3, 0.5, 0.7}, T in {1.0, 3.0, 5.0}
- **Best config:** alpha=0.3, T=3.0 for Hybrid CNN-3L (93.31%)

### 2. Multiclass Classification (5-class)

- **Task:** Promoter subtype classification (13.6K samples)
- **Key finding:** Data augmentation converts KD from harmful (-1.4%) to beneficial (+1.5%)

### 3. Pure Architecture Experiments

| Architecture | Params | Baseline | KD | Gain |
|---|---|---|---|---|
| 4L-Pure-Transformer | 32.5M | 91.98% | 93.01% | +1.03% |
| 4L-Pure-CNN | 3.1M | 83.83% | 84.83% | +1.00% |
| Hybrid-Pure-3T1C | 26.3M | 91.84% | 92.85% | +1.01% |

### 4. Cross-Species Generalization

Models trained on human promoters evaluated on:
- **Mouse** (DeePromoter, 21K sequences) — students outperform teacher
- **Plant** (13 species, 8.3K sequences) — near-random for all models

### 5. Inference Speed

Benchmarked on T4 GPU, 1000 sequences, batch size 16.

---

## How to Run

All notebooks are designed for **Kaggle** with 2x NVIDIA T4 GPUs:

1. Upload the teacher training notebook and run it first
2. Use the teacher output as input for student KD notebooks
3. Use student outputs as input for inference/generalization notebooks

**Requirements:** `transformers`, `datasets`, `accelerate`, `scikit-learn`, `einops`

---

## Citation

If you use this work, please cite:

```bibtex
@inproceedings{joy2025compressing,
  title={Compressing DNABERT-2 via Knowledge Distillation for Lightweight DNA Promoter Classification},
  author={Joy, Anup Halder and Mahmud, Sheikh Rahat and Sojib, Md. Roqunuzzaman},
  year={2025},
  institution={Bangladesh University of Engineering and Technology}
}
```
