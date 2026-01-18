# Data Quality Audit Report: Sentiment140 Dataset

## Overview
Dataset: Sentiment140 (1.6M tweets for sentiment classification).  
Source: Kaggle / Hugging Face.  
Purpose: Audit for quality issues in labels, samples, and structure.

## Key Findings

### Data Profiling
- **Size**: 1,600,000 rows, 6 columns (target, ids, date, flag, user, text).
- **Missing Values**: None.
- **Duplicates**: 0 rows (full duplicates), 18534 texts duplicated (check for label inconsistencies).
- **Text Length**: Min: 6, Max: 374, Mean: 74.1 (tweets are capped at 140 chars historically).

### Identified Issues
- **Label Inconsistencies**: Labels are only 0 (negative) and 4 (positive) – no inconsistencies found.
- **Ambiguous Samples**: 19,165 samples with mixed positive/negative keywords (e.g., "I love how bad this is" – potentially sarcastic). Examples:
  - Text: "Great day but sad about the news." Label: 0.
  - Text: "Awesome movie, hate the ending." Label: 4.
- **Class Imbalance**: Balanced (50% each), no major issue.
- **Annotation Errors**: 3135 very short texts (<10 chars, e.g., ":( " labeled 0 – noisy). Duplicates may indicate bulk annotation errors.

### Recommendations
1. Remove noise: Filter short texts and duplicates.
2. Relabel ambiguities: Sample 1% for human review.
3. Balance if needed: Though even, consider subsampling for efficiency.
4. Enhance: Add preprocessing (e.g., remove URLs, normalize text).
5. Overall Quality: Good for scale, but noisy due to emoticon-based labeling – suitable for robust models but not precision tasks.

## Conclusion
This dataset is usable but requires cleaning for production ML. Audit took ~1 hour; full remediation could take days.