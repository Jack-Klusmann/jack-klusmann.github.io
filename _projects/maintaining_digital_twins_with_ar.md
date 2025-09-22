---
title: "Maintaining Digital Twins with Augmented Reality"
short: "Developing AR interaction pipelines for digital twin maintenance in chemical facilities."
tags: ["Augmented Reality", "Digital Twins", "UI/UX"]
hero: "/assets/robot.png"
thumb: "/assets/chemical_facility.png"
weight: 2022
---

## Motivation
Creating and maintaining **digital twins** of industrial facilities promises efficiency, sustainability, and safety improvements. However, keeping twins up to date is difficult:

- **Complex environments:** Chemical plants contain large networks of pipes, pumps, scaffolding, and components of varying shapes and visibility.
- **Safety constraints:** Workers often operate in explosive environments requiring certified devices and protective clothing.
- **Communication challenges:** Describing issues textually across shifts can be ambiguous and error-prone.
- **Annotation difficulty:** Existing 3D selection methods (primitive placement, extrusion, point picking) are ill-suited for cluttered, partially occluded, or concave structures.

To advance digitization in chemical plants, we explored how **workers with minimal AR experience** could intuitively specify a **Volume of Interest (VoI)** for further processing (inspection, annotation, scanning).

---

## Approach
We developed a **tablet-based AR application** designed around ease of use, ergonomics, and compatibility with industrial environments.

### Key Design Choices
- **Handheld tablet with pen input**: explosion-safe, usable with gloves or dirty hands.
- **Visio-inertial odometry tracking**: anchors the device relative to markers in the plant, ensuring 6DoF pose without reliance on specialized LIDAR.
- **Silhouette-based interaction**: users trace outlines of components directly on captured photos.
- **Voxel carving algorithm**: multiple 2D silhouettes from different perspectives are intersected to form a 3D volume.
- **Adaptive octree subdivision**: ensures coarse volumes for large objects and fine detail for small ones.
- **Marching Cubes mesh generation**: produces real-time previews and exportable volumes (glTF).

### Workflow
1. **Initialization** – scan a marker to localize the tablet in the plant.
2. **Image capture** – take photos of the target component from multiple angles.
3. **Silhouette drawing** – roughly outline the component on each image using a pen.
4. **Intersection & carving** – system computes a 3D volume from overlapping silhouettes.
5. **Review & refine** – preview the reconstructed mesh in AR, adjust by adding/removing images or re-drawing silhouettes.
6. **Export** – store the annotated VoI for integration into CAD or digital twin systems.

---

## Findings
We evaluated the system with a **user study of 15 engineering students** in a laboratory facility with chemical-plant-like equipment.

### Quantitative Results
- **Usability (SUS):** mean score 82.8 (“Excellent”).
- **Workload (NASA TLX):** mean 18.5/100 (low cognitive & physical demand, though device weight was noted).
- **Learning effect:** mean task completion time improved from 135s to 102s between first and fourth attempts.
- **Consistency:** performance did not significantly vary by prior AR experience or academic background.

### Qualitative Feedback
- Users found the **annotation process intuitive and effective** even without prior AR training.
- The **3D previews** were valued for verifying annotation quality.
- **Main limitation:** the iPad device was large and heavy, especially for participants with smaller hand spans. Many suggested smartphones as more ergonomic.

### Expert Feedback
Industry engineers highlighted additional use cases:
- Documenting **temporary changes** (e.g., sensor placement).
- **Guided scanning** of selected sub-areas to update digital twins.
- Clearer **component selection** compared to pointer-based methods.

---

## Applications
This AR interaction pipeline demonstrates a practical and scalable way to maintain digital twins in real industrial environments:

- **Shift communication:** workers can mark anomalies unambiguously for colleagues.
- **Maintenance & inspection:** selected VoIs streamline planning and task execution.
- **Digital twin updates:** VoIs define scan regions, enabling targeted scanning and annotation.
- **Documentation:** spatially contextual records of temporary sensors, replaced parts, or modifications.
- **Selection tool:** more robust than single-point picking in complex assemblies.

### Future Directions
- Integration into full **immersive digital twin interfaces** for chemical plants.
- Exploration of **lighter, more ergonomic devices** (smartphones, certified wearables).
- Support for **complex geometries** (e.g., components with holes).
- **Guided navigation & safety monitoring** to reduce risk in constrained plant environments.
- Potential use of **neural radiance fields** for large-scale localization.

---

**Links:**
[IEEE ISMAR 2023 Paper](https://ieeexplore.ieee.org/document/10308192)

## Media