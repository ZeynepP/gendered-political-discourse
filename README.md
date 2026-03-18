# Anatomy of an Election from a Gender Perspective

**Zeynep Pehlivan, Esli Chan**
McGill University

Published at **ICWSM 2026** (International AAAI Conference on Web and Social Media)

This repository contains the code and data for our paper analyzing how gender shapes online political communication during the 2025 Canadian federal election. Drawing on 193,620 social media posts from 921 candidates across five platforms (X/Twitter, Instagram, TikTok, Bluesky, YouTube), we employ transformer-based topic modeling and a linguistic gender axis to quantify communicative patterns.


## Key Findings

1. A statistically robust stylistic difference exists between the discourse of male and female candidates (KS D = 0.26, p < 10^-12, AUC = 0.67).
2. This gendered linguistic gap persists systematically across all major political parties (AUC 0.70-0.77), demonstrating it is not merely an artifact of ideology.
3. Women more frequently employ community-action framing across policy-related discussions, while men place greater emphasis on economic and industrial issues.

## Project Structure

```
gender-canada-elections/
│
├── data/
│   ├── canada_2025_candidates.jsonl      # Official candidate list (1,562 candidates)
│   ├── canadian_election_2025_posts.jsonl # 193,620 social media posts
│   └── embedding_state.json              # Aggregated embeddings per candidate
│
├── gender-analysis.ipynb                 # Main analysis notebook (all figures)
├── bck/                                  # Backup/draft versions of analysis
├── requirements.txt                      # Python dependencies
├── ICWSM_GENDER_cl.pdf                   # Paper (camera-ready)
└── README.md
```

## Data

The dataset covers a 95-day window from February 23 to May 28, 2025, including the official 37-day campaign period (March 23 - April 28).

| Platform   | # Cand. | # Female | # Male | # Posts  |
|------------|---------|----------|--------|----------|
| X/Twitter  | 630     | 185      | 445    | 132,071  |
| Instagram  | 732     | 269      | 463    | 45,152   |
| TikTok     | 62      | 27       | 35     | 1,500    |
| Bluesky    | 176     | 74       | 102    | 13,506   |
| YouTube    | 100     | 36       | 64     | 1,391    |
| **Total**  | **1,700** | **591** | **1,109** | **193,620** |

*Note: A candidate can have accounts on multiple platforms.*

**Data fields (posts):**
- `id`, `date`, `platform`, `gender`, `party`, `candidate_id`, `candidate_name`
- `topic_id`, `topic_label`, `topic_probability` (BERTopic)
- `original` (true = original post, false = retweet/repost)
- `like_count`

**Embedding state:** Aggregated post embeddings per candidate (`sum` + `count`) using the multilingual sentence-transformer `paraphrase-multilingual-MiniLM-L12-v2`.

## Analyses

The notebook `gender-analysis.ipynb` reproduces all figures and analyses from the paper:

| Figure | Description |
|--------|-------------|
| Fig. 1 | Gender distribution of candidates by political party |
| Fig. 2 | Gender Bias Score (GBS) by platform and party |
| Fig. 3 | Distribution of likes for original content by gender and party |
| Fig. 4 | Temporal posting dynamics by gender and party (7-day rolling avg) |
| Fig. 5 | Thematic divergence by gender (per-candidate normalized topic shares) |
| Fig. 6 | Distributions of alignment scores on the gender axis |
| Fig. 7 | Sentiment distribution across gender axis quartiles |
| Fig. 8 | Candidate-level gender axis alignment vs. mean sentiment |
| Fig. 9 | Gendered linguistic styles across political parties (violin plots) |
| Fig. 10 | Temporal trajectories of topic-level alignment on the gender axis |

### Methods

- **Gender Bias Score (GBS):** `(# Posts by Male / # Unique Male) - (# Posts by Female / # Unique Female)` — normalizes for the male-heavy candidate pool
- **Topic Modeling:** BERTopic with multilingual sentence-transformer embeddings; 58% of posts classified as outliers by HDBSCAN (expected for short, noisy social media text)
- **Gender Axis:** Constructed from candidate-level embedding centroids following Bolukbasi et al. (2016); validated via AUC, KS tests, and permutation tests
- **Sentiment Analysis:** Multilingual DistilBERT-based classifier (tabularisai/multilingual)

## Usage

```bash
pip install -r requirements.txt
jupyter notebook gender-analysis.ipynb
```

## Reproducibility

All experiments were conducted on the Digital Alliance of Canada research infrastructure (Fir cluster) using a single NVIDIA H100 GPU. Topic modeling and embedding extraction took ~3 hours; sentiment analysis ~2 additional hours.

## Requirements

- Python 3.9+
- pandas, numpy, matplotlib, seaborn
- scikit-learn, scipy
- Jupyter Notebook

## Citation

If you use this code or data, please cite:

```bibtex
@inproceedings{pehlivan2026anatomy,
  title={Anatomy of an Election from a Gender Perspective},
  author={Pehlivan, Zeynep and Chan, Esli},
  booktitle={Proceedings of the International AAAI Conference on Web and Social Media (ICWSM)},
  year={2026}
}
```

## License

Copyright 2026, Association for the Advancement of Artificial Intelligence (www.aaai.org). All rights reserved.
