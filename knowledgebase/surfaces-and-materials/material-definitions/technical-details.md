---
description: >-
  Key technical considerations for the Material Definition workflow and the use
  of Detail Textures.
---

# Technical Details

## 1. Material Definitions

The Material Definition workflow allows users to easily create and handle layered terrain materials, but it does have compromises. The biggest drawback is that this workflow relies heavily on Texture  Arrays, which don't allow for granular texture streaming. Additionally, due to performance considerations, only three layers can be applied at a given position.&#x20;

### &#x20;  1.1 Layer Blending

When generating graph outputs, the plugin will keep track of all layer weights assigned throughout the graph. When outputting the final mesh, the index and weights of the three most prominent layers for each voxel will be written into a texture. This data can then be read in the material through the **GetVoxelMaterialID** node.

The three layer limit does mean that seams can occur if more than three materials meet at a given point. From internal experience and testing, 4+ layer intersections are rare in practice. Additionally, the seams aren't usually very visible due to the low weight of the discarded layers.

### &#x20;  1.2 Texture Arrays

The use of texture arrays allows for incredibly efficient sampling of a very high amount of terrain layers per chunk. Unlike with Landscape materials and Voxel Plugin 1.2's Multi-Index materials, there is no theoretical limit on how many layers can be present in one chunk.&#x20;

#### &#x20;     1.2.1 Texture Streaming

The key drawback of using texture arrays is that texture streaming will treat the entire array as a single object. This means that if an array has ten textures, but only one is shown on screen, all ten textures will still be loaded into VRAM. This means that as layer counts increase, VRAM budgets can quickly explode.

Memory use increases exponentially with texture size (doubling the resolution will quadruple memory use). For the memory overhead of a single 4K texture, sixteen 1K textures could be resident. Considering this, we recommend keeping terrain textures as low resolution as possible. Use 1K textures, or 2K if absolutely necessary. If more surface detail is needed, add a basic detail overlay in the material.

{% hint style="info" %}
To work around the streaming limitation, we intend to add a two-step array system. The plugin would create two texture arrays; one array containing every single layer at a low resolution, and another array containing only textures used in nearby chunks, but at high resolutions.&#x20;
{% endhint %}

##
