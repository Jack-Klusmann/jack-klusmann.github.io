---
title: "Experiments in Soft Robotics"
short: "Exploring gripper design, curvature modeling, and soft sensing for soft robots."
tags: ["Soft Robotics", "Gripper Design", "Soft Sensing"]
hero: "/assets/origami.png"
thumb: "/assets/origami.png"
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
    <img src="/assets/soft/initial.png" alt="Tentacle Gripper">
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
We implemented the **Piecewise Constant Curvature (PCC) method** in Python to approximate the kinematics of a continuum actuator. The idea is to simplify the continuous deformation of a soft arm into circular arcs, each described by three parameters: curvature, arc length, and bending direction.

To build this step by step, we first defined the Denavit–Hartenberg (DH) transformations that would allow us to compute the pose of an arc in 3D space. We then created functions to generate arcs from given curvature parameters and visualize them, which gave us immediate feedback on whether our representation matched the expected shapes.

With this foundation, we implemented the two mappings central to PCC:
- **Actuator-specific mapping (actuator space → configuration space):** converting tendon (string) lengths into arc parameters.
- **Actuator-independent mapping (configuration space → task space):** using DH transformations to convert arc parameters into the 3D pose of the actuator tip.

Finally, we tested the reverse direction by implementing the inverse mappings, which allowed us to start from a target tip pose, calculate the corresponding arc parameters, and then map those back to tendon lengths. After validating this for a single arc, we extended the framework to two arcs and visualized more complex shapes such as C- and S-bends.

### Findings
- The single-section model reproduced smooth, realistic bending motions with only three arc parameters.
- Running forward and inverse mappings consistently returned the same tendon lengths, confirming that both transformations were implemented correctly.
- Extending to two arcs showed how more complex shapes (such as C- and S-bends) can be represented, making the method scalable to multi-section soft arms.

### Application
The PCC framework gave us a way to approximate continuum robots without the heavy computations of full physics models such as Cosserat rods. Instead of solving partial differential equations for every material property, we only needed a few arc parameters to describe the actuator’s shape. This made the model lightweight enough to run interactively, which is essential for fast simulation or real-time control.

At the same time, PCC improves on purely geometric splining approaches. While splines can generate smooth shapes, their parameters lack direct physical meaning. In contrast, PCC describes each section with arc parameters that correspond directly to actuation inputs such as tendon lengths or pressures. This makes the representation not only efficient to compute but also physically interpretable and more useful for control.

Overall, PCC provided a balanced approach: computationally simple enough for practical use, yet structured enough to capture the key behaviors of continuum robots.  

### Media
<div class="grid media-grid">

  <figure>
    <img src="/assets/soft/single_arc.png" alt="Granular Jamming Gripper">
    <figcaption>PCC model for a single actuator arc.</figcaption>
  </figure>

  <figure>
    <img src="/assets/soft/double_arc_c.png" alt="Granular Jamming Gripper">
    <figcaption>PCC model of a two-section actuator forming a C-shape.</figcaption>
  </figure>

  <figure>
    <img src="/assets/soft/double_arc_s.png" alt="Granular Jamming Gripper">
    <figcaption>PCC model of a two-section actuator forming an S-shape.</figcaption>
  </figure>

</div>


## Experiment 4: Measuring Actuator Force
### Approach
We built an Arduino-based setup with a load cell to perform **blocked-tip force measurements** on pneumatic actuators.  
In this method, the actuator tip is fixed in place so that all generated force is transferred directly to the sensor. We programmed automatic pressurization and depressurization cycles at three different frequencies (0.1 Hz, 0.2 Hz, 1 Hz) and recorded the corresponding pressure–force curves.

### Findings
- The measurements revealed clear **hysteresis** between loading and unloading, caused by material elasticity and airflow dynamics.
- At higher frequencies (1 Hz), the actuator could not reach equilibrium, leading to lower peak forces and noticeable lag.
- At lower frequencies (0.1–0.2 Hz), the actuator had more time to stabilize, resulting in smoother and more consistent curves.

### Application
Blocked-tip force measurements are a standard way to evaluate the **force capacity and efficiency** of soft actuators. They provide a baseline for comparing different actuator designs and highlight trade-offs between speed, stability, and maximum output — insights directly relevant for grasping and manipulation tasks.  

### Media
<div class="grid media-grid">

  <figure>
    <img src="/assets/soft/0_1HzS.jpg" alt="Hysteresis curve at 0.1 Hz">
    <figcaption>Hysteresis curve for 0.1 Hz cycle frequency.</figcaption>
  </figure>

  <figure>
    <img src="/assets/soft/0_1HzT.jpg" alt="Time progression at 0.1 Hz">
    <figcaption>Timely progression of pressure and force at 0.1 Hz.</figcaption>
  </figure>

  <figure>
    <img src="/assets/soft/0_2HzS.jpg" alt="Hysteresis curve at 0.2 Hz">
    <figcaption>Hysteresis curve for 0.2 Hz cycle frequency.</figcaption>
  </figure>

  <figure>
    <img src="/assets/soft/0_2HzT.jpg" alt="Time progression at 0.2 Hz">
    <figcaption>Timely progression of pressure and force at 0.2 Hz.</figcaption>
  </figure>

  <figure>
    <img src="/assets/soft/1HzS.jpg" alt="Hysteresis curve at 1 Hz">
    <figcaption>Hysteresis curve for 1 Hz cycle frequency.</figcaption>
  </figure>

  <figure>
    <img src="/assets/soft/1HzT.jpg" alt="Time progression at 1 Hz">
    <figcaption>Timely progression of pressure and force at 1 Hz.</figcaption>
  </figure>

</div>


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