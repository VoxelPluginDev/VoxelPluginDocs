---
description: Outlining how Voxel Plugin materials work on the technical side.
---

# Technical Overview

## How Materials Are Drawn

The current solution to material rendering has two components:

* Pixel Shader Material
  * This is used for:
    * Shading terrains with Nanite enabled.
  * Any (opaque) material can be drawn anywhere.&#x20;
  * One material is drawn per pixel - soft layer blends are not possible.
    * This uses Nanite's standard shading pipeline.
    * Which material is drawn where is affected by the Voxel Displacement configured in the Base Material.
* MegaMaterial's Base Material
  * This is used for:
    * Controlling Voxel Displacement. This affects Pixel Shader blending and displacement.
    * Shading non-Nanite terrains.
    * Rendering the Lumen Surface Cache.
  * &#x20;The same logic has to be used for the entire terrain.
    * This means that non-Nanite rendering and, more importantly, height blending and displacement, do not automatically match the Pixel Shader Material.&#x20;
  * Can optionally use [manual blend logic](smooth-alpha-blends.md).

#### Texture Arrays & Texture Builder

As outlined above, the Base Material cannot use an "any material anywhere" approach. Instead, it uses a more traditional multi-layer setup.&#x20;

To make it possible to access textures and properties assigned on terrain layers in the base material, the plugin can automatically gather texture and float-based parameters from any Pixel Shader Materials used.

{% hint style="info" %}
`Voxel Texture Parameters` work the same as UE's `Texture Object Parameters`. They can be used in Material Functions by having the Parameter be separate from the Sample node.
{% endhint %}

Any Voxel Parameters in the Base Material will collect Parameters with the same name from Pixel Shader Materials. Under the hood, scalar and vector parameters are written into textures. Texture parameters are compiled into a texture array. Note that the use of texture arrays means all textures being collected need to have the same compression format. This follows the setting of the texture assigned on the Voxel Parameter node as default.
