---
title: "Automated Quantification of Periodic Discharges in Human Electroencephalogram"
collection: publications
category: manuscripts
permalink: /publication/2024-09-20-auotmated-quantification-periodic-discharges-human-eeg
date: 2024-09-20
authors: C. M. McGraw, S. Rao, S. Manjunath, J. Jing, and M. B. Westover
venue: "Biomedical Physics and Engineering Express"
paperurl: "https://iopscience.iop.org/article/10.1088/2057-1976/ad6c53/meta"
---

_Abstract:_ Periodic discharges (PDs) are pathologic patterns of epileptiform
discharges repeating at regular intervals, commonly detected in the human
electroencephalogram (EEG) signals in patients who are critically ill. The
frequency and spatial extent of PDs are associated with the tendency of PDs to
cause brain injury, existing automated algorithms do not quantify the frequency
and spatial extent of PDs. The present study presents an algorithm for
quantifying frequency and spatial extent of PDs. The algorithm quantifies the
evolution of these parameters within a short (10–14 second) window, with a focus
on lateralized and generalized periodic discharges. We test our algorithm on 300
'easy', 300 'medium', and 240 'hard' examples (840 total epochs) of periodic
discharges as quantified by interrater consensus from human experts when
analyzing the given EEG epochs. We observe 95.0% agreement with a 95% confidence
interval (CI) of [94.9%, 95.1%] between algorithm outputs with reviewer clincal
judgement for easy examples, 92.0% agreement (95% CI [91.9%, 92.2%]) for medium
examples, and 90.4% agreement (95% CI [90.3%, 90.6%]) for hard examples. The
algorithm is also computationally efficient and is able to run in 0.385 ± 0.038
seconds for a single epoch using our provided implementation of the algorithm.
The results demonstrate the algorithm's effectiveness in quantifying these
discharges and provide a standardized and efficient approach for PD
quantification as compared to existing manual approaches.

Code and data for this publication can be found at
[https://github.com/bdsp-core/IIIC-Frequency-Analysis](https://github.com/bdsp-core/IIIC-Frequency-Analysis).
