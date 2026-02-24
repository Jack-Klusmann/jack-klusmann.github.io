---
title: "Controlling Soft Robots Part 3: Human-Robot Interaction and Teleoperation in Mixed Reality"
short: "Building immersive teleoperation pipelines for intuitive end-user control."
tags: ["Human-Robot Interaction", "Hybrid Robot Integration", "Mixed Reality"]
hero: "/assets/interact.jpg"
thumb: "/assets/interact.jpg"
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

## Approach

### System Overview

The AR-based teleoperation framework is built as a tightly integrated hardware–software stack. The system combines real-time simulation, mixed-reality visualization, rigid-arm control, and soft-robot actuation into a unified human–robot interaction pipeline.

At a high level:

- The user interacts through a **Meta Quest 3 headset and Touch Plus controllers**.
- Unity integrates mixed-reality rendering, tracking, and physics simulation.
- MuJoCo simulates the hybrid rigid–soft robot in real time.
- Control commands are streamed to two separate workstations:
  - One controlling the rigid arm.
  - One controlling the soft robot tendon motors.

This structure ensures that visualization, control, and physical execution remain synchronized.

### Hardware Stack

The physical system consists of the following components:

#### User Interface Layer

- **Meta Quest 3**
  - Provides stereoscopic mixed-reality visualization.
  - Overlays the simulated robot onto the physical robot.
  - Previews the resulting motion based on selected control actions.

- **Left Touch Plus Controller**
  - Controls the end-effector position of the rigid robot.
  - Provides 6-DoF pose tracking for rigid-arm target specification.

- **Right Touch Plus Controller**
  - Controls the soft robot.
  - Provides 6-DoF pose tracking for specifying soft-arm target positions.

#### Robot Actuation Layer

- **Franka Emika Panda**
  - 7-DoF rigid robotic arm.
  - Provides base motion and positioning for the hybrid system.
  - Controlled via the MoveIt control framework.

- **Soft Robot Tendon Motors**
  - Drive tendon actuation for the soft continuum arm.
  - Controlled through a custom USB-based PID control framework.

#### Control Infrastructure

- **Workstation A**
  - Receives rigid control values via UDP over Wi-Fi.
  - Sends actuation commands to the Franka Emika Panda.

- **Workstation B**
  - Receives soft control values via UDP over Wi-Fi.
  - Sends actuation commands to the tendon motors of the soft arm.

This distributed setup allows rigid and soft subsystems to be controlled independently while remaining coordinated through the AR interface.

### Software Stack

The software architecture integrates simulation, tracking, rendering, and deployment components.

#### Core Engine

- **Unity Engine**
  - Central integration platform.
  - Manages visualization, user interaction, and data exchange.
  - Connects simulation, tracking plugins, and external control systems.

#### Simulation Layer

- **MuJoCo Plugin**
  - Simulates the physical behavior of the hybrid rigid–soft robot.
  - Visualizes kinematic and dynamic responses.
  - Provides interfaces for actuation control.

#### Mixed-Reality & Tracking Layer

- **Meta XR Plugin**
  - Manages mixed-reality interactions.
  - Maps user inputs to robot control actions in physical space.

- **Meta OVR Plugin**
  - Provides real-time headset and controller tracking data.
  - Supplies pose information for rendering and interaction.

- **Meta MRUK Plugin**
  - Handles spatial mapping and passthrough rendering.
  - Aligns simulated and physical robots in mixed reality.

#### Deployment Pipeline

- **Gradle Build System**
  - Builds and packages the compiled application for Meta Quest.

- **SideQuest**
  - Transfers and manages the application on the Meta Quest device.

Together, this stack enables real-time bidirectional interaction:

- User inputs → Unity → MuJoCo simulation → Control commands → Physical robot  
- Robot state → Simulation update → Mixed-reality overlay → User feedback  

This architecture ensures that the digital twin, user interaction, and physical actuation remain tightly synchronized throughout teleoperation.




### Teleoperation Concept

#### Dual Target Specification

The user operates two AR-tracked controllers equipped with virtual laser pointers:

- One pointer defines the desired position of the **rigid arm end-effector**
- The second pointer specifies the target position of the **soft arm tip**

The laser rays are retractable, enabling intuitive 3D spatial interaction. By moving the ray endpoints in space, the user defines desired robot poses directly within the shared workspace.

This creates a clear and natural mapping:

- Rigid arm → coarse positioning and orientation  
- Soft arm → fine manipulation and compliant grasping  

---

#### Simulation Preview Before Execution

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

### Control Strategy

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

<div style="position:relative; width:100%; margin:0 auto; padding-bottom:56.25%; height:0; overflow:hidden; border-radius:10px; border:1px solid #eee; background:#000;">
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