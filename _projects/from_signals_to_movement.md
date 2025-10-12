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

An effective prosthesis must combine **accuracy**, **reliability**, and **intuitive control** to function as a natural extension of the user's body, seamlessly integrating with their own movements. Recent research has focused on achieving this through the interpretation of signals captured by various types of **sensors**, allowing the user to control the prosthesis implicitly rather than manually selecting specific actions. One example is the use of **surface-based EMG electrodes**, which measure the electrical activity of muscles and enable the system to infer the user's intended movements. However, many existing solutions still rely on simple threshold-based EMG control or basic linear classifiers that struggle to handle the variability of muscle signals over time.

Interest has grown in leveraging **machine learning** to develop more advanced methods for interpreting these signals and enabling **multi-grasp prosthesis systems** that can perform a wider range of actions. However, achieving stable and reliable control in such systems remains an open challenge. The position of EMG electrodes can shift slightly each time the prosthesis is worn, and factors such as sweat, temperature, and muscle fatigue can significantly affect signal quality. These variations make real-time signal interpretation a complex and technically demanding problem.

I had the opportunity to contribute to the development of a prosthetic hand for the **ETH Zurich Cybathlon Arm Prosthesis Race**, where competitors complete practical tasks such as carrying a crate of bottles, screwing in a light bulb, or zipping up a jacket using their prosthesis. As part of the perception group of my university's team, my primary responsibility was to design a **robust deep-learning-based classification pipeline** capable of translating surface EMG signals into discrete commands in real time and converting them into functional hand poses. The system was designed to run directly on the prosthesis hardware, ensuring smooth and natural grasp transitions. To evaluate the complete system, we had the chance to test the prosthesis together with an amputee. This experience allowed us to better understand and design a control system that feels intuitive, comfortable, and reliable for the user.

## Approach
To achieve robust prosthesis control, we designed a complete EMG signal processing and classification pipeline that combines **signal preprocessing**, **feature extraction**, **neural network-based classification**, and **post-processing**. In addition, we implemented a **state machine** to translate the classified signals into corresponding grasp types. The entire system was developed in **MATLAB** and **Simulink**.

### Signal Classification

**1. Raw Data Collection and Preprocessing**
Surface EMG signals are recorded from **four Ottobock MyoBock electrodes** placed on the user's residual arm. Each channel captures the electrical activity of a different muscle group responsible for hand and wrist motions, transmitting signals from the arm to the hand. The placement of the electrodes is determined individually for each user, as the muscle structure and available activation sites vary from person to person.

The signals are sampled at high frequency and segmented into overlapping time windows of **800 ms** with a **400 ms overlap**, ensuring both temporal continuity and responsiveness during real-time operation.

**2. Feature Extraction**
Each window is transformed into a compact feature vector describing the temporal and dynamic characteristics of the EMG signal. We selected the features based on established EMG signal analysis literature:

- Temporal moment
- Slope sign change
- Zero crossings
- Root mean square (RMS)
- Waveform length
- Maximum fractal length
- Average amplitude change
- Enhanced mean absolute value

To reduce computational load and improve generalization, we applied **Linear Discriminant Analysis (LDA)** to project the feature space into a lower-dimensional representation before classification.

**3. Classification Models**
We implemented and compared several classification approaches to evaluate their performance and suitability for our use case:

- **k-Nearest Neighbors (kNN):** Simple to implement but computationally expensive during inference.
- **Logistic Regression (LRC):** Fast and interpretable but limited by its linear decision boundaries.
- **Feedforward Neural Network (FFNN):** Capable of modeling nonlinear relationships but more sensitive to overfitting.
- **Convolutional Neural Network (CNN):** Provides robust spatial–temporal feature extraction while maintaining scalability.

The final model used was a **CNN** with **leaky ReLU** activations. The network was trained using **mini-batch gradient descent** and the **Adam optimizer**, with **batch normalization** to improve convergence and stability.

**4. Post-Processing**
To ensure smooth and stable predictions during real-time control, we implemented a **probability-based refinement** of the classifier output:

- If the first three labels agree, the pose is assigned.
- If the second and third labels agree and their combined probability exceeded 75%, the pose is assigned.
- If the third label's probability exceeds 90%, the pose is assigned.
- If the first two labels agree and their combined probability exceeds 80%, the pose is assigned.
- In all other cases, the pose defaults to the resting state.

