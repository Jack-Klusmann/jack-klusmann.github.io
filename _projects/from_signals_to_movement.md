---
title: "From Signals to Movement"
short: "Surface-based EMG signal classification for enhanced hand prosthesis control."
tags: ["Wearable Robotics", "Deep Learning", "Prostheses"]
hero: "/assets/prosthesis.jpg"
thumb: "/assets/prosthesis.jpg"
weight: 2023
---

## Motivation
In recent years, exciting progress has been made in the development of human prosthetic devices, driven by significant advancements in **robotics**, **biomedical engineering**, and **machine learning**. Modern prostheses are becoming increasingly capable of enabling people with **missing or impaired limbs** to perform complex daily tasks that would otherwise be difficult to accomplish.

An effective prosthesis must combine **accuracy**, **reliability**, and **intuitive control** to function as a natural extension of the user’s body, seamlessly integrating with their own movements. Recent research has focused on achieving this through the interpretation of signals captured by various types of **sensors**, allowing the user to control the prosthesis implicitly rather than manually selecting specific actions. One example is the use of **surface-based EMG electrodes**, which measure the electrical activity of muscles and enable the system to infer the user’s intended movements. However, many existing solutions still rely on simple threshold-based EMG control or basic linear classifiers that struggle to handle the variability of muscle signals over time.

Interest has grown in leveraging **machine learning** to develop more advanced methods for interpreting these signals and enabling **multi-grasp prosthesis systems** that can perform a wider range of actions. However, achieving stable and reliable control in such systems remains an open challenge. The position of EMG electrodes can shift slightly each time the prosthesis is worn, and factors such as sweat, temperature, and muscle fatigue can significantly affect signal quality. These variations make real-time signal interpretation a complex and technically demanding problem.

I had the opportunity to contribute to the development of a prosthetic hand for the **ETH Zurich Cybathlon Arm Prosthesis Race**. As part of the perception group of my university’s team, my primary responsibility was to design a **robust deep-learning-based classification pipeline** capable of translating surface EMG signals into discrete commands in real time and converting them into functional hand poses. The system was designed to run directly on the prosthesis hardware, ensuring smooth and natural grasp transitions. To evaluate the complete system, we had the chance to test the prosthesis together with an amputee. This experience allowed us to better understand and design a control system that feels intuitive, comfortable, and reliable for the user.


<!--
## Approach
To achieve reliable control, I designed a complete EMG signal processing and classification pipeline combining **signal preprocessing**, **feature extraction**, **dimensionality reduction**, and **neural network-based classification**.

### Data Collection
Surface EMG signals were recorded from **four Ottobock MyoBock electrodes** placed on the pilot’s residual limb. Each channel captured the electrical activity of a different muscle group responsible for hand and wrist motions.  

Signals were acquired at high frequency and segmented into overlapping time windows of **800 ms** with a **400 ms overlap**, allowing both temporal smoothness and responsiveness.

### Feature Extraction
Each window was transformed into a compact feature vector describing the shape and dynamics of the EMG signal. Extracted features included:

- Temporal moment  
- Slope sign change  
- Zero crossings  
- Root mean square (RMS)  
- Waveform length  
- Maximum fractal length  
- Average amplitude change  
- Enhanced mean absolute value  

These features are commonly used in EMG literature and help capture both time-domain and complexity characteristics of muscle activity.

### Dimensionality Reduction
To reduce computational load and improve generalization, I applied **Linear Discriminant Analysis (LDA)**, projecting the feature space to 12 dimensions before classification.  

### Classification Model
I implemented a **feedforward neural network (FFNN)** with one hidden layer of 12 units and **leaky ReLU** activations. The network was trained using **mini-batch gradient descent** and the **Adam optimizer**, with **batch normalization** applied for faster convergence and better stability.  
Training was performed in MATLAB using custom scripts (`MLP_Trainer.m`, `MLP_classifier.m`).

### Baseline Models
For comparison, I trained two classical machine learning baselines:
- **k-Nearest Neighbors (kNN)** classifier  
- **Logistic Regression Classifier (LRC)**  

All models were evaluated on the same dataset derived from windowed EMG samples.

### Post-Processing
To ensure smooth and confident predictions in real time, I implemented a **probability-based refinement** of the classifier output:
- If the first three labels agreed → assign that pose.  
- If the second and third agreed and their combined probability > 75% → assign.  
- If the third label alone exceeded 90% → assign.  
- If the first two labels agreed with combined probability > 80% → assign.  
- Otherwise, default to rest.  

This approach effectively suppressed noisy transitions and ensured stable control for the user.

---

## Findings
The neural network achieved an **accuracy of around 90%**, outperforming both kNN and logistic regression baselines.  

In practice, the combination of robust feature extraction and post-processing yielded smooth transitions between rest, open, and close states — essential for real-time control of the prosthesis. The resulting predictions were used to trigger a **state machine** that mapped these three motions to multiple hand grasps (e.g., power, pinch, lateral, and precision grips).

During real-world testing with an **amputee pilot**, the classifier successfully operated **in real time** on the embedded system, maintaining consistent performance and enabling intuitive grasp switching during task execution.

---

## Application
The trained EMG classifier was deployed on a **custom embedded board** integrated into the Cybathlon prosthesis setup.  
In operation, it provided real-time intent recognition for controlling grasp transitions, as shown in the grasping scheme below:

<div class="grid media-grid">
  <figure>
    <img src="/assets/emg/grasp_scheme.png" alt="Grasping Scheme">
    <figcaption>State machine mapping EMG-based rest, open, and close signals to functional grasps such as power, pinch, and precision sphere.</figcaption>
  </figure>
</div>

This work demonstrates how lightweight deep learning models can run efficiently on embedded systems, providing reliable human–machine interfaces for prosthetic control.

---

## Media
<div class="grid media-grid">
  <figure>
    <img src="/assets/emg/emg_pipeline.png" alt="EMG Classification Pipeline">
    <figcaption>Signal processing and classification pipeline: from raw EMG acquisition to feature extraction, dimensionality reduction, neural classification, and post-processing.</figcaption>
  </figure>

  <figure>
    <img src="/assets/emg/ffnn_training.png" alt="Training Curve">
    <figcaption>Feedforward neural network training curves showing convergence and final accuracy of ~90%.</figcaption>
  </figure>

  <figure>
    <img src="/assets/emg/testing.png" alt="Real-Time Testing">
    <figcaption>Real-time control tests with the amputee pilot using the embedded EMG classification board.</figcaption>
  </figure>
</div>

---

-->