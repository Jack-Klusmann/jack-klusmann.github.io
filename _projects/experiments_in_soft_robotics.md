---
title: "Experiments in Soft Robotics"
short: "Exploring gripper design, curvature modeling, and soft sensing for soft robots."
tags: ["Soft Robotics", "Gripper Design", "Soft Sensing"]
hero: "/assets/origami.png"
thumb: "/assets/origami.png"
weight: 2024
---

---
title: "Explorations in Soft Robotics"
short: "Hands-on experiments with grippers, actuators, and soft sensors."
tags: ["Soft Robotics", "Grippers", "Continuum Robots", "Sensors"]
hero: "/assets/soft_robotics.png"
thumb: "/assets/soft_robotics.png"
weight: 2024
---

## Motivation  
Soft robotics takes a very different approach to building robots than rigid robotics: instead of metal arms and grippers, it uses **flexible and compliant materials** that bend, stretch, and conform to whatever they touch. This opens up entirely new possibilities, enabling robots to handle fragile objects without breaking them, adapt to irregular shapes, and interact safely alongside people.

With these promises also come challenges. Soft robots are difficult to model, since they do not have predictable joints. Their actuation mechanisms can be unreliable or slow. Measuring forces and sensing motion often requires inventing new components from scratch. And ultimately, integrating all of this into a working system means rethinking how we design, control, and evaluate robots.

To explore these questions, my teammates and I carried out a series of **five experiments**, each focusing on a different aspect of soft robotics:
1. **Gripper Design:** How can soft grippers be designed and actuated?
2. **Universal Gripping:** How can particle jamming enable universal gripping?
3. **Continuum Modeling:** How can continuum robots be modeled mathematically?
4. **Force Measurement:** How can actuators generate measurable force?
5. **Soft Sensing:** How can compliant sensors be built from biocompatible materials?

Together, these experiments gave us a practical foundation in how **design, actuation, modeling, and sensing** come together in soft robotic systems and what some of the challenges are.

## Experiment 1: Origami and Bio-Inspired Grippers
### Approach
We built two vacuum-driven grippers with very different design inspirations:
- An **origami “magic ball”** structure that contracts uniformly around objects.
- A **tentacle-like gripper** inspired by jellyfish, intended to entangle and latch onto objects.
We built the origami designs and embedded them in vacuum plastic bags that could be inflated and deflated using a motor. Deflating causes the gripper to close and grasp, while inflating allows it to open and relax.  

We tested the grippers on objects of different sizes and shapes and measured their force–pressure characteristics.

### Findings
- The **magic ball gripper** achieved stable grasps on smaller and moderately sized objects, but tended to lose precision when scaled up.
- The **tentacle design** offered more reach and the potential to catch onto irregular surfaces, but airtightness issues limited its effectiveness.
- In both cases, performance improved with increased contact area, but over-compression reduced grip quality before the gripper recovered at higher pressures.

### Application
These experiments showed how different design strategies trade off **precision versus adaptability**. Magic ball-style grippers excel at controlled, repeatable grasps, while tentacle-inspired designs highlight how bio-inspired structures could expand the range of objects a soft gripper can handle.

### Media
<div class="grid media-grid">

  <figure>
    <img src="/assets/soft/ball.jpg" alt="Magic Ball Gripper">
    <figcaption>The magic ball gripper, inspired by https://www.youtube.com/watch?v=xRrLTwrZCBI.</figcaption>
  </figure>

  <figure>
    <img src="/assets/soft/hand.jpeg" alt="Tentacle Gripper">
    <figcaption>The tentacle-like gripper, inspired by the octopus.</figcaption>
  </figure>

  <figure>
    <img src="/assets/soft/initial.jpeg" alt="Tentacle Gripper">
    <figcaption>Preliminary design of a tentacle-like gripper.</figcaption>
  </figure>

</div>


## Experiment 2: Particle Jamming Gripper
### Approach
We built a **granular jamming gripper** by filling a balloon with coffee grounds. When the air inside the balloon was evacuated, the particles jammed together and locked into place. Pressing the gripper onto an object in this state allowed it to conform to the surface and lift the object.

We tested several variations:
- **Granular material:** Coffee grounds vs. dry rice.
- **Surface friction:** With and without adhevsie rubber dots on the membrane.
- **Quantity:** Different amounts of filling to study sealing and conformity.

### Findings
- Gripping relied on a combination of **friction, suction, and geometric interlocking**.
- **Coffee grounds** performed better than rice because their smaller grain size improved conformity.
- Force–pressure curves followed mostly linear trends for coffee, while rice introduced nonlinear effects.
- Adding **adhevsive rubber** increased surface friction, broadening the range of graspable objects.
- Objects with crevices, irregular shapes, or protrusions were the easiest to grasp.

### Application
These experiments showed that, unlike rigid grippers that require carefully planned contact points, the particle jamming gripper adapts passively to whatever it touches, making it especially useful for handling objects with irregular shapes or unknown geometry.

### Media
<div class="grid media-grid">

  <figure>
    <img src="/assets/soft/jamming_1.jpg" alt="Granular Jamming Gripper">
    <figcaption>The granular jamming gripper with adhesive rubber added for extra friction.</figcaption>
  </figure>

  <figure>
    <img src="/assets/soft/jamming_2.png" alt="Granular Jamming Gripper">
    <figcaption>The granular jamming gripper in its inflated state.</figcaption>
  </figure>

  <figure>
    <img src="/assets/soft/cube.png" alt="Granular Jamming Gripper">
    <figcaption>The granular jamming gripper grasping a cube.</figcaption>
  </figure>

</div>


## Experiment 3: Modeling with Piecewise Constant Curvature
### Approach
I implemented the **Piecewise Constant Curvature (PCC) method** in Python to model a continuum actuator.  
This involved actuator-specific mappings (string lengths → arc parameters) and actuator-independent mappings (arc parameters → 3D pose using DH parameters).  

### Application
The PCC framework provides a **computationally efficient model** for soft continuum robots, making real-time simulation and control feasible.  

### Findings
- The model reproduced realistic bending motions using only a few arc parameters.  
- Forward and inverse kinematics were consistent, validating the mapping.  
- Extending from one to two actuator sections illustrated how multi-segment soft arms can be controlled.  


## Experiment 4: Measuring Actuator Force
### Approach
I set up an Arduino-based system with a load cell to perform **blocked-tip force measurements** on pneumatic actuators.  
Automatic pressurization/depressurization cycles were run at different frequencies (0.1 Hz, 0.2 Hz, 1 Hz).  

### Application
Measuring blocked-tip force provides insights into the **power output and efficiency** of soft actuators, relevant for grasping and manipulation tasks.  

### Findings
- Clear **hysteresis** appeared between loading and unloading due to material elasticity and air dynamics.  
- At higher frequencies, maximum force output dropped and lag increased.  
- Lower frequencies allowed equilibrium and yielded more stable force–pressure curves.  


## Experiment 5: Soft Fluidic Strain Sensor
### Approach
I fabricated a **biocompatible strain sensor** by injecting an electrolyte solution into a silicone tube and sealing electrodes at both ends.  
The sensor was connected to a comparator-based relaxation oscillator, and resistance changes were measured as signal period shifts on an oscilloscope.  

### Application
Such sensors can be embedded into soft robots or wearable systems, providing **real-time strain feedback** with high compliance and low cost.  

### Findings
- Stretching the tube increased resistance, which directly increased the oscillation period.  
- Resistance changed by ~1.6 MΩ between relaxed and stretched states.  
- The measured strain (~18%) matched the theoretical model derived from resistance scaling.