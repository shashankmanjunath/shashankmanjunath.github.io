---
layout: default
title: Projects
permalink: /projects/
---

# Boston University

## Machine Learning Techniques for Reconstruction and Segmentation of Nanoparticle Interferometric Signatures

This was my Master's thesis, performed under the supervision of M. Selim Ünlü at Boston University. A PDF of the thesis
is available upon request.

### Abstract

Single particle interferometric reflectance imaging sensor (SP-IRIS) allows for label-free biological nanoparticle
detection. This imaging technique allows collection of a 3D defocus profile of interferometric signatures of particles
on a reflecting surface. However, automated reconstruction of information from SP-IRIS data and identification of
particles remains a challenge. In this work, we develop machine learning techniques to perform accurate reconstruction
of defocus profiles from undersampled SP-IRIS data, and machine learning techniques to segment particles of interest
from background in SP-IRIS data. We test the performance of a direct, single-signal fully connected neural network based
reconstruction technique, an untrained non-convolutional network for single-signal reconstruction, and a 2D
convolutional neural network reconstruction model for reconstruction of SP-IRIS signal defocus profiles from
undersampled data. We then test a UNet based segmentation model to segment particle signals from background signals.
Lastly, we propose a novel combined reconstruction and segmentation model which can perform both reconstruction and
segmentation on undersampled SP-IRIS data.

# LumaSil

## Apparatus for Providing Low-Level Light Therapy

### Description
LumaSil was my senior design project at Vanderbilt University, which eventually evolved into a startup that functioned
until January 2019. LumaSil focused on mitigating diabetic foot ulcer infections by using a low-level light based
therapy that could be integrated into casts. I worked on the hardware and software side of the electronics portion of
this project, ensuring that the product achieved battery life goals as well as functioned correctly in various
environments. I also worked on proof-of-concept efficacy studies for the product. Although the startup is no longer
existent, as the product is not commercially viable, we were able to file a currently pending patent on our technology.

### Related Works
1. [Grisham, Candace, Manjunath, Shashank, Perlin, Benjamin, Russo, Anthony, Wigginton, Nicholas, Walker III, Matthew.
    "Low-Level Light Therapy for Improvement of Diabetic Foot Ulcer Infection Outcomes." Biomedical Engineering Society, October 2018.](/assets/lumasil_paper.pdf)
2. [Pending] Grisham, C., Manjunath, S., Perlin, B., Russo, A., Wigginton, N., Walker III, M. "Apparatus for Providing Low Level Light Therapy." Application No. 62/873166.

# Vanderbilt University Institute for Imaging Science

## Machine Learning Applications to Sciatic Nerve Analysis in MRI

### Description
I began working on this project during my sophomore year of college, attempting to segment the sciatic nerve from 3D
magnetic resonance image (MRI) volumes of the upper leg. After making several attempts to design a more traditional
computer vision system to perform this segmentation, including an attempt at a multi-atlas segmentation technique, we
decided to approach this project from a machine learning perspective and use a convolutional neural network to perform
the segmentation task. Since we had expertly-labeled data from radiologists at Vanderbilt University Medical Center, the
convolutional neural network based technique performed extremely well.

### Related Works
1. [Manjunath, Shashank, Dortch, Richard"Sciatic Nerve Segmentation in MR Images of the Upper Leg via Convolutional Neural Networks." Biomedical Engineering Society, October 2017.](/assets/sciatic_nerve_paper_bmes.pdf)
2. [Hancock, Matthew, Manjunath, Shashank, Dortch, Richard. "Sciatic Nerve Segmentation in MRI Volumes of the Upper Leg via 3D Convolutional Neural Networks." International Society of Magnetic Resonance in Medicine, June 2018.](/assets/sciatic_nerve_paper_ismrm.pdf)

## Free-Water Elimination in Diffusion Tensor Imaging

### Description
project during my junior year at Vanderbilt University. In particular, I worked on creating a novel signal model that
could account for free water penetrating the region of interest when acquiring diffusion tensor images. Diffusion tensor
imaging (DTI) works by tracking the movement of water molecules. Unfortunately, when free water penetrates the region of
interest, the signal of interest is corrupted by the random movement of the free water molecules. I attempted to develop
a technique that could separate the free water signal from the signal of interest in the DTI signal.

### Related Works
1. [Manjunath, Shashank, Manzanera-Esteve, Isaac, Thayer, Wes, Does, Mark, Dortch, Richard. "Free-Water Elimination Diffusion Tensor Imaging to Assess Nerve Recovery in Excised Rat Nerve." International Society of Magnetic Resonance in Medicine, June 2018.](/assets/dti_paper_ismrm.pdf)

# Other Works

## Charles River Analytics
These works were performed during my time at Charles River Analytics.
1. [Manjunath, Shashank, Bracken, Bethany K., German, Stan, Monnier, Camille, Farry, Mike. "User Activity Context
    Recognition From Smartphone Data Using Deep Neural Networks." Biomedical Engineering Society, October 2019.](/assets/smanjunath_bmes_2019.pdf)
2. [Manjunath, S., Thornton, W. "Deep Learning for Maritime Imagery." Submarine Technology Symposium, May 2019
   (Oral)](https://www.navalsubleague.org/events/submarine-technology-symposium/)
