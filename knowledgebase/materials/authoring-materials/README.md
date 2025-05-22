---
description: An outline on how the material rendering pipeline functions in Voxel Plugin 2.
---

# Authoring Materials

{% hint style="warning" %}
**Nanite Tesselation is disabled by default.**&#x20;

Add the following lines to your project's DefaultEngine.ini to enable it:

* `r.Nanite.AllowTessellation=1`
* `r.Nanite.Tessellation=1`
{% endhint %}

{% hint style="info" %}
There is an [example ](../../../getting-started/installing-voxel-content.md)available on this topic!
{% endhint %}

When materials are assigned to the voxel world from a stamp, they are automatically tracked by the MegaMaterial, and are then combined into a single material.

Because many materials are being combined, your terrain material may not render. Errors related to SRV and Sampler limits may appear. If you see errors like this, they can potentially be worked around by enabling Bindless Rendering.&#x20;

Bindless Rendering can be enabled by adding the following lines to your project's DefaultEngine.ini, under the rendering section:

* `rhi.Bindless.Resources=Enabled`
* `rhi.Bindless.Samplers=Enabled`

{% hint style="danger" %}
Bindless Rendering is in most contexts beneficial or neutral, but it is not well-tested and it is **only supported on SM6**-compatible systems.
{% endhint %}

Whatever is plugged into a material's  `Displacement` pin is treated as height information for heightblends. It is also used for Nanite displacement.&#x20;

{% hint style="info" %}
Rendering **translucent** materials on a voxel world is not supported.

If a translucent material is assigned, those voxels will be entirely invisible in the final mesh.
{% endhint %}
