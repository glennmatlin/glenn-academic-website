---
title: SumGNN - Drug Interaction Prediction
summary: Explainable predictions of negative medication interactions with deep graph learning
tags:
  - ML
  - NLP
  - Healthcare
date: '2022-10-13T00:00:00Z'

# Optional external URL for project (replaces project detail page).
external_link: ''

image:
  caption: SumGNN Architecture
  focal_point: Smart

links:
  - icon: github
    icon_pack: fab
    name: Code
    url: https://github.com/glennmatlin/SumGNN
url_code: ''
url_pdf: 'https://academic.oup.com/bioinformatics/article/37/18/2988/6174861'
url_slides: ''
url_video: ''

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---

## SumGNN: Explainable Predictions of Negative Medication Interactions

The SumGNN project addresses the critical challenge of predicting drug-drug interactions (DDIs) in healthcare settings where data may be limited or incomplete. This collaboration between Georgia Tech, IQVIA, and Stanford University resulted in a novel graph neural network approach that outperforms existing methods, especially in low-data scenarios.

### The Challenge

Medical patients trust physicians to make the correct clinical decisions when caring for them. Physicians often must make significant decisions with incomplete information while selecting medications from a wide array of options and analyzing potential interactions. Negative drug-drug interactions (DDI) can result in less effective treatment or potentially harm patients.

The risk for patients increases dramatically as they take more medications:
- 36% of elderly patients in the US use 5+ drugs
- 15% of these patients are at high risk of significant negative DDIs

### Our Solution

SumGNN incorporates biomedical knowledge graphs (KGs) into the prediction process through three key components:

1. **Subgraph Extraction**: Anchors work on the most relevant subgraph in the biomedical KG
2. **Summarization Module**: Uses self-attention mechanism to provide physicians with reasons behind predictions
3. **Multi-Channel Integration**: Combines external biomedical KG data with DDI datasets for accurate predictions

### Research Impact

SumGNN outperformed state-of-the-art baselines by up to 5.54% in F1 score, with the most significant improvements seen in low-data scenarios. The model was evaluated on two major DDI databases:

- **DrugBank**: 1,709 drugs and 136,351 drug pairs with 86 interaction types
- **TWO-SIDES**: 645 drugs and 46,221 drug pairs with 200 different side effect types

For the biomedical knowledge integration, we used **HetioNet**, a comprehensive KG that combines over 50 years of biomedical information from 29 public databases into a single resource with 47,031 nodes and over 2.2 million relationships.

### Significance

The performance gains of SumGNN are particularly notable when training data is limited, making it especially valuable for rare drug combinations or newly introduced medications. This approach demonstrates how graph neural networks can be effectively applied to critical healthcare problems, potentially reducing adverse drug events and improving patient outcomes.
