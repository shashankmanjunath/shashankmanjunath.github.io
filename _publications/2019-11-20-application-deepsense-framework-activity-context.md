---
title: "Application of the DeepSense Deep Learning Framework to Determination of Activity Context from Smartphone Data"
collection: publications
category: conferences
permalink: /publication/2019-11-20-application-deepsense-framework-activity-context
date: 2019-11-20
authors: Bethany Bracken, Shashank Manjunath, Stan German, Camille Monnier, Mike Farry
venue: "Proceedings of the Human Factors and Ergonomics Society Annual Meeting"
paperurl: "https://doi.org/10.1177/1071181319631002"
---

_Abstract:_ Current methods of assessing health are infrequent, costly, and
require advanced medical equipment. 92% of US adults carry mobile phones, and
77% carry smartphones with advanced sensors (Smith, 2017). Smartphone apps are
already being used to identify disease (e.g., skin cancer), but these apps
require active participation by the user (e.g., uploading images). The goal of
this research is to develop algorithms that enable continuous and real-time
assessment of individuals by leveraging data that is passively and unobtrusively
captured by cellphone sensors. Our first step to accomplish this is to identify
the activity context in which the device is used as this affects the accuracy
and reliability of sensor data for measuring and inferring a user’s health; data
should be interpreted differently when the user is walking or running versus on
a plane or bus. To do this, we use DeepSense, a deep learning approach to
feature learning first developed by (Yao, Hu, Zhao, Zhang, & Abdelzaher, 2017).
Here we present six experiments validating our model on: (1) a baseline
implementation of DeepSense on the same data used by Yao et al., (2017)
achieving a balanced accuracy (BA) of 95% over the six main contexts; (2) its
ability to classify context using a different publically-available dataset (the
ExtraSensory dataset) using the same 70/30 train/test split used by Vaizman et
al. (2018), with a BA of 75%; (3) its ability to achieve improved classification
when training on a single user, with a BA of 78%; (4) its ability to achieve
accurate classification of a new user with a BA of 63%; (5) its improvement to
70% BA for new users when we considered phone placement to remove confounding
information, and (6) its ability to accurately classify contexts over all 51
contexts collected by Vaizman et al, achieving a BA of 80% on 9 contexts, 75% on
12, and 70% on 17. We are now working to improve these results by adding other
sensors available through smartphone data collection included in the
ExtraSensory dataset (e.g., microphone). This will allow us to more accurately
assess minor deviations in user behaviors that could indicate changes in health
or injury status by accurately accounting for irrelevant, inaccurate, or
misleading readings due to contextual effects that may confound interpretation.

[Download paper here](https://doi.org/10.1177/1071181319631002 )
