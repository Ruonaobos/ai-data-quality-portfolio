# Sentiment140 Dataset Quality Audit

A practical **data quality audit** of the classic Sentiment140 dataset (1.6 million tweets) — exactly the kind of analysis real ML teams (Scale AI, Appen, enterprise data pipelines) perform before training models.

This project demonstrates:
- Basic profiling (size, missing values, distributions)
- Detection of common issues (noise, duplicates, inconsistencies, ambiguities)
- Rule-based heuristics + manual review examples
- Clear recommendations for cleaning

## Dataset Overview

- **Name**: Sentiment140
- **Size**: 1,600,000 tweets
- **Labels**: 0 = negative, 4 = positive (noisy labels originally derived from emoticons)
- **Source**: [Kaggle – Sentiment140](https://www.kaggle.com/datasets/kazanova/sentiment140)
- **Columns**: target, ids, date, flag, user, text
- **Languages**: Mostly English (with some informal/slang)

## Key Quality Findings

| Issue Type                        | Count          | % of Dataset | Severity   | Notes / Examples                                                                 |
|-----------------------------------|----------------|--------------|------------|----------------------------------------------------------------------------------|
| Full row duplicates               | 0              | 0.0%         | None       | Clean in this regard                                                             |
| Duplicate **texts**               | 26,968         | ~1.7%        | Medium     | Many repeated tweets – possible copy-paste or retweet noise                      |
| Texts with **inconsistent labels** across duplicates | 2,225 | ~0.14%       | **High**   | Serious annotation problem – same text labeled both positive & negative         |
| Very short / noisy texts (<10 chars) | 3,135       | ~0.2%        | Medium     | Examples: "At work", "Evicted", "headache", "is cold", "SO COLD", "Homework"   |
| Potentially **ambiguous** (mixed positive/negative keywords) | 11,341 | ~0.7%        | Medium-High| Likely sarcasm, irony, or mixed feelings (see examples below)                   |
| Class distribution                | 50.0% / 50.0%  | Balanced     | None       | Perfectly even – no imbalance issue                                              |
| Text length range                 | Min 6 – Max 374<br>Mean ≈ 74.1 | —            | Low        | Some outliers >140 chars (possible concatenation or copy-paste errors)          |
| Missing values                    | 0              | 0.0%         | None       | Very clean in structure                                                          |

### Interesting Examples of Issues

**Very short / noisy texts** (negative label – 0):
- "At work"
- "Evicted"
- "headache"
- "Up early"
- "is cold"
- "is coldd"
- "is tired"
- "SO COLD"

**Potentially ambiguous / mixed-sentiment tweets** (selected from notebook):
- "@oanhLove I hate when that happens..." (label: 0)
- "A bad nite for the favorite teams: Astros and ..." (label: 0)
- "@mercedesashley Damn! The grind is inspirational..." (label: 0)
- "Think I'm going to bed. Goodniight. I hate this" (label: 0)
- "Bad news was Dad has cancer and is dying Good..." (label: 0)

These often contain both positive ("good", "glad", "love") and negative ("hate", "bad", "damn") words — classic cases of sarcasm, irony, or complex emotions.

## Recommendations

1. **Clean noise**  
   - Remove or filter tweets with <10–15 characters  
   - Deduplicate texts (keep one version or drop all duplicates)

2. **Fix inconsistencies**  
   - Prioritize review of the **2,225 texts with conflicting labels** — this is the most serious quality issue

3. **Handle ambiguities**  
   - Manually review or crowdsource labels for ~5–10k most ambiguous samples  
   - Consider advanced techniques: active learning, sarcasm detection models, or adding neutral class

4. **Preprocessing**  
   - Remove URLs, mentions, hashtags (if not needed)  
   - Normalize text (lowercase, fix typos like "coldd")  
   - Consider spell-checking for better quality

5. **Overall suitability**  
   Great scale & balance, but **noisy labels** (emoticon-based + inconsistencies + sarcasm) make it more suitable for robust models than for high-precision applications.

## Files in this Repository

- `dataset_quality_audit.ipynb` → main Jupyter notebook with profiling, stats & visuals  
- `qa_report.md` → concise written summary of findings & recommendations  
- `screenshots/` → add your key plots: class distribution, text length hist, issue summary table, etc.

### Key Visualizations

**Class Distribution** – Perfectly balanced  
![Class Distribution](screenshots/class_distribution_balanced.png)

**Tweet Length Distribution** – Shows typical tweet lengths + some outliers  
![Tweet Lengths](screenshots/tweet_length_distribution_histogram.png)

**Top 20 Most Active Users** – Interesting power-user pattern  
![Top Users](screenshots/top_20_most_active_users.png)

**Very Short/Noisy Texts Examples** – Clear examples of annotation noise  
![Short Texts](screenshots/very_short_noisy_tweets_examples.png)

## Technologies Used

- Python 3
- pandas (data manipulation & profiling)
- matplotlib (visualizations)
- Simple rule-based heuristics + manual sampling

---

Created by Esibe Obokparo  
Feel free to fork, use, or extend this audit for your own datasets!  
Questions? → Open an issue or DM on X: [@Ruonaobos](https://x.com/EsibeObokparo)

⭐ If this helped your ML/data project, give it a star!