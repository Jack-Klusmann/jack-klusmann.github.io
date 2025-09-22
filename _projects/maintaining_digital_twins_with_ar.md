---
title: "Maintaining Digital Twins using Augmented Reality"
short: "Developing AR interaction pipelines for digital twin maintenance in chemical facilities."
tags: ["Augmented Reality", "Digital Twins", "UI/UX"]
hero: "/assets/chemical_facility.png"
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

## Approach
We developed a **tablet-based AR application** designed around ease of use, ergonomics, and compatibility with industrial environments.

### Key Design Choices
- **Handheld tablet with pen input:** Explosion-safe, usable with gloves or dirty hands.
- **Visio-inertial odometry tracking:** Anchors the device relative to markers in the plant, ensuring 6DoF pose without reliance on specialized LIDAR.
- **Silhouette-based interaction:** Users trace outlines of components directly on captured photos.
- **Voxel carving algorithm:** Multiple 2D silhouettes from different perspectives are intersected to form a 3D volume.
- **Adaptive octree subdivision:** Ensures coarse volumes for large objects and fine detail for small ones.
- **Marching Cubes mesh generation:** Produces real-time previews and exportable volumes (glTF).

### Workflow
1. **Initialization:** Scan a marker to localize the tablet in the plant.
2. **Image capture:** Take photos of the target component from multiple angles.
3. **Silhouette drawing:** Roughly outline the component on each image using a pen.
4. **Intersection & carving:** System computes a 3D volume from overlapping silhouettes.
5. **Review & refine:** Preview the reconstructed mesh in AR, adjust by adding/removing images or re-drawing silhouettes.
6. **Export:** Sstore the annotated VoI for integration into CAD or digital twin systems.

## Findings
We evaluated the system with a **user study of 15 engineering students** in a laboratory facility with chemical-plant-like equipment.

### Quantitative Results
- **Usability (SUS):** Mean score 82.8 (“Excellent”).
- **Workload (NASA TLX):** Mean 18.5/100 (low cognitive & physical demand, though device weight was noted).
- **Learning effect:** Mean task completion time improved from 135s to 102s between first and fourth attempts.
- **Consistency:** Performance did not significantly vary by prior AR experience or academic background.

### Qualitative Feedback
- Users found the **annotation process intuitive and effective** even without prior AR training.
- The **3D previews** were valued for verifying annotation quality.
- **Main limitation:** The iPad device was large and heavy, especially for participants with smaller hand spans. Many suggested smartphones as more ergonomic.

### Expert Feedback
Industry engineers highlighted additional use cases:
- Documenting **temporary changes** (e.g., sensor placement).
- **Guided scanning** of selected sub-areas to update digital twins.
- Clearer **component selection** compared to pointer-based methods.

## Applications
This AR interaction pipeline demonstrates a practical and scalable way to maintain digital twins in real industrial environments:

- **Shift communication:** Workers can mark anomalies unambiguously for colleagues.
- **Maintenance & inspection:** Selected VoIs streamline planning and task execution.
- **Digital twin updates:** VoIs define scan regions, enabling targeted scanning and annotation.
- **Documentation:** Spatially contextual records of temporary sensors, replaced parts, or modifications.
- **Selection tool:** More robust than single-point picking in complex assemblies.

### Future Directions
- Integration into full **immersive digital twin interfaces** for chemical plants.
- Exploration of **lighter, more ergonomic devices** (smartphones, certified wearables).
- Support for **complex geometries** (e.g., components with holes).
- **Guided navigation & safety monitoring** to reduce risk in constrained plant environments.
- Potential use of **neural radiance fields** for large-scale localization.

## Media

<div class="grid media-grid">

  <figure>
    <img src="/assets/voi/voi_1.png" alt="Camera Tab">
    <figcaption>The app opens on the camera tab, with settings [1], camera [2], and photos [3] buttons.</figcaption>
  </figure>

  <figure>
    <img src="/assets/voi/voi_2.png" alt="AR Marker">
    <figcaption>The system outlines the Vuforia AR marker [4] when recognized.</figcaption>
  </figure>

  <figure>
    <img src="/assets/voi/voi_3.png" alt="Settings Tab">
    <figcaption>The settings tab allows the user to visualize AR computations if needed [5][6][7].</figcaption>
  </figure>

  <figure>
    <img src="/assets/voi/voi_4.png" alt="Settings Tab">
    <figcaption>The settings tab also lets the user manually specify the area of interest [8].</figcaption>
  </figure>

  <figure>
    <img src="/assets/voi/voi_5.png" alt="Settings Tab">
    <figcaption>Users can manually set the object size [9] and control the voxel mesh granularity [10]–[15].</figcaption>
  </figure>

  <figure>
    <img src="/assets/voi/voi_6.png" alt="Drawing Tab">
    <figcaption>After capturing a photo, the user can annotate the volume of interest, outlined in yellow [16], and edit if needed [17-19].</figcaption>
  </figure>

  <figure>
    <img src="/assets/voi/voi_7.png" alt="Drawing Tab">
    <figcaption>The user captures images and annotates them from multiple angles.</figcaption>
  </figure>

  <figure>
    <img src="/assets/voi/voi_8.png" alt="Drawing Tab">
    <figcaption>The user captures images and annotates them from multiple angles.</figcaption>
  </figure>

  <figure>
    <img src="/assets/voi/voi_9.png" alt="Photos Tab">
    <figcaption>In the photos tab, the user can view annotations [20], continue editing [21]–[23], or begin mesh reconstruction [24].</figcaption>
  </figure>

  <figure>
    <img src="/assets/voi/voi_10.png" alt="Photos Tab">
    <figcaption>Mesh reconstruction takes up to 5 seconds to calculate.</figcaption>
  </figure>

  <figure>
    <img src="/assets/voi/voi_11.png" alt="3D Reconstruction">
    <figcaption>The 3D mesh reconstruction of the volume of interest is shown in AR, where it can be further refined until satisfactory.</figcaption>
  </figure>

</div>

**Links:**
[IEEE ISMAR 2023 Paper](https://ieeexplore.ieee.org/document/10308192)