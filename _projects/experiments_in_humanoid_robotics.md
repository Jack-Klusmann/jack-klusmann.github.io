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
2. **Learning Visuomotor Mappings:** Training both a standard neural network (MLP) and a cerebellar-inspired CMAC to map visual input to arm motion.
3. **Reinforcement Learning for Action Selection:** Teaching NAO to play soccer using reinforcement learning.

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


## Experiment 2: Learning Visuomotor Mappings
### Approach
To give NAO the ability to reach for objects based on vision, we explored two learning methods for mapping **2D visual input (blob position)** to **motor output (shoulder joint angles)**:

1. **Feedforward Neural Network (MLP):**
We implemented a lightweight multi-layer perceptron in NumPy from scratch, including custom layers, loss functions, and gradient-based optimization. Training data was collected by manually guiding NAO’s arm to objects and recording 100 pairs of blob positions and shoulder angles. Both input and output were normalized to [0,1] before training.

2. **Cerebellar Model Articulation Controller (CMAC):**
Inspired by the human cerebellum’s role in motor learning, the CMAC discretizes the continuous input space into a grid of overlapping receptive fields. Each input activates a small subset of these fields, and the corresponding table entries are combined to produce the output. Because only a fraction of the table is updated at each step, the network learns quickly while still generalizing to nearby inputs. We trained the CMAC with 100 samples and systematically varied the receptive field size to study how it affects accuracy (precision vs. smoothness of the mapping).

Both models were integrated into the ROS framework so that the learned mappings could run in real time on the NAO robot.

### Findings
- **MLP:**
  - Learned smooth, continuous mappings from vision to motor commands, producing realistic reaching motions.
  - Generalized reasonably well to unseen inputs despite the small dataset.
  - Computationally lightweight enough for interactive control, though training was sensitive to dataset size.

- **CMAC:**
  - Converged faster than the MLP, achieving lower mean squared error with the same training data.
  - Performance strongly depended on receptive field size:
    - Small fields improved precision but reduced generalization.
    - Larger fields smoothed the mapping but introduced systematic error.
  - Provided a more interpretable structure compared to the dense MLP.

### Application
- With the MLP, NAO learned to approximate the mapping in a continuous, data-driven way, resembling how artificial neural networks generalize patterns. This kind of model can grow with more data and scale to higher-dimensional tasks where smooth generalization is essential.
- With the CMAC, NAO relied on a discretized, cerebellum-inspired representation that favored fast learning and interpretability. This makes it especially useful in situations where a robot needs to adapt from just a few demonstrations, or where we want controllers that are transparent and easy to inspect.

### Media
<div class="grid media-grid">

</div>


## Experiment 3: Reinforcement Learning for Action Selection
### Approach
For the final experiment, we implemented a **reinforcement learning (RL)** framework for NAO to perform penalty kicks in a robot soccer scenario. The method was based on the model-learning approach of Hester et al., *Generalized Model Learning for Reinforcement Learning on a Humanoid Robot*, ICRA 2010.

The setup consisted of:
- **State space (s):** ball position, goalkeeper position, and NAO's joint configuration.
- **Action space (a):** discrete kick trajectories generated by parameterized joint sequences.
- **Transition model (Pr(s' \| s, a)):** estimated from experience using decision trees, which captured the probabilistic outcomes of different kicking motions against varying goalkeeper positions.
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