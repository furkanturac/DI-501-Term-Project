# Spotify Track Popularity Prediction — Phase 1

**DI 501 Term Project** | Replication Study

## Overview

This repository contains Phase 1 of a multi-phase research study on predicting Spotify track popularity from audio features. Phase 1 covers data exploration, baseline classifier evaluation, and a preliminary hypothesis test on musical mode.

## Research Questions

- **RQ1:** Can audio features (danceability, energy, valence, etc.) predict whether a track falls in the Low, Medium, or High popularity tier?
- **RQ2:** Does musical mode (major vs. minor key) correlate with track popularity?

## Dataset

**SpotGenTrack Popularity Dataset (SPD)**

| File | Records | Description |
|------|---------|-------------|
| `spotify_tracks.csv` | 101,939 | Track audio features + popularity score |
| `spotify_artists.csv` | 56,129 | Artist metadata (followers, genres) |
| `spotify_albums.csv` | — | Album-level metadata |
    
> Raw data files are excluded from version control (see `.gitignore`). Download the dataset separately and place it under `data/raw/`.

## Repository Structure

```
replication_phase1/
├── 01_data_exploration.ipynb   # Main analysis notebook
├── requirements.txt            # Python dependencies
├── data/
│   └── raw/                    # Place downloaded CSV files here
└── reports/
    └── figures/                # Generated plots
```

## Setup

```bash
# 1. Create and activate a virtual environment
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# 2. Install dependencies
pip install -r requirements.txt

# 3. Launch Jupyter
jupyter notebook
```

Then open `01_data_exploration.ipynb`.

## Results

Popularity was binned into three tiers (Low / Medium / High) using tertile cutpoints. Baseline results confirm the task is non-trivial and motivate feature-based models in later phases.

## Dependencies

- Python ≥ 3.10
- numpy ≥ 1.26, pandas ≥ 2.1, scipy ≥ 1.11
- scikit-learn ≥ 1.4
- matplotlib ≥ 3.8, seaborn ≥ 0.13
- notebook ≥ 7.0, ipykernel ≥ 6.28