This refinement step aims to reduce transient misclassifications and improve stability during continuous operation.

### State Machine
A key challenge in prosthesis control is that users typically have access to only a few distinct and repeatable muscle activations. The control strategy must therefore extract as much functionality as possible from a limited number of EMG inputs. To address this, we designed a **state machine** that maps muscle activations to specific control actions. It enables multiple grasp types and wrist motions using only three user inputs that mimic the muscle activations for **hand extension**, **hand flexion**, and **rest**.

The prosthesis supports several essential grasp types commonly used in daily activities, including:
- **Palmer pinch:** Bringing the thumb and index finger together to pick up small objects.
- **Lateral pinch:** Positioning the thumb against the side of the index finger to hold flat items.
- **Power grasp:** Closing all fingers into a fist to carry larger objects, such as a crate of bottles.
- **Precision sphere grasp:** Using the fingertips to manipulate small round objects, such as screwing in a light bulb.
- **Wrist rotation:** Allowing pronation and supination for improved positioning and dexterity.

The system recognizes three main EMG states: **Rest** (no activation), **Open** (hand extension), and **Close** (hand flexion), which serve as the primary control inputs. Depending on the current state and the duration or sequence of these activations, the prosthesis transitions between different grasping modes. This design allows the user to access multiple grasp types seamlessly without explicit manual selection.

The control logic was designed with a focus on **consistency**, **ease of use**, and **adaptability**:
- Short activations result in small, precise finger or wrist adjustments.
- Long activations trigger complete transitions between grasp types.
- All transitions are **non-blocking**, allowing the user to return to the resting position at any time.

Through this structure, the state machine enables **multi-grasp, multi-motion control** using only three intuitive EMG commands.

## Findings
Overall, the system demonstrated robust performance, although the challenges we encountered were quite different from what we had initially expected.

### Signal Classification
The main goal of this project was to design a model that was **compact enough for embedded deployment** while remaining **fast and accurate** for real-time control.

Interestingly, achieving high accuracy was not the main difficulty. The extracted features were already sufficiently discriminative and provided stable inputs for classification. Although natural signal variations caused by sweat, electrode displacement, and muscle fatigue introduced some noise, our **post-processing** mitigated these effects in most cases.

All four models we tested achieved classification accuracies above 90%. However, none initially met the real-time performance requirements, with **low-latency inference on limited hardware** emerging as the main challenge.

Among the models we tested, the **Convolutional Neural Network (CNN)** provided the best balance between performance and efficiency. It could be scaled down substantially while maintaining high accuracy and stable real-time behavior. The relatively small number of input classes also contributed to system robustness, as we focused on a limited set of reliably distinguishable muscle activation patterns that the amputee could comfortably reproduce.

### State Machine
The **state machine** played a crucial role in translating these limited input classes into a much broader range of functional grasps. Although the control scheme required some initial familiarization, the amputee quickly adapted and learned to switch between multiple grasp types using only a few intuitive muscle signals.

However, due to the large number of states within the state machine, reaching certain positions took longer than expected, causing slowdowns during tasks that required frequent transitions between different grasp types.

## Application
This project demonstrates how **machine learning and control logic** can be combined to create intuitive, real-time interfaces for assistive robotics. By integrating EMG-based signal classification with a structured state machine, we enabled a prosthetic hand to perform multiple functional grasps using only a few muscle inputs. The result is a control scheme that feels natural to the user while remaining lightweight enough for embedded deployment.

What stood out most to me was that even with **limited and noisy input data**, it is possible to build a model that can interpret these signals and translate them into meaningful actions. This highlights the potential of data-driven methods to create practical and responsive control systems, even under imperfect real-world conditions.

### Future Directions
Looking ahead, this approach could evolve toward more intelligent and adaptive prosthetic systems.

**Opportunities for expansion**
- Combining EMG with additional sensing modalities to improve robustness and enable richer control signals.
- Developing **user-specific adaptation algorithms** that adjust the model online to compensate for muscle fatigue or electrode shifts.

**Areas for improvement**
- Exploring alternative **electrode placements** to capture activity from additional muscle groups, leading to a greater number of distinguishable input classes.
- Simplifying the **state machine** to reduce latency when switching between grasp types.
- Collecting larger, more diverse EMG datasets to improve generalization across users.

## Media
<div class="grid media-grid">

</div>
