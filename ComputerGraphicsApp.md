---
layout: default
title: Computer Graphics Project
description: Simulates an immersive rocky ocean environment in which a player controls a ship capable of destroying procedural terrain.
permalink: /ComputerGraphicsApp.html
---

<link rel="stylesheet" href="site.css">

<script src="https://unpkg.com/lucide@latest"></script>
<script>
  document.addEventListener("DOMContentLoaded", () => {
    lucide.createIcons();
  });
</script>
<p>
  <svg data-lucide="cpu" class="icon"></svg>  DX11
  <svg data-lucide="user-cog" class="icon"></svg>  Computer Graphics & Procedural Programmer
</p>



## Water Simulation - Gerstner Waves
*   Implemented a physically-inspired Gerstner Wave model in the domain shader to deform a tessellated water plane.
*   Supports multi-layered waves with editable parameters: steepness, wavelength, speed, and direction.
*   Accurate vertex normal reconstruction was done using derived tangents and bitangents per wave layer to ensure proper lighting and shading.
 <img src="./CG_GerstnerWaveControl.gif" alt="Terrain">
 <img src="https://s14.gifyu.com/images/bNMhY.gif" alt="Terrain">


## Parallel Split Shadow Maps (PSSM)
*   Integrated Parallel Split Shadow Mapping (PSSM) for high-quality directional shadows with 3 cascade levels (near, mid, far).
*   Each cascade is calculated using frustum slicing and orthographic projection aligned with the light direction.
*   Light positions and properties fully configurable at runtime via GUI.
 <img src="./CG_ShadowMaps.gif" alt="Terrain">

## Compute-shader Buoyancy 

*   The boat is physically represented by a rectangular Bullet3D body synced with the visible mesh.
*   A grid of points is sampled across the bottom hull surface each frame.
*   A compute shader evaluates each point using gradient descent to find the corresponding wave height (Gerstner waves require solving for vertical displacement, as they map 3D inputs to 3D outputs).
*   The vertical distance between each hull point and wave surface is used to estimate displaced fluid volume and generate upward buoyant forces.


 <img src="./CG_CSBuoyancy.gif" alt="Terrain">


## Screen-Space Reflection (SSR)
Realistic water surface reflections were achieved using a screen-space ray marching algorithm:
*   Ray-marching in screen space with Digital Differential Analyzer (DDA) stepping to prevent over-sampling.
*   Fallback to a cubemap-based skybox when rays leave the view frustum, avoiding visual artifacts at screen edges.
*   Exposed fidelity controls (max steps, step size, reflection opacity) via ImGui interface for real-time tweaking and performance balancing.

 <img src="./CG_ScreenSpaceReflections.gif" alt="Terrain">

## Procedural Terrain With Procedural Destruction
#### Terrain Generation
*   Used fractal Brownian motion (fBM) to procedurally generate rocky peaks emerging from the ocean.
*   GPU terrain is recalculated every frame using dynamic tessellation and vertex displacement.
*   A parallel CPU mesh is generated once and partially recalculated only when geometry is altered (e.g., via cannon fire), enabling accurate ray-triangle intersections and collisions.

<div class="project-showcase">
  <div class="media">
 <img src="./CG_Terrain.png" alt="Terrain">
 </div>
  <div class="media">
 <img src="./CG_TerrainNormals.png" alt="Terrain Normals">
 </div>
 </div>
 <img src="./CG_Tesselation.png" alt="Terrain Normals">

#### Destruction System
*   When terrain is hit by a cannon, the affected mesh region is extracted and converted into a destructible fragment set.
*   A sequence of randomly positioned and oriented splitting planes intersect the geometry and fragment it into jagged, rock-like chunks.
*   Each internal face is automatically inverted and retextured to distinguish the interior from the original surface.

<div class="project-showcase">
  <div class="media">
 <img src="./CG_ProceduralDestruction1.png" alt="Terrain">
 </div>
  <div class="media">
 <img src="./CG_ProceduralDestruction2.png" alt="Terrain">
 </div>
 </div>

  



## Retro Underwater Effect 
Achieved by combining various post-processing techniques:
*   Edge Blur: Vignette-based Gaussian blur applied only to screen edges using dynamic radial masks.
*   Water Distortion: Ripple effects applied via trigonometric distortion of UVs in the pixel shader.
*   Color Tinting & Magnification: Simulated depth distortion with a center-screen magnifier using aspect-corrected circular masks.
 <img src="./CG_RetroWaterDistortion.gif" alt="Terrain">

<!--
## Dynamic Tesselation
*   Edge-based tesselation adjusts the subdivision level based on distance to the camera, optimizing detail vs. performance.

## Material Shading - Displacement + Normal Mapping
Rocks and props utilize displacement mapping driven by grayscale height maps, combined with normal mapping for high detail on low-poly meshes.
-->








[back](./)
