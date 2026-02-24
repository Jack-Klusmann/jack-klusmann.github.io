---
title: "Controlling Soft Robots Part 3: Human-Robot Interaction and Teleoperation in Mixed Reality"
short: "Building immersive teleoperation pipelines for intuitive end-user control."
tags: ["Human-Robot Interaction", "Hybrid Robot Integration", "Mixed Reality"]
hero: "/assets/interact.jpg"
thumb: "/assets/interact.jpg"
weight: 2026
---

---
title: "Teleoperating Hybrid Robots in Augmented Reality"
short: "AR-based teleoperation framework for hybrid rigid–soft robotic manipulation."
tags: ["Augmented Reality", "Teleoperation", "Soft Robotics", "Human–Robot Interaction"]
hero: "/assets/ar_teleoperation.jpg"
thumb: "/assets/ar_teleoperation.jpg"
weight: 2026
---

## Motivation

Hybrid rigid–soft robots combine the **precision of rigid manipulators** with the **compliance and adaptability of soft continuum arms**. This combination is especially promising for grasping in unstructured environments: the rigid arm provides accurate positioning, while the soft structure enables safe, conforming contact with objects. However, teleoperating such hybrid systems is fundamentally challenging.

With a rigid robot, the relationship between command and motion is predictable. If you move the end-effector to a target pose, you can reasonably anticipate what the robot will do. The kinematics are well understood, and the behavior is largely configuration-independent.

Soft robots behave very differently. Their motion depends not only on actuation, but also on gravity, internal compliance, and interaction forces. The same tendon input can result in different shapes depending on orientation or load. As a user, you cannot always mentally simulate what will happen next. This makes direct teleoperation much less intuitive.

These difficulties become even more pronounced when rigid and soft subsystems are combined. It is no longer just a question of how to control a soft robot, but how to coordinate precision and compliance within a single embodied system.

Traditional teleoperation interfaces designed for rigid robots, such as joysticks, teach pendants, or graphical user interfaces, do not translate well to this setting. Hybrid rigid–soft teleoperation remains relatively unexplored, and mapping intuitive human intent to coordinated rigid–soft motion in such systems is still an open problem.

In this project, we developed an augmented reality (AR)-based teleoperation framework that allows users to:

- Control the rigid and soft subsystems jointly within a unified hybrid robot. 
- Interact directly with a simulated version of the robot before executing motions on hardware.
- Perform tasks such as reaching, grasping, and pick-and-place in an intuitive way.

Our goal was to create a **human-in-the-loop control pipeline** that feels natural and immersive, while still being grounded in physically consistent simulation.

## Approach

### System Overview

The robotic platform consists of:

- A **7-DoF rigid robotic arm**
- A **tendon-driven spiral-shaped soft continuum arm** mounted at the end-effector
- A gripper at the tip of the soft arm

The system is deployed using:

- **MuJoCo** for physics simulation  
- **Unity** for rendering and interaction  
- **Meta Quest 3** for AR visualization and input  

MuJoCo handles all physics computation, while Unity manages AR visualization and user interaction. The result is a fully integrated **simulation-to-reality teleoperation pipeline**.

---

## AR-Based Teleoperation Concept

### Dual Target Specification

The user operates two AR-tracked controllers equipped with virtual laser pointers:

- One pointer defines the desired position of the **rigid arm end-effector**
- The second pointer specifies the target position of the **soft arm tip**

The laser rays are retractable, enabling intuitive 3D spatial interaction. By moving the ray endpoints in space, the user defines desired robot poses directly within the shared workspace.

This creates a clear and natural mapping:

- Rigid arm → coarse positioning and orientation  
- Soft arm → fine manipulation and compliant grasping  

---

### Simulation Preview Before Execution

A key feature of the framework is a **virtual preview mechanism**.

Before any command is executed on the physical robot:

1. The virtual robot performs the motion in simulation.
2. The user observes the predicted behavior in AR.
3. Adjustments can be made iteratively.
4. Once satisfied, the command is executed on hardware.

This preview loop significantly reduces trial-and-error during physical interaction and improves:

- Safety  
- Efficiency  
- User confidence  
- Task success rate  

Instead of blindly commanding the real robot, the user first evaluates the outcome in a physically grounded digital twin.

---

## Soft Arm Control Strategy

The soft continuum arm is constrained to **planar bending motions**, allowing unique shape mappings under single-degree-of-freedom actuation.

When the user specifies a soft-arm target outside its bending plane, the rigid robot automatically reorients its end-effector so that:

- The bending plane aligns with the user-defined direction  
- The soft arm can execute a physically consistent curl toward the target  

This automatic coordination between rigid and soft subsystems enables smooth whole-arm grasping behavior without requiring the user to manually reason about internal actuation constraints.

The soft arm inverse kinematics is handled by a lightweight neural network that maps:

- Gravity direction  
- End-effector position  
- End-effector orientation  

to tendon lengths in real time.

This allows low-latency control across the full reachable workspace.

---

## Experiments

We evaluated the AR teleoperation framework across multiple task categories:

### Free Motion
The user moves the virtual robot freely in 3D space, while the physical robot follows with a controlled delay.

### Reaching
The user teleoperates the system to reach a hanging target by coordinating rigid positioning and soft curling.

### Grasping
The system successfully grasps:
- Objects placed on flat surfaces  
- Objects suspended in mid-air  
- Objects handed over dynamically  

### Object Handling
The robot grasps an object from one surface, transfers it through mid-air, and releases it at a different location.

Across these experiments, the hybrid system demonstrated:

- Stable trajectory following  
- Successful contact formation  
- Secure grasping without slipping  
- Smooth rigid–soft coordination  

---

## What This Project Demonstrates

This work highlights several key ideas:

- **Hybrid embodiment requires hybrid control.**
- Intuitive teleoperation of soft robots is possible when grounded in physically consistent simulation.
- AR can serve as a powerful interface for human–robot interaction beyond simple visualization.
- Digital twin preview significantly enhances safety and usability in physical interaction tasks.

Most importantly, this project shows that even complex, high-DoF hybrid robots can be made accessible to human users through thoughtful interface design and real-time simulation.

---

## Future Directions

Potential extensions of this framework include:

- Shared autonomy and intention inference  
- Haptic feedback integration  
- Learning from teleoperation data for imitation learning  
- Quantitative user studies evaluating cognitive workload and intuitiveness  
- Extension to contact-rich and force-sensitive manipulation  

This project represents a step toward **immersive, simulation-grounded human–robot collaboration**, where physical robots can be safely and intuitively controlled in real environments through augmented reality.

## Media

<div class="video-container">
  <iframe 
    src="https://www.youtube.com/watch?v=xgFLkJ0uQh0" 
    title="Physical Human-Robot Interaction for Grasping in Augmented Reality via Rigid-Soft Robot Synergy"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen>
  </iframe>
</div>


## Links
[IEEE RoboSoft 2026 Paper](https://arxiv.org/abs/2602.17128)