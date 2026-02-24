---
title: "Controlling Soft Robots Part 3: Human-Robot Interaction and Teleoperation in Mixed Reality"
short: "Building immersive teleoperation pipelines for intuitive end-user control."
tags: ["Human-Robot Interaction", "Hybrid Robot Integration", "Mixed Reality"]
hero: "/assets/interact.jpg"
thumb: "/assets/interact.jpg"
weight: 2026
---

## Motivation

Hybrid rigid–soft robots combine the **precision of rigid manipulators** with the **compliance and adaptability of soft continuum arms**. This makes them especially promising for grasping in unstructured environments: the rigid arm provides accurate positioning, while the soft structure enables safe, conforming contact with nearby objects. However, teleoperating such systems is fundamentally challenging.

Rigid robots exhibit largely **predictable kinematics**. When a target pose is specified, the resulting motion is configuration-independent and easy to anticipate. Soft robots behave very differently. Their motion depends not only on actuation, but also on gravity, internal compliance, and interaction forces. The same tendon input can produce different shapes depending on orientation or load. As a result, users cannot reliably predict what will happen next, making direct teleoperation far less intuitive.

These challenges intensify in hybrid systems. The problem is no longer just controlling a soft robot, but coordinating **precision and compliance within a single embodied system**.

Traditional interfaces designed for rigid manipulators, such as joysticks, teach pendants, or GUIs, do not translate well to this setting. Hybrid rigid–soft teleoperation remains relatively unexplored, and mapping intuitive human intent to coordinated rigid–soft motion is still an open problem.

To address this, we developed an augmented reality (AR)-based teleoperation framework that enables users to:

- Control rigid and soft subsystems jointly within a unified hybrid robot  
- Preview motions in simulation before executing them on hardware  
- Perform reaching, grasping, and pick-and-place tasks intuitively  

The goal was to create a **human-in-the-loop control pipeline** that feels natural and immersive while remaining grounded in physically consistent simulation.

## Approach

### System Overview

The AR-based teleoperation framework is built as an integrated hardware–software pipeline that unifies real-time simulation, mixed-reality visualization, and hybrid rigid–soft robot control.

The user interacts through a **Meta Quest 3 headset and Touch Plus controllers**, while **Unity** serves as the central integration layer for rendering, tracking, and communication. A **MuJoCo** model simulates the hybrid robot in real time, and validated control commands are streamed to separate workstations driving the rigid arm and the soft tendon motors.

A demonstration of the system in action is available in the Media section below.

### Hardware Stack

The system consists of three main layers: user interface, robot actuation, and distributed control.

**User Interface**  
A Meta Quest 3 headset provides mixed-reality visualization and overlays the simulated robot onto the physical system. Two Touch Plus controllers map directly to the rigid and soft subsystems, enabling 6-DoF target specification for each.

**Robot Actuation**  
A 7-DoF Franka Emika Panda provides global positioning, while a tendon-driven soft continuum arm enables compliant manipulation. The rigid arm is controlled via MoveIt, and the soft arm through a custom PID-based motor control framework.

**Control Infrastructure**  
Control commands are streamed via UDP to two separate workstations: one driving the rigid arm and one driving the soft arm. This distributed setup allows both subsystems to operate independently while remaining synchronized through the AR interface.

### Software Stack

The software architecture integrates simulation, tracking, and mixed-reality rendering into a unified control loop.

**Core Integration**  
Unity serves as the central orchestration layer, handling visualization, user interaction, networking, and communication with external control systems.

**Physics Simulation**  
A MuJoCo plugin runs the hybrid rigid–soft robot model in real time, providing physically grounded simulation and actuation interfaces.

**Mixed Reality & Tracking**  
Meta XR, OVR, and MRUK plugins provide headset and controller tracking, spatial mapping, and passthrough alignment, ensuring that the simulated robot remains registered to the physical system.

Together, this stack enables real-time bidirectional interaction:

User input → Unity → MuJoCo simulation → Control commands → Physical robot  
Robot state → Simulation update → AR overlay → User feedback  

This architecture keeps the digital twin, user interaction, and physical actuation tightly synchronized throughout teleoperation.

### Teleoperation Workflow

Despite the complexity of the underlying stack, using the system follows a **simple and intuitive flow**.

#### 1. Registration and Alignment

After launching the application, the user looks at a QR marker attached to the robot base. The system registers the simulated model to the physical robot and overlays the virtual robot directly onto it. Once aligned, the **digital twin remains spatially anchored**, allowing the user to interact with a unified hybrid embodiment in which simulation and hardware share the same space.

#### 2. Dual-Hand Interaction

Each controller maps to one subsystem: the **left controller** operates the rigid arm and the **right controller** controls the soft arm. Both emit retractable virtual rays that allow the user to specify **end-effector poses directly in 3D**.

The **index trigger adjusts position**, the **middle trigger adjusts orientation**, and the thumbstick controls ray length. The interaction scheme is consistent across subsystems while respecting their fundamentally different physical behaviors.

#### 3. Simulation-First Control

All actions are executed in **simulation first**.

When a target is specified, inverse kinematics are computed and the virtual robot updates in real time. The user observes the predicted motion in AR and confirms it only if it matches their intent. Otherwise, the system can be reset instantly without affecting the hardware. Confirmed motions are then streamed to the physical robot.

