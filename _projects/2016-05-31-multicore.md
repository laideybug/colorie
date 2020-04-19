---
layout: project
title: Distributed Machine Learning
description: Exploring microarray gene expression biclustering on the Parallella III Microserver
summary: Exploring microarray gene expression biclustering on the Parallella III Microserver
category: Embedded Systems, Machine Learning
---

{% include picture.html img="parallella" ext="jpeg" alt="parallella" %}

## Introduction

Traditional machine learning algorithms are highly centralised, with a large number of processing agents, distributed across parallel processing resources, accessing a single, very large data object. This creates bottlenecks as a result of limited memory access rates. Distributed learning has the potential to resolve this problem by employing networks of co-operating agents each operating on subsets of the data, but as yet their suitability for realisation on parallel architectures such as multicore are unknown. To explore this, I implemented the deployment of a [distributed dictionary learning algorithm](https://ieeexplore.ieee.org/document/6994844)[1] for microarray gene expression bi-clustering on a 16-core Parallella III Microserver.

My work was subsequently published at the International Conference on Acoustics, Speech, and Signal Processing in 2017 and can be read [here]({{site.files.multicore}}).

## Dataset

In typical centralised dictionary learning algorithms, each *agent* in a network maintains a copy of the entire dictionary. In this approach, however, each utilised core of the Parallella Epiphany processor (eCore) is only in charge of a single dictionary column. The networked eCores work co-operatively to learn the entire dictionary in a distributed fashion. The input dataset for this experiment consists of 12,625 gene expression levels taken from a sample of 56 subjects. The subjects are classified into four lung cancer categories:

* **Normal**: Subjects without cancer
* **Carcinoid**: Patients with pulmonary carcinoid tumours
* **Colon**: Patients with colon metastases
* **SmallCell**: Patients with small-cell carcinoma

The data is presented as a 56x12,625 matrix. The rows represent the patients grouped together by their classification, while the columns represent their corresponding gene expression levels.

This experiment is an unsupervised machine learning task in that the eCores are unaware of the ground truth of the classification of each patient. The ultimate aim, however, is for the eCores to group patients with similar genetic information into *clusters*. Once the process is completed, different coloured markers are applied to each subject for identification and evaluation on the plotted graph. The algorithm is a decentralised *online* algorithm in that the eCores are exposed to each column of the cancer data matrix once in a streaming manner.

## Results

This approach demonstrates that distributed learning approaches can enable near-linear speed-up with the number of processing resources and, via the use of DMA-based communication, a 50% increase in throughput can be enabled. The implementation was written in C for the Epiphany E16G301 architecture and can be found on [github](https://github.com/laideybug/multicore).

---

[1] J. Chen, Z. J. Towfic and A. H. Sayed, "Dictionary Learning Over Distributed Models," in IEEE Transactions on Signal Processing, vol. 63, no. 4, pp. 1001-1016, Feb.15, 2015.