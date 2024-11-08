---
description: A list of definitions for common terms used across this documentation.
---

# Glossary

**Bold** **words** are terms, followed by their definitions. If a **Bold word (has text in parentheses)**, the text in parentheses is another way to refer to the same thing.&#x20;

Note that any blue text snippets can be clicked to take you to another page with more detailed information about that topic.

## General Terminology

* **Voxel**: A pixel, but in 3D instead of 2D.
* **Volumetric**: Data that exists in a 3D state. For example, Volumetric Lighting is lighting that is computed not just for every pixel on screen (2D), but also for various points in between the camera and that pixel's position (3D). &#x20;
* [**Distance Field**](broken-reference): A 3D dataset in which the value in each voxel represents the distance to the nearest surface. At the surface, the distance is zero. Inside the surface, the distance is negative. Distance fields are often approximate, not perfect, and this can cause artifacts.
* **Triplanar**: A texturing technique where rather than using mesh UVs, one can draw a texture onto a mesh by projecting (think of a projector 'projecting' on a wall) along each cardinal axis. Blending the three projections (X, Y and Z) together creates a seamless texture across objects, without requiring UVs of any sort.&#x20;
* **Heightmap (Heightfield)**: A 2D texture which represents a landscape, with each pixel's brightness being the terrain height for that XY point.
* **Weightmap**: A 2D texture where each pixel represents the intensity at which something, often foliage or a material, should appear for that XY position.
* **Chunk**: When computing data, the world is usually divided into blocks, which are referred to as chunks. For some usecases these chunks are the same size across the world, and for others their size (and resolution) changes based on distance.&#x20;
* **Recursive**: When something repeats within itself, i.e. when a graph calls itself again.
* **Instance**: An object with another object as parent. It shares its core logic, but parameters from the parent can be changed. An example of this are material instances in standard Unreal. Voxel Plugin has support for instances on its Voxel Graphs and on its Material Definitions.

## Voxel Plugin Terminology

* [**Voxel Graph**](../knowledgebase-1/using-graphs/): Voxel Plugin's graph system. It is used for everything from procedural terrains, to foliage scattering, to sculpting tools. It is not the same graph system as Blueprints.
* [**Channel**](broken-reference)**:** Used to communicate between graphs. A channel holds a list of references to nodes. Each node in the list has a user-assigned priority and bounds. A channel does not cache any data.
  * **Query Channel (Read)**: A channel can be queried by calling the **Query Channel** node. This will run the most relevant node (based on priority and bounds) in the channel's list of nodes.
  * **Register to Channel (Write)**: Nodes can be added to a channel's list by calling the **Register to Channel** node.&#x20;
  * **Dependencies**: A channel dependency is a technical term for a system keeping track of state-changes that might happen on a channel-registered node, for instance, voxel actor positions changing.&#x20;
* [**(Landmass) Brushes**](broken-reference): This does not refer to a specific plugin system. It is just a term for a graph that can be placed in the world, non-destructively modifying the terrain (or foliage).
  * **Non-destructive**: A term used to describe that a brush does not 'destroy' the terrain it affects. This is means it can be moved, changed or removed after having been added, without impacting the original terrain underneath.
* [**Surface**](broken-reference): A type used in Voxel Graphs which holds a distance field, bounds, material information and potentially other attributes. Using surfaces allows for the easy combining of two terrains, as the distance field as well as all its attributes can be merged in one go.&#x20;
* [**Material Definition**](broken-reference): Similar in purpose to Unreal Engine's Lanscape Layers. A template with configurable parameters which is used to create different surface types for use in voxel graphs. It interacts with materials through a handful of voxel-specific material nodes. Materials need to be specifically designed for this workflow.
* [**Distance Checks**](broken-reference): An optimization step where chunks check whether the distance field value in their corners is larger than the size of the chunk. If yes, that implies that there cannot be a surface in the chunk, and its further computation is skipped. This can cause holes when the distance field is not perfect.
