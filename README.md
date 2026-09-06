# NeuroSentry

Deep learning system that detects intracranial hemorrhage (bleeding inside the skull) on non-contrast head CT scans. AI standing watch — flags suspected bleeds in seconds so a radiologist can review them first.

## Goal

**Target:** 90% accuracy on binary hemorrhage detection.
**Precise metric spec:**
- Primary: Area Under the ROC Curve ≥ 0.94 on CQ500 (5-fold cross-validation)
- Secondary: at sensitivity ≥ 90%, specificity ≥ 85%
- Stretch: ≤ 5 percentage-point drop on an external test set

## Team

Four people. Everyone learns everything — no permanent specialist roles. Task-based rotation across four workstreams (data, model, infrastructure, evaluation) over four sprints of two weeks each.

### Rotation table

| Person | Sprint 1 | Sprint 2 | Sprint 3 | Sprint 4 |
|---|---|---|---|---|
| A | MODEL | INFRA | EVAL | DATA |
| B | DATA | MODEL | INFRA | EVAL |
| C | INFRA | EVAL | DATA | MODEL |
| D | EVAL | DATA | MODEL | INFRA |

Whoever *didn't* do the task pair-reviews it (reads the code, writes 2-3 sentences of what they learned). Reviewer is the person one seat below on the table.

### Weekly cadence

- **Async standup** in Discord `#standups`: everyone posts 3 lines daily — *yesterday / today / blocked-by*
- **Weekly sync** (voice, 45 min, fixed day): each person walks the other 3 through their week's task (~10 min each), then plan next sprint

## Sprint plan

### Sprint 1 — foundations (weeks 1-2)
- MODEL: baseline scan-level classifier on qct01-15, eval on qct16-19
- DATA: refactor today's notebook into a proper Python module
- INFRA: GitHub repo scaffold + Weights & Biases setup
- EVAL: metrics module (AUC, sensitivity, specificity, calibration) + Grad-CAM utility

### Sprint 2 — real training (weeks 3-4)
- MODEL: 5-fold patient-level cross-validation
- DATA: patient-level splits, augmentation pipeline
- INFRA: hyperparameter sweeps, config files
- EVAL: per-subtype AUC (IPH, IVH, SDH, SAH), confidence intervals

### Sprint 3 — iterate (weeks 5-6)
- MODEL: compare DenseNet vs EfficientNet vs ResNet, hyperparameter search
- DATA: better series selection, slice-sampling strategies
- INFRA: model registry on Hugging Face Hub
- EVAL: Grad-CAM analysis vs. hemorrhage locations, error case review

### Sprint 4 — external validation + polish (weeks 7-8)
- MODEL: final model selection, ensemble if warranted
- DATA: external dataset ingestion (INSTANCE 2022 or held-out fold)
- INFRA: Streamlit/Gradio demo app, deploy to Hugging Face Spaces
- EVAL: external validation report, final metrics table

## Learning path — mandatory for everyone

### Before Sprint 1 (this week)
- **fast.ai Practical Deep Learning for Coders**, lessons 1-3 — https://course.fast.ai (free, ~6 hours)
- Skim: PyTorch 60-Minute Blitz — https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html

### During Sprints 1-2
- Andrew Ng Deep Learning Specialization, courses 1-2 (audit free on Coursera)
- Read: Chilamkurthy et al. (2018), the original CQ500 paper — https://arxiv.org/abs/1803.05854
- Read: CheXNet — https://arxiv.org/abs/1711.05225

### During Sprints 3-4
- Andrew Ng courses 3-4 (structuring ML projects + CNNs)
- MONAI Bootcamp on YouTube (medical imaging framework)
- Kaggle RSNA ICH competition winner writeups

### Everyone must be able to explain (by end of Sprint 2)
- Transfer learning
- Cross-entropy loss
- AUC vs. accuracy (why AUC in medical AI)
- Class imbalance handling
- CT windowing
- Grad-CAM
- Overfitting signs

## Stack

| Layer | Tool | Cost |
|---|---|---|
| Compute | Google Colab (free T4) + Colab Pro backup ($10/month) | Mostly free |
| Data hosting | Hugging Face Hub (`neurosentry/cq500`) | Free |
| Code | GitHub | Free |
| Experiment tracking | Weights & Biases (student plan) | Free |
| Team chat | Discord | Free |

## Dataset

**CQ500** — 491 head CT scans, 3-radiologist consensus labels for 9 findings (hemorrhage subtypes, fractures, mass effect, midline shift). Mirrored to https://huggingface.co/datasets/neurosentry/cq500.

## Current state

- ✅ Pipeline validated end-to-end on qct01 (20 scans): DICOM loader → series selection → 3-window preprocessing → scan-level dataset → forward pass → AUC
- ✅ Data on Hugging Face Hub (19 tar files, ~40GB)
- ⏭ Next: actual training (Sprint 1 begins)

## Repository setup (Sprint 1 deliverable — placeholder)

To be filled in by whoever owns INFRA in Sprint 1:

```bash
git clone https://github.com/<org>/neurosentry
cd neurosentry
pip install -e .
```
