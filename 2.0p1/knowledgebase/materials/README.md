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
There is an [example ](../../getting-started/installing-voxel-content.md)available on this topic!
{% endhint %}

## A Simple Material

{% hint style="info" %}
This section is just a quick illustration of how the materials 'communicate' - more technical details can be found in [the next section](./#how-materials-are-drawn).&#x20;
{% endhint %}

The stamp in the level has a flat color material assigned, with a `Vector Parameter` named Color. This material is rendered on the terrain directly because Nanite is enabled.

<figure><img src="../../.gitbook/assets/image (221).png" alt=""><figcaption></figcaption></figure>

The Voxel World has a MegaMaterial assigned, which in turn has a Base Material assigned:

<figure><img src="../../.gitbook/assets/image (218).png" alt=""><figcaption></figcaption></figure>

The BaseMaterial has a `Voxel Vector Parameter` in it, also named Color. In this case, it's multiplied by another color so it's easy to see in what contexts the BaseMaterial is used.

This voxel parameter will make the voxel terrain look for materials assigned to it that have non-voxel parameters with the same name.&#x20;

<figure><img src="../../.gitbook/assets/image (220).png" alt=""><figcaption></figcaption></figure>

If Nanite is disabled on the Voxel World, the terrain will be rendered with the MegaMaterial's Base Material instead.&#x20;

<figure><img src="../../.gitbook/assets/image (222).png" alt=""><figcaption><p>The terrain's color changed with Nanite disabled, because the Color parameter is multiplied by a reddish tint in the Base Material.</p></figcaption></figure>

The Base Material is used to render the terrain whenever Nanite rendering is not available. It's used when Nanite is simply disabled in the project or on the Voxel World, but it's also used for things like rendering the Lumen Surface Cache.&#x20;

