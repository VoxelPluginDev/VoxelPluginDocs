---
description: What surfaces are, and how to work with them.
---

# Working with Surfaces

A surface defines a shape along with its properties, like materials. For example, a mesh can be sampled and converted into data used in terrain generation - the mesh&#x20;

At its most basic, a surface just holds a set of bounds (infinite by default) within which it exists, and some information per voxel within those bounds. It always holds a distance field (more on this in the child article), as well as information on what weights the various terrain materials have. By default, it will not have any material data applied.

Generally, surfaces are created by special nodes. For example, the **Create Voxelized Mesh Surface** node, which allows you to turn a static mesh asset into a voxel surface. \
Nodes like this will output a surface with correct bounds and a coherent distance field.

Surfaces are finally used to generate an output mesh using nodes like the **Generate Marchig Cubes Surface** one. Nanite is not currently supported for terrain rendering.
