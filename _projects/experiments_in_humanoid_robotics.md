---
title: "Experiments in Humanoid Robotics"
short: "Exploring biologically-inspired learning for humanoid robots."
tags: ["Humanoid Robotics", "Cerebellar Neural Networks", "Reinforcement Learning"]
hero: "/assets/nao_kick.jpeg"
thumb: "/assets/nao_kick.jpeg"
weight: 2023
---

## Motivation
Humanoid robots like NAO give us a unique chance to explore how ideas from neuroscience and machine learning can translate into embodied behavior. Unlike rigid industrial robots, humanoids are built to mimic human proportions and movements, which makes them an ideal platform to test biologically inspired control methods. At the same time, their small size and limited actuation power force us to be creative: motion planning, learning, and control all need to be lightweight and adaptive.

Through this series of **four experiments**, we explored how different learning methods—from simple sensorimotor mappings to reinforcement learning—can help a humanoid robot perceive, learn, and act more like a biological system:
1. **Perception and Motion Control:** Communicating with NAO, controlling joints, and processing camera input.
2. **Feedforward Neural Networks:** Learning visuomotor mappings for reaching using standard neural networks.
2. **CMAC Neural Networks:** Learning visuomotor mappings for reaching using cerebellar-inspired neural networks.
3. **Reinforcement Learning:** Teaching NAO to play soccer using reinforcement learning.

Together, these experiments gave us a step-by-step path from low-level control to adaptive learning and decision-making in humanoid robots.

## Experiment 1: Perception and Motion
### Approach
Our first step with NAO was to make it both **move and perceive**. Using the Robot Operating System (ROS) as the communication backbone, we wrote simple nodes that could send commands and receive feedback. With this setup, we could listen to tactile sensors, command joints, and stream camera images into our own programs. On top of that, we integrated **OpenCV** to detect colored objects in the robot's view.

### Findings
- The ROS publish–subscribe model provided a clear framework for structuring NAO's perception and control.
- Sensor feedback and joint commands could be synchronized reliably, enabling smooth low-level interaction.
- Real-time blob detection demonstrated how vision signals can be translated into actionable inputs for the control loop.

### Application
This experiment established the first functional bridge between **perception and motion**. With tactile sensing, joint actuation, and visual input all integrated into a single pipeline, NAO could now respond to its environment in real time. This closed-loop setup provided the technical baseline for building higher-level behaviors in later experiments.

### Media
<div class="grid media-grid">

</div>

## Experiment 2: Feedforward Neural Networks
### Approach
We implemented a simple **multi-layer perceptron (MLP)** in NumPy. Training data was collected by manually guiding NAO's arm to objects and recording both the **blob position (vision)** and **shoulder joint angles (motor)**. With about 50–60 samples, we normalized inputs/outputs and trained the network to map 2D visual positions to motor commands.

### Findings
- The trained network reproduced smooth reaching motions for objects it had seen before.  
- Generalization to unseen positions worked surprisingly well, given the small dataset.  
- The network had to be lightweight to run interactively, but was still expressive enough to learn nonlinear mappings.  

### Application
This experiment showed how a **visuomotor mapping** can emerge from data rather than explicit geometry. NAO could look at an object and directly move its arm toward it, an ability closer to biological reaching than to analytical inverse kinematics.

### Media
<div class="grid media-grid">

</div>

## Experiment 3: CMAC Neural Networks
### Approach
We implemented a **Cerebellar Model Articulation Controller (CMAC)**, a neural network inspired by how the human cerebellum discretizes and generalizes motor control. Unlike a dense MLP, the CMAC uses overlapping receptive fields in a lookup-table style, making it efficient and interpretable.

We trained the CMAC with 150 visuomotor samples and compared it against our earlier feedforward network.

### Findings
- CMAC learned the mapping faster, with lower mean squared error for the same number of samples.  
- However, its resolution and receptive field sizes had a large effect: small fields gave precision but poor generalization, while larger fields smoothed behavior but reduced accuracy.  

### Application
The CMAC experiment highlighted how **bio-inspired architectures** can provide advantages over generic deep learning—particularly in terms of speed, interpretability, and suitability for embedded robotics.

### Media
<div class="grid media-grid">

</div>

## Experiment 3: Reinforcement Learning
### Approach
For the final experiment, we implemented a **reinforcement learning (RL)** framework for NAO to perform penalty kicks in a robot soccer scenario. The method was based on the model-learning approach of Hester et al., *Generalized Model Learning for Reinforcement Learning on a Humanoid Robot*, ICRA 2010.

The setup consisted of:
- **State space (s):** ball position, goalkeeper position, and NAO's joint configuration.
- **Action space (a):** discrete kick trajectories generated by parameterized joint sequences.
- **Transition model (Pr(s' \mid s, a)):** estimated from experience using decision trees, which captured the probabilistic outcomes of different kicking motions against varying goalkeeper positions.
- **Reward function (R(s, a)):** defined by the success of the kick (goal scored = positive reward; blocked or missed = negative reward).

The algorithm iteratively updated the transition model from experience and selected actions according to a learned value function. Exploration was encouraged through ε-greedy action selection.

### Findings
- **Learning curve:** The cumulative reward increased steadily across episodes, confirming that NAO was adapting its strategy.
- **Robustness:** After training, NAO maintained a higher scoring rate even when the goalkeeper moved to new positions, showing generalization beyond the training set.
- **Model representation:** Decision trees worked well for modeling both the transition and reward functions, offering a good trade-off between interpretability and the ability to capture nonlinear behavior.
- **Limitations:** The discrete action space simplified the kicking motion but constrained expressiveness compared to continuous motor control.

### Application
This experiment showed us how reinforcement learning can be applied to **embodied control under uncertainty and feedback**. Through repeated trial-and-error interaction, NAO developed adaptive kicking strategies instead of relying on pre-scripted motions. Even in a simple penalty setup, we could see how trial-and-error learning leads to more adaptable and responsive behavior; the kind of behavior that could later extend to walking, balance recovery, or even teamwork on the field.

### Media
<div class="grid media-grid">

  <figure>
    <img src="/assets/nao/nao_right.gif" alt="Nao kicking right">
    <figcaption>NAO performing a penalty kick to the right.</figcaption>
  </figure>

  <figure>
    <img src="/assets/nao/nao_middle.gif" alt="Nao kicking middle">
    <figcaption>NAO performing a penalty kick straight to the middle.</figcaption>
  </figure>

  <figure>
    <img src="/assets/nao/nao_whoops.gif" alt="Nao losing balance while kicking ;(">
    <figcaption>NAO losing balance after the kick :').</figcaption>
  </figure>

</div>