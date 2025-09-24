---
title: "Maintaining Digital Twins using Augmented Reality"
short: "Developing AR interaction pipelines for digital twin maintenance in chemical facilities."
tags: ["Augmented Reality", "Digital Twins", "UI/UX"]
hero: "/assets/chemical_facility.png"
thumb: "/assets/chemical_facility.png"
weight: 2022
---

## Motivation
In recent years, industrial companies (especially those in chemicals, energy, and manufacturing) have become increasingly interested in creating **digital twins** of their facilities. These twins can be used to monitor equipment health, plan maintenance or inspections, and train workers in a safe virtual environment.

The challenge, however, is keeping these twins accurate and up to date. Industrial facilities are constantly changing: new pipes are added, equipment is replaced, scaffolding goes up or comes down, and the digital twin has to reflect all of it. One way to support this maintenance is by allowing workers to mark specific areas or equipment that need attention, a process often referred to as **Volume of Interest (VoI) annotation**.

Keeping the twin updated in this way quickly becomes an ongoing problem, for several reasons:
1. **Complex environments:** Industrial plants are dense networks of pipes, pumps, and scaffolding. Many components are partly hidden or difficult to reach, making them challenging to capture in detail.
2. **Safety constraints:** Workers must often use certified devices and protective clothing, which limits the types of equipment that can be brought into the plant for 3D scanning or annotation.
3. **Communication gaps:** Important updates are still passed along in text or speech between shifts, and this can easily lead to misunderstandings, delays, or mistakes.
4. **Annotation tools:** Common 3D selection methods (such as point picking or placing simple shapes) are not well suited to cluttered, irregular structures, especially when parts are partially occluded.

As part of a project with a chemical company, my colleagues and I explored how **workers with minimal AR experience** could intuitively specify a **Volume of Interest (VoI)**, for example a piece of equipment, for further processing. This could mean targeted inspection by other workers, or automated 3D photogrammetric scanning and reconstruction carried out by mobile robots operating in the facility.

## Approach
To address this challenge, we built a **tablet-based AR application** that makes Volume of Interest (VoI) annotation simple and practical in real industrial settings. At its core, the system uses a voxel carving approach to turn simple 2D sketches into 3D volumes. The focus was on ease of use, ergonomics, and compatibility with the constraints of chemical plants.  

### Key Design Choices
- **Tablet with pen input:** Unlike head-mounted displays, a tablet does not block peripheral vision, which is important for safety. The pen input is comfortable, works even with gloves or dirty hands, and feels intuitive for sketching.
- **Visual–inertial odometry tracking:** The tablet localizes itself relative to printed markers in the plant, providing a reliable 6DoF pose without requiring expensive LiDAR systems.
- **Silhouette-based interaction:** Instead of picking points in cluttered 3D space, users simply trace the outline of a component on photos they capture.
- **Voxel carving algorithm:** The system intersects multiple 2D silhouettes taken from different viewpoints to build a 3D volume.
- **Adaptive octree subdivision:** Large objects are represented with coarse volumes, while small or detailed parts are captured more finely.
- **Marching Cubes mesh generation:** Produces a real-time 3D preview and exportable meshes (e.g. glTF format).

### Workflow
The application guides the user through a simple step-by-step process:
1. **Initialization:** The user scans a marker to localize the tablet within the plant.
2. **Image capture:** The user takes photos of the target component from several angles.
3. **Silhouette drawing:** The component is roughly outlined on each image using the pen.
4. **Volume generation:** The system combines the silhouettes to compute a 3D volume.
5. **Review and refine:** The user previews the reconstructed mesh in AR and can improve it by adding more photos or re-drawing outlines.
6. **Export:** The final annotated VoI is stored and can be integrated into CAD or digital twin systems. 

## Findings
We evaluated the system through a **user study with 15 engineering students** in a laboratory facility equipped with chemical-plant-like structures.

### Quantitative Results
- **Usability (SUS):** Participants rated the system highly, with a mean score of 82.8 (“Excellent”).
- **Workload (NASA TLX):** Reported workload was low (mean 18.5/100), although several participants noted the device felt heavy during extended use.
- **Learning effect:** Task completion time improved noticeably, from an average of 135 seconds on the first attempt to 102 seconds by the fourth.
- **Consistency:** Results did not significantly vary by prior AR experience or academic background, suggesting the system is accessible to first-time users.

### Qualitative Feedback
- Participants found the **annotation process intuitive and effective**, even without prior AR training.
- The **3D previews** were appreciated as a way to check annotation quality before finalizing.
- The **main limitation** was the tablet’s size and weight. Users with smaller hand spans in particular suggested that a smartphone version could be more ergonomic.

### Expert Feedback
Industry engineers also saw potential for additional use cases, such as:
- Documenting **temporary modifications** like sensor placement.
- Enabling **guided scanning** of specific sub-areas to update digital twins.
- Providing clearer **component selection** compared to pointer-based methods.

Together, these findings suggest that intuitive AR-based volume of interest annotation via voxel carving is both feasible and valuable for maintaining accurate digital twins in industrial settings.

## Application
This AR interaction pipeline demonstrates how digital twins can move beyond static models and become active tools in day-to-day industrial work. By making annotation simple and intuitive, workers are directly involved in keeping twins accurate and up-to-date.

In practice, this opens up several concrete applications:
- **Shift communication:** Instead of vague text notes, workers can directly mark anomalies in space so their colleagues know exactly what to look for.
- **Maintenance & inspection:** Selected VoIs streamline planning and guide technicians toward the right components, saving time and reducing errors.
- **Digital twin updates:** Marked volumes define scan regions, enabling targeted 3D scanning and annotation by mobile robots equipped with cameras.
- **Documentation:** Temporary sensors, replaced parts, or modifications can be recorded in context, creating a transparent history of changes.

### Future Directions
Looking ahead, this pipeline could become part of a broader ecosystem for industrial robotics and digital twins:

**Opportunities for expansion**
- Integration into **immersive digital twin interfaces** for industrial facilities, enabling collaboration between human operators and autonomous systems.
- **Guided navigation and safety monitoring** to reduce risks when workers or robots operate in constrained spaces.

**Areas for improvement**
- Exploration of **lighter, more ergonomic devices** such as smartphones or certified wearables for use in hazardous areas.
- Support for **complex geometries**, including components with cavities or openings.

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

## Links
[IEEE ISMAR 2023 Paper](https://ieeexplore.ieee.org/document/10308192)