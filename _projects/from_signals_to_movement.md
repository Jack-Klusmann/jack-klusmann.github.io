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

<!--
## Approach
To achieve reliable control, we designed a complete EMG signal processing and classification pipeline that combines **signal preprocessing**, **feature extraction**, **neural network-based classification**, and **post-processing**. In addition, we implemented a **state machine** to translate the classified signals into corresponding grasp types. The entire system was developed in **MATLAB** and **Simulink**, enabling both algorithm design and real-time testing on the prosthesis hardware.

### Signal Classification

#### 1. Raw Data Collection
Surface EMG signals are recorded from **four Ottobock MyoBock electrodes** placed on the user's residual limb. Each channel captures the electrical activity of a different muscle group responsible for hand and wrist motions, transmitting signals from the arm to the hand.

#### 2. Preprocessing
The signals are sampled at high frequency and segmented into overlapping time windows of **800 ms** with a **400 ms overlap**, providing both temporal continuity and responsiveness during real-time operation.

#### 3. Feature Extraction
Each window is transformed into a compact feature vector describing the temporal and dynamic characteristics of the EMG signal. Extracted features included:

- Temporal moment
- Slope sign change
- Zero crossings
- Root mean square (RMS)
- Waveform length
- Maximum fractal length
- Average amplitude change
- Enhanced mean absolute value

These features are widely used in EMG signal analysis and effectively capture both time-domain and nonlinear properties of muscle activity.

#### 4. Dimensionality Reduction
To reduce computational load and improve generalization, we apply **Linear Discriminant Analysis (LDA)** to project the feature space to a lower-dimensional representation before classification.

#### 5. Classification Models
We implemented and compared several classification approaches to evaluate performance, robustness, and real-time suitability:

- **k-Nearest Neighbors (kNN):** Simple and easy to implement but computationally expensive during inference, making it less suitable for real-time embedded deployment.

- **Logistic Regression (LRC):** Fast and interpretable, but its linear decision boundaries limit performance on complex, nonlinear EMG data.

- **Feedforward Neural Network (FFNN):** Provides nonlinear decision-making capability but requires careful tuning to avoid overfitting and must remain lightweight to meet real-time constraints.

- **Convolutional Neural Network (CNN):** Capable of automatically extracting hierarchical spatial–temporal features from EMG signals, improving generalization and robustness against signal variability.

The final model we used is a **CNN** with **leaky ReLU** activations. The network was trained using **mini-batch gradient descent** and the **Adam optimizer**, with **batch normalization** to improve convergence and stability.

#### 6. Post-Processing
To ensure smooth and stable predictions during real-time control, we implemented a **probability-based refinement** of the classifier output:

- If the first three labels agreed, the pose was assigned.
- If the second and third labels agreed and their combined probability exceeded 75%, the pose was assigned.
- If the third label's probability exceeded 90%, the pose was assigned.
- If the first two labels agreed and their combined probability exceeded 80%, the pose was assigned.
- In all other cases, the pose defaulted to rest.

### State Machine
The challenge here is taht the user cannot do a lot of different msucle activations.
To convert the classified EMG signals into meaningful and practical control, we implemented a **state machine** that maps the user's muscle activity to specific grasp types. The state machine provides an intuitive interface between the **signal classification pipeline** and the **prosthesis control logic**, ensuring smooth and predictable transitions between actions.

The system recognizes three main EMG states — **Rest**, **Open**, and **Close** — which serve as the user's primary control inputs. Depending on the current state and the duration or sequence of these inputs, the prosthesis transitions through a hierarchy of grasping modes. This structure enables the user to access multiple grasp types without needing explicit manual selection, making control more fluid and natural.

#### Grasp Transitions
- From **Rest**, the user can trigger **Open** or **Close** depending on muscle activation patterns.  
- Short or long activations allow movement between **Grasp Open**, **Grasp Rest**, and **Grasp Close** states, controlling the opening and closing of the hand.  
- When fully open, the system enters the **Fully Open** state, which allows selection of specific grasp types such as **Palmar Pinch**, **Power Grasp**, **Precision Sphere**, or **Lateral Pinch**.  
- Separate states handle **wrist rotation**, allowing transitions between **Pronation**, **Wrist Rest**, and **Supination** for more complete manipulation tasks.

#### Control Logic
The design prioritizes **consistency and ease of use**:
- Short activations lead to small, precise movements.  
- Long activations trigger full hand or wrist transitions.  
- All transitions are designed to be **non-blocking**, allowing quick return to the resting position at any point.

By combining this structured control logic with the EMG classifier, the state machine enables **multi-grasp, multi-motion control** using only three intuitive muscle commands. This approach strikes a balance between simplicity for the user and flexibility in functionality, supporting a wide range of everyday tasks such as grasping, lifting, rotating, and releasing objects. -->

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