This creates a tight preview loop: **specify → observe → refine → execute**.

#### 4. Coordinated Hybrid Behavior

The rigid arm provides **global positioning**, while the soft arm enables **compliant local manipulation**. In practice, the rigid subsystem brings the robot into place and the soft arm executes controlled curling to grasp or interact.

Because the soft arm is constrained to **planar bending**, specifying a target outside its bending plane automatically triggers reorientation of the rigid end-effector. The system aligns the bending plane with the user-defined direction so the soft arm can execute a physically consistent curl.

This coordination happens **transparently**. The user does not need to reason about curvature constraints or tendon geometry; they simply indicate a spatial target and the system resolves the rigid–soft interaction internally.

A lightweight neural network performs **real-time inverse kinematics**, mapping gravity direction and end-effector pose to tendon lengths. This enables **low-latency, physically grounded control** across the reachable workspace.

From the user’s perspective, it feels like controlling a **single embodied system**, even though rigid and soft subsystems are simulated and actuated separately.

## Findings

### Experiments

We evaluated the AR teleoperation framework across multiple task categories:

- **Object Following:** The user moves the virtual robot freely in 3D space, while the physical robot follows with a controlled delay.
- **Object Reaching:** The user teleoperates the system to reach a hanging target by coordinating rigid positioning with soft-arm curling.
- **Object Grasping:** The system successfully grasps objects placed on flat surfaces, suspended in mid-air, or handed over dynamically.
- **Object Handling:** The robot grasps an object from one surface, transfers it through mid-air, and releases it at a different location.

### System Evaluation

Across these experiments, the hybrid system demonstrated stable trajectory following, successful contact formation, secure grasping without slipping, and smooth coordination between rigid positioning and soft manipulation.

### User Evaluation

Initial user trials indicated that the AR interface significantly lowered the barrier to controlling the hybrid system. The dual-target specification and simulation preview made task execution more predictable and reduced hesitation during interaction. Users reported that coordinating the rigid and soft subsystems felt more intuitive than expected once the visual feedback loop was established.

### Insights

This work highlights several key ideas:

- **Hybrid embodiment requires hybrid control.**
- Intuitive teleoperation of soft robots becomes feasible when grounded in physically consistent simulation.
- AR can serve as a powerful interaction layer, not just a visualization tool.
- Digital twin preview meaningfully enhances safety, confidence, and usability during physical interaction.

Most importantly, this project demonstrates that even complex, high-DoF hybrid robots can be made accessible to human users through thoughtful interface design and real-time simulation-based feedback.

## Application

While this framework enables intuitive teleoperation, I see its most powerful application as a tool for **teaching robots through demonstration**.

Rather than programming behaviors explicitly, users can control the hybrid robot naturally in mixed reality and demonstrate how a task should be performed. A sequence of such demonstrations generates high-quality data that can be used for **imitation learning (IL)** or behavior cloning.

Because the user interacts through a physically grounded digital twin, these demonstrations are:

- Safe, thanks to simulation preview.
- Structured, since targets are specified explicitly in 3D space.
- Consistent across trials.
- Directly transferable to real hardware.

In practice, this opens up several concrete possibilities:

- **Task teaching:** Users can demonstrate grasping, pick-and-place, or coordinated manipulation behaviors without writing low-level control code.
- **Rapid data collection:** Complex hybrid robot motions can be recorded efficiently for supervised policy learning.
- **Hybrid coordination learning:** Models can learn how to coordinate rigid positioning and soft manipulation jointly.
- **Dataset generation for physical AI:** The AR interface provides a scalable way to collect multimodal interaction data grounded in real robot dynamics.

Instead of treating teleoperation as the final control strategy, this system turns it into a **data engine for learning-based control**.


### Future Directions

Upcoming extensions of this framework include:

- Quantitative user studies evaluating performance, cognitive workload, and intuitiveness.
- Extending the simulation to consider contact-rich and force-sensitive manipulation with objects and surfaces.

More broadly, this project represents a step toward **immersive, simulation-grounded human–robot collaboration**, where humans do not just operate robots, but teach them.

## Media

<div class="grid media-grid">

  <figure>
    <img src="/assets/hri/vr.jpeg" alt="1st Person View (VR)">
    <figcaption>The 1st person view in the VR mode, used for initial testing.</figcaption>
  </figure>

  <figure>
    <img src="/assets/hri/ar.jpeg" alt="1st Person View (AR)">
    <figcaption>The 1st person view in AR mode, used for user experiments.</figcaption>
  </figure>

  <figure>
    <img src="/assets/hri/human.jpeg" alt="3rd Person View">
    <figcaption>Me interacting with the robot :)</figcaption>
  </figure>

</div>

<div style="position:relative; width:100%; margin:40px auto 0 auto; padding-bottom:56.25%; height:0; overflow:hidden; border-radius:10px; border:1px solid #eee; background:#000;">
  <iframe
    src="https://www.youtube.com/embed/xgFLkJ0uQh0"
    title="Physical Human-Robot Interaction for Grasping in Augmented Reality via Rigid-Soft Robot Synergy"
    style="position:absolute; top:0; left:0; width:100%; height:100%; border:0;"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen>
  </iframe>
</div>

## Links
[IEEE RoboSoft 2026 Paper](https://arxiv.org/abs/2602.17128)