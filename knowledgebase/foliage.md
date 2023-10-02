---
description: Generating foliage onto an existing surface.
---

# Foliage

{% hint style="info" %}
The [downloadable Voxel Content](../getting-started/installing-voxel-content.md) includes a fully featured foliage cluster generator.&#x20;
{% endhint %}

As of 2.0p-340, foliage uses a point-based workflow, drawing inspiration from Unreal Engine's PCG, with a focus on high-performance bulk compute. The result is a powerful and flexible system, without needing to rely on inherently expensive techniques.

{% hint style="warning" %}
This article assumes basic knowledge about [Channels ](channels.md)and [Surfaces](surfaces-and-materials/).
{% endhint %}

## 1. Generating starting points

Before we can do anything meaningful with our foliage, we need to create a set of points to start working from. There are a couple of approaches we can take for this. The two primary ones are to ~~_**simply scatter points on a grid to adjust afterwards**_~~_** (not yet implemented)**_, or to to spawn points directly from a volumetric surface, the latter generally being significantly more costly.

### &#x20;  1.1 Scattering on a Volumetric Surface

In order to spawn points from the volumetric terrain, we use the we use **Generate Surface Points** node. Cell Size represents the distance between each point, the Surface is what we sample to scatter the points on, and the Bounds are the area you want to scatter points in. Generally, the Bounds will be Spawnable Bounds, which are passed down by a Chunk Spawner, the execution node generally used with foliage.

<figure><img src="../.gitbook/assets/image (132).png" alt=""><figcaption><p>An example configuration of how to use the Generate Surface Points node.</p></figcaption></figure>

When generating points on a surface, we need to first compute the specified surface at a resolution of the specified Cell Size. This means that if the Cell Size is set to 100, it has the same cost as generating the terrain at a Voxel Size of 100 for the entire area the foliage is spawned in. Because it's volumetric, cost increases are cubic, so going from 100 to 50 will be eight (2³) times as expensive to compute.&#x20;

It is important to bear in mind that a high cell size value will cause any shapes in the terrain that are smaller than it to no longer have points spawned on them, as they are too small to show up at that resolution.

#### &#x20;     1.1.1 Adjusting Points using Raymarch Distance Field

Points generated at a cell size which is higher than the final terrain's voxel size will not align with the terrain perfectly, but this can be fixed with the **Raymarch Distance Field** node.&#x20;

<figure><img src="../.gitbook/assets/image (129).png" alt=""><figcaption><p>Use the Raymarch Distance Field node to adjust points so they're no longer floating. High step counts are expensive!</p></figcaption></figure>

For a given point, this will sample the surface's distance field at its current location, as well as the surface gradient (slope/normal) and then move the point towards the surface, along the normal. It will repeat this until it reaches Max Steps, or until the distance at its current location is less than the Tolerance. If it reaches Max Steps, but the distance at its location is still more than Kill Distance, the point will be removed. If the point is kept, and Update Normal is true, it will also update the input's Point Normal to match whatever the distance field slope was at that position, aligning it to the surface.

<details>

<summary>  1.2 Scattering on a 2D Grid</summary>

**2D Point Scattering is not yet implemented.**

~~For the reasons mentioned above, scattering directly on a surface, while ideal from a workflow-perspective, can be very costly. A cheaper approach which can be perfectly viable for foliage in most use-cases, is to instead treat the terrain as a heightmap when generating foliage. This way, rather than generating a surface so we can scatter points, we can simply scatter points on a grid along the world's XY plane, and then sample the terrain's height for each point generated.~~&#x20;

~~If the world contains volumetric elements that foliage should not float over or intersect through, there are two options:~~&#x20;

~~Any floating points can simply be removed. The world's final surface channel can be sampled, and points can be removed using a **Density Filter** (detailed below) node if the distance at the point position is more than a given value.~~

~~Alternatively, rather than removing the points entirely, one can Raymarch through the distance field for each point to adjust its location to the final distance field. This is covered in~~ [~~this section above~~](foliage.md#adjusting-points-using-raymarch-distance-field)~~.~~

</details>

## 2. Filtering Points and Randomizing Visuals&#x20;



### &#x20;  2.1 Controlling foliage density with brushes



