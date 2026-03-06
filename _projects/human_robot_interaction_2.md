---
title: "Controlling Soft Robots Part 2: Parameter Identification and Real-to-Sim Transfer"
short: "Leveraging motion capture data to build high-fidelity simulation models."
tags: ["Robotic Simulation", "System Identification", "Data-Driven Optimization"]
hero: "/assets/sim.png"
thumb: "/assets/sim.png"
weight: 2026
---

## Motivation

Soft continuum robots behave very differently from traditional rigid manipulators. Their motion emerges from the interaction between tendon actuation, internal material compliance, gravity, and external forces. While this flexibility enables highly adaptive grasping, it also makes the system much harder to model and predict.

For simulation-driven robotics pipelines, this creates a major challenge. If the simulated robot behaves differently from the real one, user interfaces, control policies, and learning algorithms trained in simulation may fail when deployed on hardware.

Addressing this gap is commonly referred to as **real-to-simulation (real-to-sim) transfer**, the process of aligning the behavior of a simulated robot with that of its physical counterpart. To achieve this on the **SpiraTrunk platform**, we developed a **simulation-centered parameter identification pipeline** that calibrates a MuJoCo model of the robot using real-world experimental data collected through motion capture. Rather than deriving a full analytical model, our approach focuses on identifying the parameters that govern how the robot behaves within the physics simulation.

The result is a calibrated simulation model capable of reproducing both the **static shape deformation** and the **dynamic motion behavior** of the physical robot.

## Approach

### Modeling the Soft Robot in MuJoCo

To simulate the robot efficiently, we represent the continuum arm using a **pseudo-rigid-body model**.

Rather than modeling the arm as a fully deformable structure, the backbone is discretized into multiple rigid segments connected by torsional spring–damper joints. Each joint approximates the elastic bending behavior of the soft material while remaining compatible with real-time physics simulation.

In this model, the passive torque at each joint follows a simple viscoelastic formulation:

- A **spring term** captures bending stiffness.
- A **damping term** suppresses oscillations during motion.

This representation captures the robot’s essential physical behavior while remaining computationally efficient enough for interactive simulation.

An important property of the robot is that its geometry follows a **logarithmic spiral structure**. Because of this, segment dimensions scale consistently along the backbone. This geometric regularity allows stiffness and damping parameters to be initialized using physically meaningful scaling laws before the optimization step.

### Motion Capture Data Collection

To calibrate the simulation model, we recorded the robot’s motion using an **OptiTrack motion capture system** with six infrared cameras.

Reflective markers were attached to **18 segments along the robot’s backbone**, allowing us to reconstruct the full 3D shape of the arm over time. The system recorded motion at 120 Hz, producing high-resolution trajectory data for both static and dynamic experiments.

To capture different aspects of the robot’s physical behavior, we designed three types of experiments:

**1. Static tilting experiments**
The robot base was tilted relative to gravity while the arm relaxed into its equilibrium configuration.

**2. Free uncurling experiments**
The robot was released from a bent configuration to observe its natural oscillations and damping behavior.

**3. Actuation experiments**
The robot performed curling and uncurling motions under different tendon actuation levels.

Together, these experiments provide complementary information about the robot’s material properties and the behavior of its control system.

### Staged Parameter Identification

Instead of optimizing all parameters at once, we structured the identification pipeline into **three sequential stages**.

#### Stage 1: Stiffness Identification

The first step estimates the bending stiffness of each segment.

Simulations reproduce the static tilting experiments, and the resulting robot shapes are compared with the motion capture data. The optimization minimizes the difference between simulated and measured segment positions while preserving the internal shape of the arm.

A **differential evolution optimizer** searches for the parameter values that best reproduce the observed deformation behavior.

#### Stage 2: Damping and Friction Identification

Next, we estimate the damping coefficients and tendon friction that determine the robot’s dynamic response.

In these experiments, the robot is released from a bent configuration and allowed to return to its neutral shape. The resulting oscillations reveal how energy dissipates throughout the structure.

To capture this behavior, the loss function combines:

- **Position error**, measuring differences in segment positions.
- **Velocity error**, capturing oscillation frequency and decay behavior.

This stage ensures that the simulation reproduces both the **shape trajectory** and the **temporal dynamics** observed in the real system.

#### Stage 3: Control System Identification

The final stage focuses on parameters related to the **actuation and control system**.

The physical robot is driven by tendon motors controlled through a triple-cascaded PID control loop. In simulation, this behavior is approximated using PD position actuators with tuned control gains, as the simulator does not support the full cascaded control architecture.

The following parameters are optimized: proportional gain, velocity gain, actuator force range, and actuator time constant.

By calibrating these parameters, the simulation captures how quickly and smoothly the robot responds to control commands.

## Findings

The calibrated simulation model closely reproduces the behavior of the real robot.

Before optimization, the simulated robot exhibited large discrepancies in shape and motion compared to the physical system. After parameter identification, these differences were significantly reduced. Across dynamic experiments, the **internal shape error decreased from 8.98 cm to 1.58 cm**, corresponding to a reduction from roughly **17% to 3% of the robot’s total length**.

This level of accuracy proved sufficient for **simulation-driven teleoperation**, where users interact with the digital twin before executing commands on the real robot (see Controlling Soft Robots Part 3).

## Application

A reliable real-to-simulation pipeline unlocks several important capabilities.

### Simulation-Based Teleoperation

The calibrated MuJoCo model forms the foundation of the **mixed-reality teleoperation system** described in the next project. Because the simulated robot behaves consistently with the real hardware, users can preview actions safely before executing them on the physical system.

### Data Generation for Learning

High-fidelity simulation also enables large-scale data generation.

Instead of collecting every trajectory on hardware, experiments can first be conducted in simulation to generate training data for inverse kinematics learning, control policy training, or motion prediction models.

In this sense, the simulation can become a **data engine** for learning-based control.

### Rapid Design Iteration

Once the modeling and identification pipeline is in place, it becomes possible to quickly test new robot designs or control strategies. Changes to robot geometry or actuation can be incorporated into the simulation and recalibrated using the same identification process.

### Future Directions

Ultimately, this work helps bridge one of the most persistent challenges in robotics: the **sim-to-real gap**.

By grounding simulation models in real experimental data, we can build digital twins that are not only visually similar to physical robots, but also **behaviorally consistent**. This creates a foundation for more advanced applications such as learning-based control, simulation-driven experimentation, and human-in-the-loop robot interaction.

## Media
Coming soon :)