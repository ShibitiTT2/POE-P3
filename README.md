README — Portfolio of Evidence: Image Recognition with CNNs
Project: Image Recognition with Convolutional Neural Networks (CNN)
Client: Publisher premises facial/image recognition (proof-of-concept)

Submission date: November 2026

1. What This Submission Contains
This portfolio demonstrates the design, training, evaluation, and refinement of a Convolutional Neural Network (CNN) for image recognition. The work is delivered as a complete, reproducible pipeline that a marker or client can re-run end-to-end.

Deliverable	Purpose	Approx. size
POE_Report.docx	Client-facing report (10 sections + 4 appendices, IIE Harvard referenced)	~2 MB
POE_CNN_Analysis.ipynb	Jupyter notebook with executed code, outputs, and figures	~5 MB
figures/	15 figures used in the report	~2 MB
models/	Saved model weights for 3 trained CNNs	~10 MB
results/	Machine-readable metrics and cleaning logs (JSON/CSV)	<1 MB
README.md	This file	—
Total: ~20 MB (unzipped).

2. Quick Start (5 minutes)
2.1 To view the report
Open POE_Report.docx in Microsoft Word (or LibreOffice). Regenerate the Table of Contents via References → Table of Contents if needed.

2.2 To view the analysis
Open POE_CNN_Analysis.ipynb in Jupyter Notebook, JupyterLab, or VS Code. All cells are pre-executed, so the notebook displays all outputs and figures without re-running.

2.3 To re-run the analysis (reproducibility)
bash
# 1. Create a virtual environment
python -m venv poe_env
source poe_env/bin/activate       # macOS/Linux
poe_env\Scripts\activate          # Windows

# 2. Install dependencies
pip install -r requirements.txt

# 3. Launch the notebook
jupyter notebook POE_CNN_Analysis.ipynb

# 4. Run all cells
#    In Jupyter: Cell → Run All
Expected runtime: ~45–60 minutes on CPU (no GPU required). The three training runs dominate the runtime; EDA and evaluation are fast.

Expected output:

All figures regenerated in figures/

Model weights regenerated in models/

results/metrics.json regenerated with the same numbers (to within ±0.1 pp on CPU)

3. Dataset
CIFAR-10 (Canadian Institute for Advanced Research) — 60,000 32×32 colour images across 10 classes.
Source: Krizhevsky, A. (2009) Learning Multiple Layers of Features from Tiny Images. Master's thesis, University of Toronto.

Automatically downloaded on first notebook run (~170 MB, cached locally).

No personal data — no POPIA/GDPR exposure in this portfolio.

CIFAR-10 acts as a controlled proxy for facial recognition: same tensor shape (H, W, C) and same multi-class structure. The pipeline transfers directly to a face-specific dataset (VGGFace2, LFW) in phase 2.

4. Requirements
requirements.txt (already included — this is what it installs):

text
torch>=2.0
torchvision>=0.15
numpy>=1.24
pandas>=2.0
scipy>=1.10
scikit-learn>=1.3
scikit-image>=0.20
matplotlib>=3.7
seaborn>=0.12
statsmodels>=0.14
jupyter>=1.0
Python version: 3.10 or later.
Hardware: Any CPU from the last 5 years. GPU is optional (training runs ~5× faster on CUDA).
OS: Tested on macOS 14, Ubuntu 22.04, Windows 11.

5. Reproducibility Guarantees
Control	Value
Random seed	42 (Python, NumPy, PyTorch)
Python version	3.10+
PyTorch version	2.0+
Notebook execution	Top-to-bottom, all outputs saved
Model weights	Saved after each training run
Metrics	Written to results/metrics.json
Figures	Written to figures/
Hardware	CPU only (no GPU required)
Re-running the notebook with seed 42 reproduces every number in the report to within ±0.1 pp. The only source of small variation is non-deterministic CPU reduction order in PyTorch, which is industry-standard and documented in the notebook.

6. Headline Results
Model	Test accuracy	Macro F1	Epochs	Parameters	Training time (s)
Model 1 — baseline	73.70%	0.7357	10	554,346	704.72
Model 2 — improved	83.45%	0.8342	18	242,474	1,406.72
Model 3 — tuned	77.86%	0.7770	12	242,474	921.34
Statistical significance: McNemar χ² = 468.95, p = 5.44 × 10⁻¹⁰⁴ (Model 2 vs Model 1).

Success criteria from §4.1 of the report:

Metric	Target	Achieved	Met?
Test accuracy	≥ 75%	83.45%	✅
Macro F1	≥ 0.73	0.8342	✅
Improvement over baseline	≥ +8 pp	+9.75 pp	✅
Worst-class recall	≥ 0.60	0.73 (cat)	✅
Reproducibility	Identical on re-run	Seed 42	✅
7. Folder Structure
text
PoE_CNN_Submission/
│
├── README.md                            ← this file
├── requirements.txt                     ← pip dependencies
├── POE_Report.docx                      ← main report (client-facing)
├── POE_CNN_Analysis.ipynb               ← executed analysis notebook
│
├── figures/                             ← 15 figures (referenced in report)
│   ├── fig01_sample_grid.png
│   ├── fig02_class_means.png
│   ├── fig03_class_balance.png
│   ├── fig04_pixel_histograms.png
│   ├── fig05_class_statistics.png
│   ├── fig06_quality_descriptors.png
│   ├── fig07_near_duplicates.png
│   ├── fig08_similarity.png
│   ├── fig09_pca_scree.png
│   ├── fig10_pca_projection.png
│   ├── fig11_cleaning_effect.png
│   ├── fig12_learning_curves.png
│   ├── fig13_confusion_matrix.png
│   ├── fig14_roc_pr.png
│   └── fig15_gradcam.png
│
├── models/                              ← saved PyTorch weights
│   ├── model1_baseline.pt
│   ├── model2_improved.pt
│   └── model3_tuned.pt
│
└── results/                             ← machine-readable outputs
    ├── metrics.json                     ← all metrics for all 3 models
    ├── cleaning_log.csv                 ← per-rule image removal trail
    └── per_class_metrics.csv            ← P/R/F1 per class for Model 2
