---
description: >-
  Material Definitions are similar to Unreal's Landscape Layers, but for
  voxel-based terrains.
---

# Material Definitions

Material Definitions are at the core of the material workflow on Voxel Plugin terrains. \
\
With this workflow, it is possible to use hundreds of material layers on the terrain. A maximum of three layers will be shown at a given position, with anything other than the three highest-weight layers being ignored. (point to detail tex for technical explanation).

With this system, all layers will run the same logic. It is impossible to have certain layers run different material logic than others. A snow layer will have to use the same instructions as a rock layer - it can only change the float and textures values used.&#x20;

{% hint style="warning" %}
Landscape materials found on the Unreal Engine Marketplace cannot generally be used well with voxel terrains. These materials are built specifically for native Landscapes, which use their own specialized layer-blending nodes.&#x20;

These nodes are incompatible with Voxel Plugin, so Landscape layers will not function. Additionally, most of these materials are not triplanar, leading to texture stretching on slopes.

We will be working with creators to create Voxel Plugin compatible versions of asset packs.&#x20;
{% endhint %}
