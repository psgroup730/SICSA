<p align="center">
  <img src="https://img.shields.io/badge/Python-3.8%2B-blue.svg" />
  <img src="https://img.shields.io/badge/PyTorch-2.0+-ee4c2c.svg" />
  <img src="https://img.shields.io/badge/License-MIT-green.svg" />
  <img src="https://img.shields.io/badge/API-Flask-lightgrey.svg" />
  <img src="https://img.shields.io/badge/Deployment-Ngrok-blueviolet.svg" />
  <img src="https://img.shields.io/badge/Dataset-PhysioNet-orange.svg" />
</p>
<svg width="900" height="520" xmlns="http://www.w3.org/2000/svg">
  <style>
    .box { fill:#f9f9f9; stroke:#333; stroke-width:1.5; }
    .arrow { stroke:#333; stroke-width:1.5; marker-end:url(#arrow); }
    .title { font-size:16px; font-weight:bold; font-family:Arial; }
    .text { font-size:13px; font-family:Arial; }
  </style>

  <defs>
    <marker id="arrow" markerWidth="10" markerHeight="10" refX="6" refY="3"
            orient="auto" markerUnits="strokeWidth">
      <path d="M0,0 L0,6 L9,3 z" fill="#333"/>
    </marker>
  </defs>

  <text x="260" y="30" class="title">
    Sleep Apnea Detection from Single-Lead ECG
  </text>

  <!-- Input -->
  <rect x="50" y="70" width="180" height="60" class="box"/>
  <text x="65" y="105" class="text">Single-Lead ECG</text>

  <!-- Preprocessing -->
  <rect x="300" y="70" width="220" height="60" class="box"/>
  <text x="320" y="95" class="text">Pre-Processing</text>
  <text x="320" y="115" class="text">Normalization + Augmentation</text>

  <!-- Lead Projection -->
  <rect x="600" y="70" width="230" height="60" class="box"/>
  <text x="620" y="105" class="text">Lead Projection</text>
  <text x="620" y="125" class="text">1-Lead → 12-Lead</text>

  <!-- Backbone -->
  <rect x="300" y="190" width="230" height="70" class="box"/>
  <text x="330" y="225" class="text">ESI Backbone</text>
  <text x="330" y="245" class="text">ConvNeXt_base</text>

  <!-- Temporal Head -->
  <rect x="300" y="310" width="230" height="80" class="box"/>
  <text x="320" y="345" class="text">BiLSTM + Attention</text>
  <text x="320" y="365" class="text">Multi-Scale Pooling</text>

  <!-- Output -->
  <rect x="300" y="430" width="230" height="60" class="box"/>
  <text x="330" y="465" class="text">Binary Prediction</text>

  <!-- Arrows -->
  <line x1="230" y1="100" x2="300" y2="100" class="arrow"/>
  <line x1="520" y1="100" x2="600" y2="100" class="arrow"/>
  <line x1="715" y1="130" x2="415" y2="190" class="arrow"/>
  <line x1="415" y1="260" x2="415" y2="310" class="arrow"/>
  <line x1="415" y1="390" x2="415" y2="430" class="arrow"/>
</svg>
## Abstract

Sleep apnea is a prevalent sleep disorder associated with severe cardiovascular
and neurological complications if left undiagnosed. Conventional diagnostic
methods such as polysomnography are costly, intrusive, and unsuitable for
long-term or continuous monitoring. This work presents an automated sleep apnea
detection framework using single-lead electrocardiogram (ECG) signals, enabling
deployment in wearable and resource-constrained environments.

The proposed approach leverages transfer learning from the Electrocardiogram
Signal Intelligence (ESI) foundation model, pre-trained on large-scale 12-lead
ECG data. A learnable lead projection module maps single-lead ECG signals to a
synthetic 12-lead representation, allowing effective reuse of the pre-trained
backbone. Temporal dependencies are modeled using a bidirectional long
short-term memory (BiLSTM) network with an attention mechanism, followed by
multi-scale pooling and a lightweight classifier.

Experimental evaluation on the PhysioNet Apnea-ECG database demonstrates strong
performance, achieving an ROC-AUC of 0.921 and an accuracy of 87.3%. The proposed
system is deployed as a real-time RESTful API, highlighting its practicality
for clinical screening and continuous home-based sleep monitoring.


## ✨ Key Features

- **Single-Lead ECG Input**  
  Designed for wearable and portable devices with minimal hardware requirements.

- **Transfer Learning with ESI Foundation Model**  
  Utilizes a ConvNeXt-based ECG foundation model pre-trained on large-scale
  12-lead ECG data to improve generalization and data efficiency.

- **Learnable Lead Projection**  
  Projects single-lead ECG signals into a synthetic 12-lead representation,
  enabling effective reuse of multi-lead pre-trained models.

- **Temporal Modeling with Attention**  
  Captures long-range temporal dependencies using a bidirectional LSTM with
  attention to highlight apnea-relevant ECG segments.

- **Robust Preprocessing & Augmentation**  
  Includes outlier filtering, normalization, noise injection, amplitude scaling,
  and baseline wander simulation.

- **Production-Ready Deployment**  
  Real-time inference via a Flask-based REST API with Ngrok support.

- **Interpretable Outputs**  
  Provides confidence scores and risk levels alongside binary predictions.

## 📊 Dataset

This project uses the **PhysioNet Apnea-ECG Database**, a widely adopted benchmark
dataset for sleep apnea research.

- **Number of Subjects:** 70
- **Sampling Frequency:** 100 Hz
- **Recording Duration:** ~8 hours per subject
- **Labels:** Per-minute annotations (A = Apnea, N = Normal)

### Data Split
- Training: 70%
- Validation: 10%
- Testing: 20%

Stratified splitting is used to maintain balanced class distribution across all
sets.
