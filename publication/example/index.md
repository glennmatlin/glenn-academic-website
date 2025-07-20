---
title: "UnfoldML: Cost-Aware and Uncertainty-Based Dynamic 2D Prediction for Multi-Stage Classification"
author: 
  - "Yanbo Xu"
  - "Alind Khare"
  - "Glenn Matlin"
  - "Monish Ramadoss"
  - "Chao Zhang"
  - "Alexey Tumanov"
date: "2022-10-19"
categories: [Machine Learning, Systems, NeurIPS]
description: "A cost-aware and uncertainty-based framework for dynamic 2D prediction in multi-stage classification systems."
image: "featured.png"
---

## Publication Details

**Venue:** Neural Information Processing Systems (NeurIPS) 2022

**PDF:** [Download Paper](unfoldml.pdf)

## Abstract

Machine Learning (ML) research has focused on maximizing the accuracy of predictive tasks. ML models, however, are increasingly more complex, resource intensive, and costlier to deploy in resource-constrained environments. These issues are exacerbated for prediction tasks with sequential classification on progressively transitioned stages with "happens-before" relation between them.

We argue that it is possible to "unfold" a monolithic single multi-class classifier, typically trained for all stages using all data, into a series of single-stage classifiers. Each single-stage classifier can be cascaded gradually from cheaper to more expensive binary classifiers that are trained using only the necessary data modalities or features required for that stage. 

UnfoldML is a cost-aware and uncertainty-based dynamic 2D prediction pipeline for multi-stage classification that enables:

1. Navigation of the accuracy/cost tradeoff space
2. Reducing the spatio-temporal cost of inference by orders of magnitude
3. Early prediction on proceeding stages

UnfoldML achieves orders of magnitude better cost in clinical settings, while detecting multi-stage disease development in real time. It achieves within 0.1% accuracy from the highest-performing multi-class baseline, while saving close to 20X on spatio-temporal cost of inference and earlier (3.5hrs) disease onset prediction. We also show that UnfoldML generalizes to image classification, where it can predict different level of labels (from coarse to fine) given different level of abstractions of a image, saving close to 5X cost with as little as 0.4% accuracy reduction.
---

## Abstract

Machine Learning (ML) research has focused on maximizing the accuracy of predictive tasks. ML models, however, are increasingly more complex, resource intensive, and costlier to deploy in resource-constrained environments. These issues are exacerbated for prediction tasks with sequential classification on progressively transitioned stages with "happens-before" relation between them.

We propose UnfoldML, a cost-aware and uncertainty-based dynamic 2D prediction pipeline for multi-stage classification that:

1. Navigates the accuracy/cost tradeoff space
2. Reduces the spatio-temporal cost of inference by orders of magnitude
3. Enables early prediction on proceeding stages

In clinical settings, UnfoldML achieves within 0.1% accuracy of high-performing multi-class baselines while saving close to 20X on inference costs and enabling earlier (3.5hrs) disease onset prediction. We demonstrate that UnfoldML also generalizes to image classification tasks.
