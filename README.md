# Predicting Music Popularity on the SpotGenTrack Dataset

**DI 501 — Introduction to Data Informatics (Middle East Technical University)**
**Term Project, Phase 1 (Interim Report)**

Author: **Furkan Turac** · [furkan.turac@metu.edu.tr](mailto:furkan.turac@metu.edu.tr)

---

## Overview

This repository is the replication package for a term project that studies
how well the Spotify popularity score of a track can be predicted from its
objective audio and metadata features, and whether a simple musical
property — the major vs. minor mode — is associated with a statistically
significant difference in popularity.

The dataset is the **SpotGenTrack Popularity Dataset (SPD)** introduced by
Martín-Gutiérrez et al. (IEEE Access, 2020): 101,939 tracks collected from
the Spotify Web API, covering the top-50 playlists across 26 countries.

The project has two phases. This package covers **Phase 1** (interim
report): problem framing, literature review, data-quality checks,
descriptive statistics, and evaluation plan. **Phase 2** (final paper,
due 14 June 2026) will add feature engineering, two trained classifiers
with hyper-parameter tuning, the formal hypothesis test, and an
interpretability analysis.

| Phase | Deliverables                             | Deadline       |
| :---: | ---------------------------------------- | -------------- |
|   1   | Interim report + Phase 1 replication zip | 19 Apr 2026    |
|   2   | Final paper + Phase 2 replication zip    | 14 Jun 2026    |

---

## Research Questions

**RQ1 — Classification (Machine Learning).**
To what extent can the three-tier popularity class of a SpotGenTrack
track be predicted from its audio and metadata features, and which
feature groups contribute most to this prediction?

The continuous popularity score is discretized into Low / Medium / High
using empirical tertile cut-points. This yields a roughly balanced
three-class problem.

**RQ2 — Hypothesis Test.**
Is the mean popularity of tracks written in a major key (`mode = 1`)
significantly different from that of tracks written in a minor key
(`mode = 0`)?

$$ \mathcal{H}_0: \mu_{\text{major}} = \mu_{\text{minor}}
\quad \text{vs.} \quad
\mathcal{H}_1: \mu_{\text{major}} \neq \mu_{\text{minor}} $$

Significance level α = 0.05, two-tailed. Default test: two-sample
Student's *t*-test (with Q–Q plot and Levene's test for assumption
checks, falling back to Mann–Whitney *U* if needed).

The two questions are linked by design: if RQ2 rejects the null, then
`mode` should surface as an important feature in the RQ1 model; if it
does not, the feature-importance analysis acts as a cross-check on the
test's finding.

---

## Key Phase 1 Findings

Based on a full run of `notebooks/01_data_exploration.ipynb` on SPD:

| Quantity                                  | Value                                 |
| ----------------------------------------- | ------------------------------------- |
| Tracks in `tracks.csv`                    | 101,939 rows × 32 columns             |
| Artists in `artists.csv`                  | 56,129 rows × 9 columns               |
| Missing values (`tracks`)                 | 0                                     |
| Missing values (`artists`)                | 1 (in `name`)                         |
| Duplicate rows by (name, artist, duration)| 2,438 (regional re-releases)          |
| Rows with `loudness > 0 dB` (out of range)| 42 (max 2.72 dB; to be clipped)       |
| Popularity mean ± std                     | 39.78 ± 16.79                         |
| Tertile cut-points (t₁ , t₂)              | 33 , 48                               |
| Class distribution (Low / Med / High)     | 32.1% / 34.2% / 33.7%                 |

Popularity statistics (μ ≈ 40, σ ≈ 17) match the values originally
reported by Martín-Gutiérrez et al. (2020), confirming that the
downloaded copy of SPD matches the published dataset.

### Naive baseline results

Stratified 5-fold cross-validation on the full dataset:

| Baseline          | Macro-F1          | Balanced Accuracy  |
| ----------------- | ----------------- | ------------------ |
| Majority-class    | 0.170 ± 0.000     | 0.333 ± 0.000      |
| Stratified random | 0.337 ± 0.003     | 0.337 ± 0.003      |

Any model trained in Phase 2 will need to beat these numbers by a
statistically significant margin, following the protocol of Demšar (JMLR,
2006), to be considered useful.

---

## Repository Layout

```
di501_project/
├── data/
├── notebooks/
│   └── data_exploration.ipynb      # Phase 1 data profiling
├── reports/
│   └── figures/
│       └── popularity_distribution.pdf
├── requirements.txt                  
├── .gitignore
└── README.md                          # this file
```

---

## Reproduction — Phase 1

### 1. Create a Python environment

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### 2. Download the dataset

SPD is distributed by Mendeley Data under a CC-BY-4.0 licence:

> Martín-Gutiérrez D., Hernández Peñaloza G., Belmonte-Hernández A.,
> Álvarez García F. (2019). *SpotGenTrack Popularity Dataset* (V1)
> [Data set]. Mendeley Data. <https://doi.org/10.17632/4m2x4zngny.1>

Download the archive and extract it into `data/raw/`. The exact layout
of the archive varies slightly between mirrors (for example
`Data Sources/spotify_tracks.csv`); the notebook auto-detects the CSV
files by partial filename match, so the folder structure underneath
`data/raw/` does not matter.

Only the Spotify audio and metadata tables are used — the Genius-derived
lyric table that ships with SPD is intentionally excluded from this
project.

### 3. Run the notebook

```bash
jupyter notebook notebooks/data_exploration.ipynb
```

Run all cells top to bottom. The notebook:

1. Locates and loads the `tracks` and `artists` CSVs.
2. Runs the five data-quality checks (completeness, uniqueness,
   validity, consistency, distributional checks).
3. Produces the descriptive-statistics table for Table I of the paper.
4. Regenerates Figure 1 (popularity distribution with tertile
   cut-points) into `reports/figures/`.
5. Evaluates the two naive baselines.
6. Prints a group summary for the RQ2 test (major vs. minor
   popularity).


## AI Usage Statement

Generative AI tools were used for language editing and as an aid during
the literature search. All content was reviewed and verified by the
author.

## Licence

Code in this repository is released under the MIT licence.
The SPD dataset is distributed by its original authors under CC-BY-4.0;
please cite both the dataset and the companion paper, as listed in
`reports/references.bib`.
