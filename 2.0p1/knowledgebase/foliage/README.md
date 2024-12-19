---
description: Generating foliage onto an existing surface.
cover: ../../.gitbook/assets/image (198).png
coverY: 66.19718309859152
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Foliage & PCG

{% hint style="info" %}
There is an [example ](../../getting-started/installing-voxel-content.md)available on this topic!
{% endhint %}

As of this release, native Voxel Plugin foliage has been replaced with a [PCG ](https://dev.epicgames.com/documentation/en-us/unreal-engine/procedural-content-generation-overview)integration. This gives full control over scattering behavior, and allows for spawning actors (including voxel brushes) as well as the usual static meshes.

{% hint style="info" %}
PCG updates from the Voxel World can be paused using the `voxel.DisablePCGRegen 1` console command.
{% endhint %}

With PCG comes the ability to analyze points and do contextual operations on them. This opens the door to things like combining multiple smaller cliff points into a single larger cliff mesh. The logic that would be used for that kind of behavior is generic to any PCG-based system.&#x20;

### Using PCG on Voxel Terrains

<figure><img src="../../.gitbook/assets/image (198).png" alt=""><figcaption><p>A terrain, including its volumetric shapes like a floating brush, being populated with trees, with a spline used as a mask.</p></figcaption></figure>

The key node for anything voxel-related on the PCG side is the `Get Voxel Data` node. This can be plugged into a `Voxel Sampler` to spawn new points, or into a `Voxel Projection` node to make existing points conform to the terrain. Both of these can be configured from the details panel to be height-based or volumetric.

<figure><img src="../../.gitbook/assets/image (197).png" alt=""><figcaption><p>Scattering points on the terrain is done using the Get Voxel Landscape Data and Voxel Landscape Sampler nodes.</p></figcaption></figure>

#### PCG Configuration

When using PCG with Voxel Plugin, we strongly recommend:

* On any PCG graph, setting `Use Hierarchical Generation` to true.
  * Note that PCG's spline sampling does not work well with Hierarchical Generation. Use Voxel Metadata instead.
* On any in-world graph actor:
  * Setting `Generation Trigger` to `Generate at Runtime`.
  * Setting `Is Partitioned` to true.
* On the PCG World Actor:
  * Setting `Treat Editor Viewport as Generation Source` to true.
  * If the world isn't largely a heightfield (i.e. it contains lots of caves, or is a planet), setting `Use 2DGrid` to false.
