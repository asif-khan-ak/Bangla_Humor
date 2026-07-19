# BH-BERT: A Balanced Bengali Humor Detection Dataset

**Companion dataset for:** *BanglaHumorBERT (BH-BERT): A Domain-Adaptive Transformer Approach to Context-Aware Humor Detection in Bengali Language Processing* (Springer, accepted)

**Owner:** [Asif Khan](https://github.com/asif-khan-ak)
**Repository:** `https://github.com/asif-khan-ak/Bangla_Humor`

---

## Table of Contents

1. [Overview](#1-overview)
2. [Motivation](#2-motivation)
3. [Repository Structure](#3-repository-structure)
4. [Dataset Description](#4-dataset-description)
5. [Data Collection](#5-data-collection)
6. [Annotation Guidelines](#6-annotation-guidelines)
7. [Dataset Statistics](#7-dataset-statistics)
8. [Data Quality and Validation](#8-data-quality-and-validation)
9. [Preprocessing](#9-preprocessing)
10. [Recommended Data Splits](#10-recommended-data-splits)
11. [Example Instances](#11-example-instances)
12. [Ethical Considerations and Privacy](#12-ethical-considerations-and-privacy)
13. [Known Limitations](#13-known-limitations)
14. [Baseline Results](#14-baseline-results)
15. [How to Load the Dataset](#15-how-to-load-the-dataset)
16. [License](#16-license)
17. [Citation](#17-citation)
18. [Changelog](#18-changelog)
19. [Contact](#19-contact)

---

## 1. Overview

This repository hosts the dataset introduced alongside **BanglaHumorBERT (BH-BERT)**, a two-stage domain-adaptive transformer framework for context-aware humor detection in the Bengali language. The dataset is a **manually annotated, binary-labeled, class-balanced collection of 8,400 Bengali text instances**, created to address the scarcity of dedicated humor-detection resources for Bengali — a low-resource language in which prior NLP work has concentrated mainly on sentiment analysis and sarcasm detection rather than humor as a distinct phenomenon.

| | |
|---|---|
| **Language** | Bengali (Bangla script) |
| **Task** | Binary text classification — Humor (1) vs. Non-Humor (0) |
| **Size** | 8,400 instances |
| **Class balance** | 4,200 Humor / 4,200 Non-Humor (perfectly balanced) |
| **Annotators** | 3 native Bengali speakers |
| **Inter-annotator agreement** | Fleiss' Kappa = 0.82 (strong agreement) |
| **Format** | `.xlsx` (also mirrored as `.csv`) |
| **License** | See [Section 16](#16-license) |

---

## 2. Motivation

Humor detection in Bengali NLP has lagged behind related tasks such as sentiment analysis and sarcasm detection. Sarcasm is only one narrow slice of the broader phenomenon of humor, and models trained on sarcasm data do not transfer well to jokes, puns, one-liners, or general comedic text. Prior Bengali-language efforts in this space have relied on very small corpora (e.g., datasets on the order of 1,600 instances), which limits both statistical reliability and the ability of transformer models to learn robust humor-specific representations.

This dataset was built specifically to close that gap: it is five times larger than the largest prior Bengali humor resource we are aware of, drawn from a diverse mix of real-world social platforms, and strictly balanced to avoid class-imbalance bias during model training.

---

## 3. Repository Structure

```
.
├── README.md                          # This file
├── data/
│   └── bh_humor_bn.xlsx                # Full dataset (8,400 labeled instances)
├── figures/
│   └── label_length_distribution.png   # Class balance & length-distribution plot
├── LICENSE                             # License terms (see Section 16)
└── CITATION.cff                        # Citation metadata (optional, recommended)
```

> Adjust file/folder names above to match what you actually upload — this structure is a suggested layout, not a requirement.

---

## 4. Dataset Description

The dataset is distributed as a single labeled sheet (`BH-BERT`) containing three columns:

| Column | Type | Description |
|---|---|---|
| `id` | integer | Unique sequential instance identifier (1–8,400) |
| `text` | string (Bengali) | The raw Bengali text instance |
| `label` | integer (0/1) | Binary gold label: `1` = Humor, `0` = Non-Humor |

There are **no missing values** in any of the three columns across all 8,400 rows.

Each instance is a short, self-contained piece of real-world Bengali text — a joke, pun, one-liner, meme caption, comment, or conversational snippet — rather than a long document. This mirrors how humor naturally occurs "in the wild" on social media, where it is typically expressed in short, punchy units rather than extended narrative.

---

## 5. Data Collection

**Sources.** Data were collected from five widely used social media platforms: **YouTube, Facebook, Instagram, Reddit, and X (Twitter)**. This cross-platform strategy was deliberately chosen to capture how humor manifests differently across distinct communicative contexts and audience communities within Bengali digital spaces, rather than reflecting the style of any single platform.

**Collection method.** Instances were gathered through a combination of:
- **Targeted keyword searches** for humor-indicative Bengali terms, phrases, and formats (jokes, puns, one-liners, meme text).
- **Contextual analysis** of posts, comments, captions, and discussion threads, so that non-humorous content could be drawn from directly comparable everyday contexts (news, informational posts, captions, general chat) rather than from a mismatched or artificially "serious" register.

**Class definitions at the source level.**
- *Humorous instances*: jokes, puns, one-liners, memes, and comedic conversational content.
- *Non-humorous instances*: general-purpose content such as news snippets, plain comments, captions, informational postings, and everyday conversational text.

This design ensures the negative (non-humor) class is realistic and topically diverse rather than a trivial contrast class, which is important for the classifier to learn genuine humor-specific cues rather than superficial stylistic shortcuts.

---

## 6. Annotation Guidelines

### 6.1 Annotators
All 8,400 instances were manually labeled by **three native Bengali speakers** with prior familiarity with Bengali social media language conventions and background in NLP-related annotation tasks.

### 6.2 Labeling scheme
Each instance was assigned a single binary label:

- **`y = 1` (Humor)** — the text is intended to amuse, and is recognized by annotators as a joke, pun, witty remark, one-liner, meme-style caption, or otherwise comedic statement, judged from its perceived communicative intent and contextual interpretation.
- **`y = 0` (Non-Humor)** — the text is a straightforward, non-comedic statement: news, informational content, plain commentary, or everyday conversational text with no humorous intent.

### 6.3 Handling ambiguous cases
Humor is inherently subjective and context-dependent — a statement that reads as funny to one annotator may read as neutral to another, particularly for culturally embedded references or subtle wordplay. To manage this:

1. Each instance was independently reviewed by the annotation team.
2. Instances flagged as ambiguous were **reviewed more carefully** and discussed among annotators.
3. Final labels for ambiguous cases were resolved via **majority agreement** across the three annotators (i.e., at least 2 of 3 agreeing on the label).

### 6.4 Inter-annotator agreement
Agreement across annotators was quantified using **Fleiss' Kappa**, yielding a score of **0.82**, which is generally interpreted as *strong* (almost-perfect) agreement. This indicates that, despite the subjective nature of humor, the annotation protocol produced highly consistent and reliable labels.

### 6.5 Class balancing
After annotation, the dataset was curated down to an exactly balanced set: **4,200 humor** and **4,200 non-humor** instances. This was a deliberate design decision to eliminate class imbalance and reduce the risk of the downstream model developing a majority-class bias.

### 6.6 What this dataset does *not* label
Per the accompanying paper's stated limitations, this dataset supports only **binary** humor classification. It does **not** distinguish between different sub-types of humor (e.g., sarcasm, satire, irony, wordplay), and it is **text-only** — it does not include multimodal signals (images, audio, or video) that often accompany humor in memes or video content on social platforms. Researchers interested in humor sub-typing or multimodal humor should treat this as a foundational text resource rather than a complete solution.

---

## 7. Dataset Statistics

All figures below were computed directly from the released data (8,400 rows, 0 missing values).

### 7.1 Class distribution

| Label | Meaning | Count | Percentage |
|---|---|---|---|
| 1 | Humor | 4,200 | 50.00% |
| 0 | Non-Humor | 4,200 | 50.00% |
| **Total** | | **8,400** | **100%** |

### 7.2 Text length statistics (character-level)

| Statistic | Overall | Humor (1) | Non-Humor (0) |
|---|---|---|---|
| Mean | 72.15 | 68.93 | 75.37 |
| Std. dev. | 30.51 | 26.55 | 33.71 |
| Minimum | 12 | 12 | 18 |
| 25th percentile | 51 | 51 | 52 |
| Median | 68 | 66 | 69 |
| 75th percentile | 86 | 82 | 92 |
| Maximum | 320 | 320 | 291 |

### 7.3 Text length statistics (word-level)

| Statistic | Overall | Humor (1) | Non-Humor (0) |
|---|---|---|---|
| Mean | 12.62 | 12.26 | 12.98 |
| Std. dev. | 5.27 | 4.65 | 5.79 |
| Minimum | 3 | 3 | 3 |
| Median | 12 | 12 | 12 |
| Maximum | 57 | 57 | 46 |

Humorous instances are, on average, slightly *shorter* than non-humorous instances at the character level — consistent with the "punchy," one-liner style typical of jokes and memes — while the two classes are almost identical in typical word count, indicating that class differences are semantic/pragmatic rather than a simple function of raw length.

### 7.4 Script and character composition

- **100%** of instances are written natively in the Bengali (Bangla) script.
- **0%** of instances contain Latin-alphabet (English) characters.
- Only **1 instance** (0.01%) contains a numeral.
- **0%** of instances contain emoji or pictographic Unicode symbols.

This confirms the dataset is a clean, monolingual Bengali-script resource with negligible code-mixing — useful information for tokenizer and preprocessing design.

### 7.5 Visualization

 <img width="1650" height="630" alt="label_length_distribution" src="https://github.com/user-attachments/assets/d57d45a1-e4e9-4081-96f5-80b497afc153" />

*Left: word-length distribution by class. Right: class balance (4,200 vs. 4,200).*

---

## 8. Data Quality and Validation

The following checks were run against the released file as part of preparing this repository:

| Check | Result |
|---|---|
| Missing values (`id`, `text`, `label`) | None — 0 missing across all 8,400 rows |
| Label validity | All labels strictly in `{0, 1}` |
| Class balance | Exactly 4,200 / 4,200 |
| Non-Bengali character contamination | None detected (no Latin characters, no emoji) |
| Empty or whitespace-only text | None |


**Reliability validation.** Beyond automated checks, the primary quality safeguard for this dataset is human: **three independent annotators** labeled the data, ambiguous cases underwent a secondary review pass, and inter-annotator agreement was formally measured (Fleiss' Kappa = 0.82), rather than relying on a single annotator's judgment or heuristic/weak labeling.

---

## 9. Preprocessing

The dataset is released close to its raw annotated form so that downstream users can apply their own preprocessing pipeline appropriate to their model. For reference, the accompanying paper's modeling pipeline applied the following preprocessing prior to model input (not baked into the released files):

- Whitespace normalization
- Duplicate removal (for the separate large unlabeled pretraining corpus used in domain-adaptive MLM pretraining — a distinct resource from this labeled dataset)
- URL filtering and light noise cleaning, while preserving the original linguistic structure of the text
- Tokenization to a maximum sequence length of 128 tokens (model-specific; not applied to the released raw text)

All personally identifiable information was removed/anonymized during preprocessing prior to public release (see [Section 12](#12-ethical-considerations-and-privacy)).

---

## 10. Recommended Data Splits

The dataset is released as a single unsplit file so that users can define their own splitting strategy. For reference and reproducibility, the original paper used the following split:

| Split | Percentage | Approx. Instances |
|---|---|---|
| Train | 70% | ~5,880 |
| Validation | 15% | ~1,260 |
| Test | 15% | ~1,260 |

The split was **stratified to preserve the 50/50 class balance** in each partition, with a fixed random seed (**42**) used for reproducibility in the original experiments. We recommend users adopt the same 70/15/15 stratified split and seed if they wish to directly compare results against the published BH-BERT benchmarks.

---

## 11. Example Instances

The table below shows representative examples (with English translations for accessibility) and illustrates the range of humor styles present in the dataset — wordplay, situational irony, and conversational punchlines — alongside comparably-styled non-humorous text.

| ID | Bengali Text | English Translation | Label |
|---|---|---|---|
| 1001 | ছাগল টাইপ ছেলে, এদের সঙ্গে প্রেমে পড়লে জীবন হবে ঘাসময় | A goat-type guy — falling in love with them turns life into nothing but grass (trouble). | 1 (Humor) |
| 1740 | যখন পরিবর্তন আসে, তখন একদল মানুষ পোশাক পরিবর্তন করে, আর অন্যদল পরিবর্তন করে চেহারা | When change comes, some people change their clothes, while others change their faces. | 0 (Non-Humor) |
| 979 | আগের মানুষ কত সহজ-সরল ছিলো, মোবাইল আসায় সব শেষ | People in the past were so simple; everything changed once mobile phones arrived. | 0 (Non-Humor) |
| 4873 | যারা যারা ধৈর্য্যের ফল খেয়েছেন, তারা প্লিজ রিভিউ দিয়ে যান, কেমন খেতে | Those who've tasted the fruit of patience — please leave a review on how it tastes. | 1 (Humor) |

*(Translations are provided for reader accessibility; the released dataset itself contains only the original Bengali text and label — no translation column.)*

---

## 12. Ethical Considerations and Privacy

- **Anonymization:** All collected data was anonymized during preprocessing prior to public release. Usernames, handles, and other personally identifiable information tied to the original social media posts were removed.
- **Public availability of source content:** Instances were drawn from publicly accessible posts, comments, and captions on the listed platforms.
- **Subjectivity of humor:** Because humor judgments are inherently subjective and culturally situated, the labels reflect the interpretation of the three annotators (with majority-vote resolution for ambiguous cases) rather than an objective ground truth. Users building on this dataset should be mindful that annotation reflects a specific cultural and linguistic community's sense of humor and may not generalize to all Bengali-speaking populations or dialectal variation.
- **Intended use:** This dataset is intended for academic and research use in humor detection, computational pragmatics, and related Bengali/low-resource NLP research. It is not intended to be used as a sole basis for automated content moderation decisions without human oversight, given the inherent ambiguity of humor annotation.

---

## 13. Known Limitations

Consistent with the limitations discussed in the accompanying paper:

1. **Binary granularity only.** The dataset distinguishes humor vs. non-humor but does not differentiate between humor sub-types such as sarcasm, satire, or irony.
2. **Annotation subjectivity.** Despite high inter-annotator agreement (Fleiss' Kappa = 0.82), humor interpretation remains implicitly subjective and culturally dependent, and some residual annotation ambiguity should be expected at the margins.
3. **Text-only modality.** The dataset captures only textual humor and does not incorporate the multimodal information (images, audio, video) that frequently accompanies humor in memes and social media content more broadly.
4. **Minor duplication.** A small number of duplicate/near-duplicate short instances remain (see [Section 8](#8-data-quality-and-validation)).

---

## 14. Baseline Results

For context, the table below summarizes benchmark results reported in the accompanying paper using this dataset (70/15/15 stratified split, seed 42). These are **not** part of the dataset file itself but are provided here so downstream users can situate their own results.

| Model | Accuracy | F1-score |
|---|---|---|
| TF-IDF + Logistic Regression | 0.7046 | 0.7144 |
| TF-IDF + Naïve Bayes | 0.6991 | 0.7125 |
| TF-IDF + Linear SVM | 0.6991 | 0.7094 |
| TF-IDF + Random Forest | 0.6681 | 0.7283 |
| BanglaBERT (baseline, no domain adaptation) | 0.8529 | 0.8556 |
| BanglaBERT + Humor-Adaptive MLM | 0.8664 | 0.8721 |
| **BH-BERT (full: MLM + threshold calibration)** | **0.8717** | **0.8805** |

BH-BERT additionally achieves an ROC-AUC of 0.9207 and a precision of 0.9028, with an optimal calibrated decision threshold of 0.33–0.35 (rather than the default 0.5).

---

## 15. How to Load the Dataset

```python
import pandas as pd

df = pd.read_excel("data/bh_humor_bn.xlsx", sheet_name="BH-BERT")
# Columns: id (int), text (str, Bengali), label (int, 0/1)

print(df.shape)          # (8400, 3)
print(df["label"].value_counts())  # 4200 / 4200
```

If a CSV mirror is provided instead:

```python
import pandas as pd

df = pd.read_csv("data/bh_humor_bn.csv", encoding="utf-8")
```

> Always read the file with UTF-8 encoding to correctly preserve Bengali script.

---

## 16. License

*MIT License*

---

## 17. Citation

If you use this dataset, please cite the accompanying paper:

```bibtex
@inproceedings{banglahumorbert,
  title     = {BanglaHumorBERT (BH-BERT): A Domain-Adaptive Transformer Approach to
               Context-Aware Humor Detection in Bengali Language Processing},
  author    = {MD. ABDUL KAIUM SHOHUG, Asif Khan, Munna Dhar, Aditi Tasmin},
  booktitle = {Proceedings of <Conference Name>},
  publisher = {Springer},
  year      = {2026}
}
```

> Replace the placeholder author names, conference name, and year with the final camera-ready publication details before publishing this file.

---

## 18. Changelog

| Version | Date | Notes |
|---|---|---|
| 1.0 | <release date> | Initial public release: 8,400 labeled Bengali instances (4,200 Humor / 4,200 Non-Humor) |

---

## 19. Contact

For questions, corrections, or collaboration inquiries regarding this dataset, please open an issue on this repository or contact the maintainer via [GitHub](https://github.com/asif-khan-ak).


