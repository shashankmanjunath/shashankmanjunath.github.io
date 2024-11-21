---
title: "Sleep Staging from Airflow Signals Using Fourier Approximations of Persistence Curves"
collection: publications
category: preprints
permalink: /preprint/2024-11-23-sleep-staging-airflow-tda
date: 2024-11-12
authors: Shashank Manjunath, Hau-Tieng Wu, Aarti Sathyanarayana
venue: "arXiv Preprint"
paperurl: "https://arxiv.org/abs/2411.07964"
---

_Abstract:_ Sleep staging is a challenging task, typically manually performed by
sleep technologists based on electroencephalogram and other biosignals of
patients taken during overnight sleep studies. Recent work aims to leverage
automated algorithms to perform sleep staging not based on electroencephalogram
signals, but rather based on the airflow signals of subjects. Prior work uses
ideas from topological data analysis (TDA), specifically Hermite function
expansions of persistence curves (HEPC) to featurize airflow signals. However,
finite order HEPC captures only partial information. In this work, we propose
Fourier approximations of persistence curves (FAPC), and use this technique to
perform sleep staging based on airflow signals. We analyze performance using an
XGBoost model on 1155 pediatric sleep studies taken from the Nationwide
Children's Hospital Sleep DataBank (NCHSDB), and find that FAPC methods provide
complimentary information to HEPC methods alone, leading to a 4.9% increase in
performance over baseline methods.

[Download paper here](https://arxiv.org/abs/2008.05381). Code is available at [https://github.com/shashankmanjunath/tda_sleep_staging](https://github.com/shashankmanjunath/tda_sleep_staging)
