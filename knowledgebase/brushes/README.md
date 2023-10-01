---
description: Using graphs as modular, non-destructive pieces for world-creation.
---

# Brushes

{% hint style="warning" %}
This article assumes basic knowledge about [Channels](../channels.md).
{% endhint %}

Any Voxel Graph can run any logic, and therefore fulfill any function, so there are no predefined graph types. But the way graphs are used in practice often does end up following a few common patterns.&#x20;

One of these patterns is the 'brush' pattern, where a graph represents a moveable object that doesn't do anything other than writing data into a channel, which another graph then reads back and produces a visible output from.

Examples of this (which can be found in the [Voxel Content](../../getting-started/installing-voxel-content.md)) are:

* A graph writes a sphere into a FoliageDensity channel. This channel is queried in a second graph, which outputs foliage, where it is used as a mask. Foliage now no longer generates where the first graph is placed in the world.  &#x20;
* A graph writes a voxelized mesh into a Surface channel. This channel is queried in a second graph which has a **Generate Marching Cube Surface** node, rendering the surface into the world, including the changes made by the first graph.

## 1. Blending Brushes

Brushes are generally blended into existing terrain, rather than being used by themselves. Additionally, most usecases involve multiple brushes adding on top of each other.&#x20;

Because of this, writing a brush' surface directly into a channel is usually not desirable. Instead, it is generally desirable to combine the brush surface with the current state of the channel the brush is about to write into. The result of that combination can then be written into the channel. This prevents brushes completely overriding others that come before them (and therefore making those previous brushes useless).

_Image_

The combining of surfaces can be done with one of a handful of nodes, all of which also take a Smoothness input (this is usually [exposed as a parameter](#user-content-fn-1)[^1]), allowing clean blending of multiple surfaces. The following functions exist:

* **Smooth Union**
  * This node adds the two surfaces together.
* **Smooth Intersection**
  * This node returns the area that overlaps between two surfaces. Somewhat counter-intuitively, an increase in smoothness will make the returned overlap smaller.
* **Smooth Subtraction**
  * This node subtracts the "surface to subtract" from another. With this operation, order is incredibly important, as A-B is not the same as B-A. &#x20;

_Images_&#x20;

## 2. Optimizing Brushes

The **Register to Channel** node takes a bounds input. When nothing is plugged into this pin, the brush will be registered with infite bounds, in turn meaning that this graph will be run for every voxel in the world. This is incredibly wasteful, as voxels a kilometer away obviously do not need to be affected by a small brush. While this does not matter too much when only a single brush is used, the total world generation time will increase rapidly as the amount of brushes grows, as each will affect every voxel in the world.

This can be addressed by assigning a sensible value to the Bounds pin. When using a node that creates a surface, this surface generally includes correct bounds, so a **Get Surface Bounds** node can be used on the surface pin, and this can be plugged into the channel's bounds pin.

<figure><img src="../../.gitbook/assets/image (135).png" alt=""><figcaption><p>The "Create Voxelized Mesh Surface" comes with correct bounds, so we can use those directly for the channel registration.</p></figcaption></figure>

When a surface is generated manually, for instance using a **Make Volumetric Surface** node, the math for the bounds will need to be done manually as well.

With the brush' bounds configured properly, the brush will only affect the world's generation within those bounds, making world generation significantly cheaper. &#x20;

{% hint style="warning" %}
Be careful when controlling bounds manually, or using high smoothness. If you set bounds that are too small, or don't account for smoothness, you might start to get [rendering glitches/holes](./#fixing-glitches-when-the-smoothness-is-too-high) in the voxel mesh.
{% endhint %}

### &#x20;  2.1 Holes and glitches with high smoothness

If you increase your smoothness a lot, you might start to notice holes in the voxel mesh:

<figure><img src="../../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

This is because as the smoothness increases, the area affected by the brush increases as well, but the voxelized mesh bounds don't increase accordingly.

Plugging the same value as was used on the Smooth Union into the Channel's Smoothness pin will allow the plugin to account for this increase in bounds.&#x20;

<figure><img src="../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

With the smoothness properly accounted for in the channel registration, the artifacts should now be resolved.

{% hint style="warning" %}
If accounting for smoothness does not fix holes in the surface, another likely culprit is an inconsistent distance field, leading to falsely failed distance checks. Read more about this issue and its workarounds here: [distance-fields-and-distance-checks.md](../surfaces-and-materials/working-with-surfaces/distance-fields-and-distance-checks.md "mention")
{% endhint %}

## 3. Brush Priorities

The **Register to Channel** node has a Priority pin. This is used to sort brushes.

The brush with the highest priority will be queried first. If that brush calls **Query Channel**, the brush with the second highest priority will then be queried, and so on.

Brush priority can be useful to tune when dealing with smoothness: usually, you want to apply Smoothness after everything else has been computed. If a brush does not seem to be properly affected by its smoothness setting, the solution is often to adjust its priority.

### &#x20;  3.1 Determinism

In order to always have different priorities & make the sorting deterministic, brushes have an internal priority computed as follows:

* Compare the **Register to Channel** Priority
* If equal, fall back to a priority based on the path of the graph asset the node is in
* If equal, fall back to a priority based on the path of the brush actor
* If equal, fall back to a priority based on the node id hash

This is not usually very important, but it can be a source of problems if brushes are **spawned at runtime**. If that is the case, make sure all brush actor names are synchronized between clients & servers, and that the names of these actors are saved and loaded properly. Failure to do so will likely result in brushes being having inconsistent effects across game instances.

## 4. Graph Instances

When you have a lot of parameters, sometimes you want to create templates for these parameters - just like material instances in the engine.

In order to do this, find the graph this instance should be based on, right-click it and select "Create Instance". This will create a Graph Instance object which inherits its logic from the parent, but allows you to override just the parameters.&#x20;

<figure><img src="../../.gitbook/assets/image (70).png" alt=""><figcaption><p>In this case, the parent brush has Smoothness exposed as a parameter, so that can be customized on the instance.</p></figcaption></figure>

## 5. Brush Preview Meshes

By default, graphs don't generate any sort of collision, making them hard to select in the viewport. For brushes, a preview mesh can be generated using the **Create Marching Cube Preview Mesh**, which can then be used as collider so it can be selected. Additionally, this will help show which brushes are selected and what shapes they have.

<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption><p>This node creates a standalone mesh with collision, rendered when it is selected.</p></figcaption></figure>

By default, the preview mesh is a perfect match of the rendered mesh. This is not always desirable as it leads to Z-fighting and overlapping meshes.

This can be accounted for by using the Grow node on the surface plugged into the preview mesh node.

### &#x20;  5.1 Improving the brush asset preview

The Preview tab of a graph asset uses the same preview mesh and material as the editor viewport.

<figure><img src="../../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

The **Is Preview Scene** node tells a graph whether it is being shown in the editor viewport, or in the graph editor scene. Using it, you can configure your graph to run different logic, or use a different material, between these two situations.



[^1]: Parameters are values that can be edited in the details panel (or through C++ and Blueprints) on actors that use a graph.&#x20;

    To expose a value as one, drag out from an input pin, let go, and select "Promote to Parameter".
