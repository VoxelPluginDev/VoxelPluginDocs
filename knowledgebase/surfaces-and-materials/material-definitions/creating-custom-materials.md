---
description: >-
  Materials need to be authored specifically for use with Voxel Plugin using
  Material Definitions.
---

# Creating Custom Materials

Having a graph output a terrain mesh is relatively straight-forward, but in order for the terrain to be approriately textured in different areas, it needs to be set up with an appropriate material. Voxel Plugin support multi-layer terrains through Material Definitions.

Material Definition assets are a template with configurable parameters. hey can be used to create different custom surface types for use in voxel graphs. It interacts with materials through a handful of voxel-specific material nodes.

{% hint style="info" %}
The material and material definitions shown throughout this article are available for download through the [Voxel Content menu](../../../getting-started/installing-voxel-content.md).&#x20;
{% endhint %}

## 1. Working with Material Definitions

A material definition is an asset that represents a type of material on the terrain. It defines a set of parameters to be used, generally float data or textures, and a material that reads the parameters for the asset's preview.

A material definition (MD) can then be used as template for material definition instances (MDI). These are similar to material instances in Unreal Engine. They inherit from their parent, and can override the values assigned to the parameters. Each customized MDI can then be used as a 'layer' on the terrain.

{% hint style="info" %}
Material definitions and instances are similar to Multi-Index Layers from Voxel Plugin 1.2.
{% endhint %}

A material definition's preview material can be configured in the Details panel when no parameters are selected. This material will only be used for previewing the definition and its instances in editor viewports. It's usually the same material as the terrain uses, but it doesn't have to be. &#x20;

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption><p>A material definition on the left, with an instance using it as parent on the left.</p></figcaption></figure>

### &#x20;  1.1 | Configuring Texture Parameters

While most parameter types are very straight-forward - just click the plus button, set the name and type, and then set a default value - texture parameters have a few extra configuration settings that are important to consider. Most Material Definitions will have quite a few texture parameters configured, and these settings will vary across them.

<figure><img src="../../../.gitbook/assets/image (4) (1).png" alt=""><figcaption><p>A Material Definition with a Texture2D parameter selected.</p></figcaption></figure>

Instead of only having a default value, texture parameters have some important things to be configured in the value dropdown underneath.

Texture Size and Last Mip Texture Size affect the VRAM (GPU memory) overhead of the texture in the final material.&#x20;

We recommend keeping texture size as low as possible to minimize VRAM use (A single 4k texture takes up as much space as sixteen 1k textures). If more detail is needed, adding it with another texture overlay in the material graph is preferable to increasing the texture size.

The Last Mip Texture Size value should generally be left at its default, unless you understand the impacts of MipMaps on a technical level.&#x20;

That leaves Texture Compression.

#### &#x20;     1.1.1 | Texture Compression Settings

The Compression type is the most important of these three values. If it is configured incorrectly, instances will throw errors when assigning textures, and your materials might not compile. Currently, the following compression types are available:

* **DXT1**
  * Colour textures without alpha channel.
* **DXT5**
  * Colour textures with alpha channel. Twice as much memory use.
* **BC5**
  * Used exclusively for normal maps.

The compression type picked here has to match whichever compression method is selected in a texture asset's texture settings.&#x20;

#### &#x20;     1.1.2 | Using Alpha Channels

Because DXT5 (colour with alpha) uses twice as much memory as DXT1 (colour without alpha), DXT1 should be used for colour textures wherever possible. Attempting to assign a texture asset with an alpha channel when DXT1 compression is selected on the material definition will cause errors. In these cases, the texture asset can be easily swapped to be without alpha by ticking the "Compress Without Alpha" tickbox in the asset's compression settings.

If the alpha channel is absolutely needed, the material definition's compression setting can be set to DXT5. Unlike the other way around, colour textures without alpha can also be assigned without problems. Be very mindful that this will double the VRAM usage for all textures assigned on this parameter, even if most of those textures do not have an alpha channel. Only use DXT5 when &#x20;

## 2. Configuring a Custom Material

With a material definition configured as above, instances can be created and configured, and they can be[ assigned to surfaces](applying-materials-to-a-surface.md) in a Voxel Graph. Doing this won't achieve anything, though, as long as there isn't a material to use the data.

### &#x20;  2.1 Reading Data from Material Definitions

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption><p>A material graph with the Material Definition workflow nodes placed. </p></figcaption></figure>

The three nodes in the center of the above image are used to access the data being passed by material definitions assigned to the surface. The **Get Voxel Material ID** node is used for advanced materials that require custom blends, for instance when height blending is desired. (Example TBD)

On each of these nodes, there is a Material Definition field and a Parameter Name field.&#x20;

When no Material Definition is assigned, the name field can be directly typed into. The material nodes will then attempt to retrieve data stored by that name on the surface. If nothing is found under the given name, the nodes will simply output zero. Because of the quiet failures with this approach, we generally recommend assigning a Material Definition.

If there is an assigned Material Definition, the Parameter Name field will change to be a dropdown instead. This dropdown will show all parameters that currently exist on the assigned Material Definition, as long as they match the node type (i.e. color, texture). Using this approach has the big benefit that, rather than quietly outputting zero, the material editor will warn you if your nodes are misconfigured.

The **Get Voxel Material Texture Array** node has Default Sampler Type as an additional field. This field functions the same as the sampler type on texture sample nodes in the material editor; if the parameter being sampled is set to normal map compression (BC5), this should be set to Normal. If the parameter being sampled is set to color compression (DXT1/DXT5), this should be set to Color. Like the Parameter Name, this field is only used when no material definition has been assigned. When one is assigned, the node will automatically match the parameter settings.&#x20;

### &#x20;  2.2 | How Layers Blend

The **Get Voxel Material** nodes can be used without plugging anything into the MaterialId pin. When used like this, they will do a lerp (linear blend) between the top three layers' attributes. More complex blending behaviour can be achieved by using the Get Voxel Material ID.&#x20;



