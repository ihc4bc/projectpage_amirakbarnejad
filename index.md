---
layout: default
---

# Sec 1. Proposed Method 1, GPEX
Gaussian process (GP) is a white-box model which has the potential to become globally faithful to a neural network, 
thereby unboxing the blackbox of deep learning [8]. 
Existing theoretical works put strict assumptions on a neural network to make it equivalent to a Gaussian process.
Accommodating those theoretical assumptions is hard in recent deep architectures, and those theoretical conditions need refinement as new deep architectures emerge.
We proposed a method that - given a neural network - adopts knowledge distillation to finds a GP which is equivalent to the neural network.
Since GP is a white-box model, the obtained GPs can provide insights about the underlying decision mechanisms of the given neural network. 
Our method called GPEX showed promissing results and was published in NeurIPS 2023 conference [2].

# Sec 2. The Initial Prediction Problem
OncotypeDX test is a genomic assay for specific cohorts of breast cancer patients. 
The output of the assay is a number between 0 and 100 for each patient, and in this article we refer to it as
recurrence score.
OncotypeDX recurrence score stratifies patients as follows:
- Recurrence-score between 0 and 18: low risk of distant recurrence in 10 years.
- Recurrence-score between 18 and 30: medium risk of distant recurrence in 10 years.
- Recurrence-score between 30 and 100: high risk of distant recurrence in 10 years.

The initial goal was to adopt machine learning to predict recurrence-score only from H&E-stained 
histopathology images, thereby bypassing the need for performing genomic assays. 
A dataset of roughly 600 breast cancer patients was collected.
The dataset contained the following information for each patient:
- One H&E-stained whole-slide image
- The result of OncotypeDX test for the patient (a number between 0 and 100)
- Estrogen/progesterone receptor (ER/PR) intensity and percentages, obtained by pathologist assessment (four ordinal integers for each patient). 

# Sec 3. Proposed Method 2
We evaluated the state-of-the-art method CLAM [6] as well as the well-known cell profiler [7] features in predicting recurrence score directly from H&E-stained breast cancer images.    
This was done mainly by N. Guruprasad and with participation of A. Akbarnejad.
These methods could achieve prediction performances of up to 82 in terms of AUC.


The state-of-the-art method CLAM [6] encodes a whole-slide image by feeding each patch to a **fixed** resnet backbone which is pretrained on imagenet.
One may ask: is the low prediction performance of CLAM related to using a fixed backbone and not training the backbone itself?


To answer this question we proposed a pipeline based on deep Fisher-vector encoding which is entirely trained in an end-to-end way.
The method was published in the ISBI conference in 2021 [4]. 
All in all the proposed pipeline [4] was slightly better than CLAM [6] in predicting recurrence-score, but still the prediction performance did not surpass 82 in terms of AUC.

When training the end-to-end pipeline, the conventional PyTorch dataloader is prohibitively slow.
Therefore, to address this issue we developed a python tool called PyDmed and made it publicly available [9].
The proposed dataloader streams patches from the whole-slide images to GPU(s) in an efficient way, so the GPU(s) do not remain idle while new pathces are being extracted from WSIs.
So far (checked on Jan 19th, 2024) PyDmed's repository on github has 18 stars.   


# Sec 4. The Proposed IHC4BC Dataset
Oncotype DX test obtains a number for many different genes/proteins (like Ki67, ER, PR, and Her2).
Afterwards, these numbers from different genes are normalized and averaged by different weights to produce the final recurrence score.
One may think: given a whole-slide image, predicting the measurement for each gene could be a simpler problem than predicting the recurrence score itself.
Because recurrence-score is a combination of measurements for different genes.

In this regards, our findings are as follows:
1. Weakly-supervised learning (i.e. learning with slide-level labels) is not effective in predicting Ki67, ER, PR, and Her2 status from slide images.
This fact is also observed in the study of Laleh et al [10].
2. Now that weak supervision is not effective, for this prediction task no large datasets containing strong (i.e. patch-level or pixel-level) labels are available [11].   
3. For this prediction task, the performance of previous methods could not surpass 80-85 in terms of AUC, probably due to the unavailablity of datasets [11]. 

Given the lack of a large dataset for this prediction task (finding 2 mentioned above), we collected a large dataset that provides strong labels (i.e. patch-level labels).
Our dataset contains roughly 180K images (90K pairs) each of which are 3K by 3K.
Collecting the dataset took roughly 8 months, and we put a lot of effort to obtain reliable labels.  
Using our large dataset and with proper labeling protocl, we were finally able to train methods whose prediction performance is around 90 in terms of AUC (point 3 mentioned above).
We published the dataset and the analysis in a paper [1] which is currently under review.
We made the dataset publicly available online [12].  

# References
[1] A. Akbarnejad and N. Ray and P. J Barnes and G. Bigras,
 “Predicting Ki67, ER, PR, and HER2 Statuses from H&E-stained Breast Cancer Images,” arxiv preprint: https://arxiv.org/abs/2308.01982.


[2] A. Akbarnejad and G. Bigras and N. Ray, “GPEX, A Framework For Interpreting Artificial Neural Networks,” Conference on Neural Information Processing Systems (NeurIPS) 2023.


[3] Y. Yang and A. Akbarnejad and N. Ray and G. Bigras, “Double adversarial domain adaptation for whole-slide-imageclassification,” Medical Imaging with Deep Learning (MIDL), 2021.


[4] A. Akbarnejad and N. Ray and G. Bigras, “Deep Fisher Vector Coding For Whole Slide Image Classification,” in International Symposium of Biomedical Imaging (ISBI), 2021.


[5] N. Guruprasad, A. Akbarnejad, G. Bigras, P. J. Barnes, and N. Ray, "A Closer Look at Weak Supervision's Limitations in WSI Recurrence Score Prediction", IEEE International Conference on Bioinformatics and Biomedicine
(BIBM), 2023.


[6] M. Y, Lu, D. F. Williamson, et al, "Data-efficient and weakly supervised computational pathology on
whole-slide images", Nature biomedical engineering, 5(6):555–570, 2021.


[7] A. E. Carpenter, T. R. Jones, et al, "CellProfiler: image analysis software for identifying and quantifying cell phenotypes", Genome Biol 7, R100 (2006). 


[8] https://blog.research.google/2020/03/fast-and-easy-infinitely-wide-networks.html


[9] https://github.com/amirakbarnejad/PyDmed/


[10] N. G. Laleh, H. S. Muti, et al, "Benchmarking weakly-supervised deep learning pipelines for whole slide classification in computational pathology", Med Image Anal. 2022 Jul;79:102474.


[11] D. Cifci, S. Foersch, and J. N. Kather, "Artificial intelligence to identify genetic alterations in conventional histopathology", The Journal of Pathology, 257(4):430–444, 2022.


[12] https://ihc4bc.github.io/


