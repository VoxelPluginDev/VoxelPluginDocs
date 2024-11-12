---
description: An outline on how the material rendering pipeline functions in Voxel Plugin 2.
---

# Materials

{% hint style="warning" %}
**Nanite Tesselation is disabled by default.**&#x20;

Add the following lines to your project's DefaultEngine.ini to enable it:

* `r.Nanite.AllowTessellation=1`
* `r.Nanite.Tessellation=1`
{% endhint %}

{% hint style="info" %}
There is an [example ](../getting-started/installing-voxel-content.md)available on this topic!
{% endhint %}

## A Simple Material

{% hint style="info" %}
This section is just a quick illustration of how the materials 'communicate' - more technical details can be found in [the next section](materials.md#how-materials-are-drawn).&#x20;
{% endhint %}

The stamp in the level has a flat color material assigned, with a `Vector Parameter` named Color. This material is rendered on the terrain directly because Nanite is enabled.

<figure><img src="../.gitbook/assets/image (221).png" alt=""><figcaption></figcaption></figure>

The Voxel World has a MegaMaterial assigned, which in turn has a Base Material assigned:

<figure><img src="../.gitbook/assets/image (218).png" alt=""><figcaption></figcaption></figure>

The BaseMaterial has a `Voxel Vector Parameter` in it, also named Color. In this case, it's multiplied by another color so it's easy to see in what contexts the BaseMaterial is used.

This voxel parameter will make the voxel terrain look for materials assigned to it that have non-voxel parameters with the same name.&#x20;

<figure><img src="../.gitbook/assets/image (220).png" alt=""><figcaption></figcaption></figure>

If Nanite is disabled on the Voxel World, the terrain will be rendered with the MegaMaterial's Base Material instead.&#x20;

<figure><img src="../.gitbook/assets/image (222).png" alt=""><figcaption><p>The terrain's color changed with Nanite disabled, because the Color parameter is multiplied by a reddish tint in the Base Material.</p></figcaption></figure>

The Base Material is used to render the terrain whenever Nanite rendering is not available. It's used when Nanite is simply disabled in the project or on the Voxel World, but it's also used for things like rendering the Lumen Surface Cache.&#x20;

## How Materials Are Drawn

The current solution to material rendering has two components:

* Pixel Shader Material
  * This is used for:
    * Shading terrains with Nanite enabled.
  * Any (opaque) material can be drawn anywhere.&#x20;
  * One material is drawn per pixel - soft layer blends are not possible.
    * This uses Nanite's standard shading pipeline.
    * Which material is drawn where is affected by the Voxel Displacement configured in the Base Material.
* Base Material
  * This is used for:
    * Controlling Voxel Displacement. This affects Pixel Shader blending and displacement.
    * Shading non-Nanite terrains.
    * Rendering the Lumen Surface Cache.
  * &#x20;The same logic has to be used for the entire terrain.
    * This means that non-Nanite rendering and, more importantly, height blending and displacement, do not automatically match the Pixel Shader Material.&#x20;

#### Texture Arrays & Texture Builder

As outlined above, the Base Material cannot use an "any material anywhere" approach. Instead, it uses a more traditional multi-layer setup.&#x20;

To make it possible to access textures and properties assigned on terrain layers in the base material, the plugin can automatically gather texture and float-based parameters from any Pixel Shader Materials used.

{% hint style="info" %}
`Voxel Texture Parameters` work the same as UE's `Texture Object Parameters`. They can be used in Material Functions by having the Parameter be separate from the Sample node.
{% endhint %}

Any Voxel Parameters in the Base Material will collect Parameters with the same name from Pixel Shader Materials. Under the hood, scalar and vector parameters are written into textures. Texture parameters are compiled into a texture array. Note that the use of texture arrays means all textures being collected need to have the same compression format. This follows the setting of the texture assigned on the Voxel Parameter node as default.

## Smooth (Alpha) Layer Blends

{% hint style="warning" %}
Nanite must be disabled to use Smooth Blends. Displacement is not supported.
{% endhint %}

To implement alpha blends, Nanite needs to be disabled on the Voxel World. This will force the terrain to render with the MegaMaterial's base material.

In the MegaMaterial's base material, the `Voxel Fallback Weights` can be used to directly interface with the current pixel's layer IDs and weights.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>The <code>Voxel Fallback Weights</code> node returns the current pixel's layer data.</p></figcaption></figure>

Every Voxel Parameter node has a `Layer` input pin. To set up alpha blending, the material logic needs to be copy-pasted three times, with each copy using one of the layers. At the end of each layer's logic, multiply the values by that layer's weight, and then add the three layers' values together.

<div>

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Three layers blended together.</p></figcaption></figure>

 

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

</div>

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>The above logic used on the Nanite Materials example.</p></figcaption></figure>