8. What Each Notebook Section Does
Notebook section	What it does	Corresponds to report section
§1 — Setup	Imports, seeds, path config	—
§2 — Data loading	Downloads and loads CIFAR-10	§3
§3 — Data quality	Shape/dtype/NaN/duplicate checks	§3.4
§4 — EDA	11 figures: class balance, sample grid, statistics, PCA, similarity, duplicates	§5.1–5.5
§5 — Cleaning	Removes near-duplicates; writes cleaning_log.csv	§5.6
§6 — Feature selection	ANOVA, forward selection, PCA, L1	§6
§7 — Model 1 (baseline)	Trains baseline CNN; saves model1_baseline.pt	§7.2
§8 — Model 1 evaluation	Confusion matrix, per-class metrics, curves	§8.1–8.4
§9 — Model 2 (improved)	Trains improved CNN with augmentation; saves model2_improved.pt	§7.2
§10 — Model 2 evaluation	Full metrics + McNemar test vs Model 1	§8.1–8.8
§11 — Model 3 (tuned)	Trains third model; saves model3_tuned.pt	§7.2
§12 — Grad-CAM	Generates attention overlays	§8.7
§13 — Error vs quality	Correlates errors with brightness/blur/contrast	§8.6
§14 — Reproducibility	Verifies seed, writes metrics.json	Appendix C
9. Reference List
Full IIE Harvard-formatted reference list appears at the end of POE_Report.docx. The primary sources are:

Krizhevsky (2009) — CIFAR-10 dataset

Goodfellow, Bengio & Courville (2016) — Deep Learning (MIT Press)

Kingma & Ba (2015) — Adam optimiser

Dietterich (1998) — McNemar test

Selvaraju et al. (2017) — Grad-CAM

Pedregosa et al. (2011) — scikit-learn

Paszke et al. (2019) — PyTorch

Republic of South Africa (2013) — POPIA

European Parliament and Council (2016) — GDPR

Full citations (35+ sources) are in the report's Reference List.

10. Known Limitations
Limitation	Impact	Mitigation
CIFAR-10 is 32×32 — insufficient detail for facial recognition	~85–90% accuracy ceiling	Stated in §3.5; upgrade path to VGGFace2/LFW in §9.3
Web-scraped images ≠ premises camera footage	Domain shift	Premises-specific validation set required in phase 2 (§9.3)
Colour/background shortcuts	Grad-CAM shows some failures focus on context	Background randomisation recommended (§9.3)
No facial data in this portfolio	End goal not directly demonstrated	Brief explicitly allows non-facial proxy; pipeline transfers
CPU training is slow	45–60 min end-to-end	GPU reduces to ~10 min
These limitations are not defects of the analysis — they are honest scope boundaries, stated explicitly in §3.5 and §9.2 of the report so the client can make an informed deployment decision.

11. Support and Contact
Issue	Resolution
Figures missing on Word export	Re-run §4–§12 of the notebook; verify files exist in figures/
Accuracy differs from report	Confirm seed = 42 in random, numpy, torch; confirm CPU (not GPU) for exact reproducibility
Jupyter fails to load notebook	pip install --upgrade jupyter ipykernel; ensure Python 3.10+
Memory error during training	Reduce batch size from 64 to 32 in §7.2 (accuracy impact: <0.3 pp)
PyTorch CUDA errors	Force CPU by setting device = torch.device('cpu') in §1
CIFAR-10 download fails	Manually download from https://www.cs.toronto.edu/~kriz/cifar.html and place in ~/.cache/torch/datasets/
For questions about the analysis: contact ______________________.

12. Submission Checklist
Before final submission, verify:

□ POE_Report.docx opens and all 15 figures render (no broken image placeholders)
□ Table of Contents regenerated via References → Table of Contents
□ Every table has "Repeat header row" enabled
□ No {{PLACEHOLDER}} strings remain (search for {{ should return 0 matches)
□ POE_CNN_Analysis.ipynb opens and shows all outputs (all cells executed)
□ All 15 figures present in figures/
□ All 3 model weights present in models/
□ results/metrics.json present and matches the numbers in the report
□ requirements.txt present
□ Report references resolve — every (Author, Year) in text has a matching entry in the Reference List
□ Student name and number filled in on the title page of the report
□ Zip the folder as PoE_CNN_Submission_<StudentNumber>.zip
Final check: search the report for the string {{. Zero matches means every placeholder has been filled.

13. Version History
Version	Date	Change
1.0	Nov 2026	Initial submission: EDA, 3 models, report
1.1	Nov 2026	Added §3.6 pitfalls table, §4.5 risk register, §7.3 code walkthrough, §8.x rationale paragraphs, Appendices B–D
