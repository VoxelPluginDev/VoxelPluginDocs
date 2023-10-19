---
description: >-
  Material Definitions are similar to Unreal's Landscape Layers, but for
  voxel-based terrains.
---

# Material Definitions

Material Definitions are at the core of the material workflow on Voxel Plugin terrains. \
\
With this workflow, it is possible to use hundreds of material layers on the terrain. Within voxel graphs, you can assign different texture layers with a weight. When the graph generates a mesh, the three highest-weighted layers for each position will be sent to the material. Anything other than the three highest-weight layers being ignored.&#x20;

The shader logic run for these layers is defined in a single material which uses some voxel helper nodes to get per-layer information. Because of this, every layer is forced to run the same logic. This means that a snow layer will have to use the same instructions as a rock layer - it can only change the float and textures values used.

{% hint style="warning" %}
Landscape materials found on the Unreal Engine Marketplace cannot generally be used well with voxel terrains. These materials are built specifically for native Landscapes, which use their own specialized layer-blending nodes.&#x20;

These nodes are incompatible with Voxel Plugin, so Landscape layers will not function. Additionally, most of these materials are not triplanar, leading to texture stretching on slopes.

We will be working with creators to create Voxel Plugin compatible versions of asset packs.&#x20;
{% endhint %}

More detailed information about the inner workings of this system can be found on the [technical-details.md](technical-details.md "mention") and [detail-textures.md](../detail-textures.md "mention") pages.
