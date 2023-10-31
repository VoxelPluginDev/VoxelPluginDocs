---
description: >-
  Building a custom material is complicated, but the Complex Material template
  doesn't fit every use case.
---

# Creating Materials from Scratch

{% hint style="danger" %}
This is an advanced workflow. [extending-the-complex-material-sample.md](extending-the-complex-material-sample.md "mention") is preferred.
{% endhint %}

## 1. Working with Material Definitions

{% hint style="success" %}
For a straight-forward example, download and explore the BasicMaterial sample.
{% endhint %}

A material definition, or an instance of one, is an asset that represents a texture layer on the terrain. It defines a set of parameters to be used, generally float data or textures, and a material that reads the parameters for the asset's preview. These parameters will then be sent through to the material being used on the terrain as needed.

Material Definition Instances use a Material Definition as template. These are similar to material instances in Unreal Engine. They inherit from their parent, and can override the values assigned to the parameters. Each customized MDI can then be used as a layer on the terrain.

{% hint style="info" %}
Material definitions and instances are similar to Multi-Index Layers from Voxel Plugin 1.2.
{% endhint %}

A material definition's preview material can be configured in the Details panel when no parameters are selected. This material will only be used for previewing the definition and its instances in editor viewports. It should usually be the same material as the terrain has assigned.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption><p>A material definition on the left, with an instance using it as parent on the right.</p></figcaption></figure>

### &#x20;  1.1 Configuring Parameters

Float-based parameter types are straight-forward to use; just click the plus button, set the name and type, and then set a default value. No other configuration is needed.

{% hint style="info" %}
Material and material definitions like the one shown throughout this article are available for download through the [Voxel Content menu](../../../getting-started/installing-voxel-content.md).&#x20;
{% endhint %}

Texture parameters have a few extra configuration settings that can be important to consider, which can be found in the Value dropdown underneath the Default Value. Most Material Definitions will have quite a few texture parameters configured, and these settings will vary across them.

<figure><img src="../../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption><p>A Material Definition with a Texture2D parameter selected. Note the additional settings underneath the Default Value.</p></figcaption></figure>

#### &#x20;     1.1.1 Texture and Mip Size

Texture Size and Last Mip Texture Size affect the VRAM (GPU memory) overhead of the texture in the final material.&#x20;

We recommend keeping texture size as low as possible to minimize VRAM use (A single 4k texture takes up as much space as sixteen 1k textures). If more detail is needed, consider adding it with another texture overlay in the material graph.

The Last Mip Texture Size value should generally be left at its default, unless you understand the impacts of MipMaps on a technical level.&#x20;

#### &#x20;     1.1.2 Texture Compression Settings

The Compression type is the most important of these three values. If it is configured incorrectly, instances will throw errors when assigning textures, and your materials might not compile. Currently, the following compression types are available:

* **DXT1**
  * Colour textures without alpha channel.
* **DXT5**
  * Colour textures with alpha channel. Twice as much memory use.
* **BC5**
  * Used exclusively for normal maps.

The compression type picked here has to match whichever compression method is selected in a texture asset's texture settings. If there is a mismatch, the plugin will throw errors.

#### &#x20;     1.1.3 Using Alpha Channels

Because DXT5 (colour with alpha) uses twice as much memory as DXT1 (colour without alpha), DXT1 should be used for colour textures wherever possible.&#x20;

Attempting to assign a texture asset with an alpha channel when DXT1 compression is selected on the material definition will cause errors. In these cases, the texture asset can be easily swapped to be without alpha by ticking the "Compress Without Alpha" tickbox in the asset's compression settings.

If the alpha channel is absolutely needed, the material definition's compression setting can be set to DXT5. Unlike the other way around, color textures without alpha can also be assigned without problems. Be very mindful that this will double the VRAM usage for **all** textures assigned on this parameter, even if most of those textures do not have an alpha channel.

## 2. Configuring a Custom Material

With a material definition configured as above, instances can be created and configured, and they can be[ assigned to surfaces](../working-with-surfaces/applying-materials-to-a-surface.md) in a Voxel Graph. A material definition by itself won't make a material appear on the terrain though; for this, an accompanying master material needs to be created.

### &#x20;  2.1 Reading Data from Material Definitions

<figure><img src="../../../.gitbook/assets/image (146).png" alt=""><figcaption><p>A material graph with the Material Definition workflow nodes placed, and a Voxel Parameter node selected.</p></figcaption></figure>

Data from a Material Definition and its instances can be accessed in materials using a handful of helper nodes. To do so, a combination of a **Voxel Parameter** node and a **Get Voxel ... Parameter** node is used.

The **Voxel Parameter** node can be configured in the details panel when selected; it takes a reference to a Material Definition, and will then show a dropdown with all the parameters from that Material Definition. The output from this node can then be plugged into the Parameter pin on a **Get Voxel ... Parameter**. Use the appropriate Get node for the type the parameter has in the Material Definition. For example, if a parameter is a Linear Color, use the **Get Voxel Color Parameter** node.

### &#x20;  2.2 How Layers Blend

The **Get Voxel ... Parameter** nodes can be used without plugging anything into the MaterialId pin. When used like this, they will do a lerp (linear blend) between the top three layers' value for this parameter.

If a standard linear blend isn't enough, the blend can be done manually by using the MaterialId pin. The relevant MaterialIds and their weights can be retrieved with the **Get Voxel Material ID** node. In this context, the Layer0/Layer1/Layer2 pins are the indexes of the three highest-weighted layers, and the LerpAlphas are the relevant weights between them.&#x20;

<figure><img src="../../../.gitbook/assets/image (148).png" alt=""><figcaption><p>The "Get Voxel Parameter" node is actually three getters blended together. This can also be done manually for full control.</p></figcaption></figure>

