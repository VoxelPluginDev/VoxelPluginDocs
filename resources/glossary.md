---
description: A list of definitions for common terms used across this documentation.
---

# Glossary

**Bold** **words** are terms, followed by their definitions. If a **Bold word (has text in parentheses)**, the text in parentheses is another way to refer to the same thing.&#x20;

Note that any blue text snippets can be clicked to take you to another page with more detailed information about that topic.

## General Terminology

* **Voxel**: Like a pixel, but in 3D instead of 2D.
* **Volumetric**: Data that exists in a 3D state. For example, Volumetric Lighting is lighting that is computed not just for every pixel on screen (2D), but also for various points in between the camera and that pixel's position (3D). &#x20;
* **Distance (Density) Field**: A 3D dataset in which the value in each voxel represents the distance to the nearest surface. At the surface, the distance is zero. Inside the surface, the distance is negative. Distance fields are often approximate, not perfect, and this can cause artifacts.
* **Triplanar**: A texturing technique where rather than using mesh UVs, one can draw a texture onto a mesh by projecting (think of a projector 'projecting' on a wall) along each cardinal axis. Blending the three projections (X, Y and Z) together creates a seamless texture across objects, without requiring UVs of any sort.&#x20;
* **Heightmap (Heightfield)**: A 2D texture which represents a landscape, with each pixel's brightness being the terrain height for that XY point.
* **Weightmap**: A 2D texture where each pixel represents the intensity at which something, often foliage or a material, should appear for that XY position.
* **Chunk**: When computing data, the world is usually divided into blocks, which are referred to as chunks. For some usecases these chunks are the same size across the world, and for others their size (and resolution) changes based on distance.&#x20;
* **Recursive**: When something repeats within itself, i.e. when a graph calls itself again.
* **Instance**: An object with another object as parent. It shares its core logic, but parameters from the parent can be changed. An example of this are material instances in standard Unreal. Voxel Plugin has support for instances on its Voxel Graphs.

## Voxel Plugin Terminology

* [**Voxel Graph**](../knowledgebase/using-graphs/): Voxel Plugin's graph system. It is used for everything from procedural terrains, to foliage scattering, to sculpting tools. It is not the same graph system as Blueprints.
* **Non-destructive**: A term used to describe that a stamp does not 'destroy' the terrain it affects. This means it can be moved, changed or removed after having been added, without impacting the original terrain underneath.
* **Distance Checks**: An optimization step where chunks check whether the distance field value in their corners is larger than the size of the chunk. If yes, that implies that there cannot be a surface in the chunk, and its further computation is skipped. This can cause holes when the distance field is not perfect.
