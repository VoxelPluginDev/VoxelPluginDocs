---
description: >-
  A basic guide exploring the basic functionality of noise and Landmass brushes
  to create a smooth (Marching Cubes) terrain through MetaGraphs.
---

# Getting Started with Smooth Terrains

{% hint style="warning" %}
**Note:** Voxel Plugin 2.0 is changing rapidly, and the master branch is often unstable or partially broken. The primary supported platform is the launcher build of 5.1.

The latest release of Voxel Plugin 2.0 can be downloaded through the [Plugin Downloader](../), or from [GitHub](https://github.com/VoxelPlugin/VoxelPlugin/) for manual compilation.&#x20;

This guide was made using Unreal Engine 5.1, with a plugin release from early December.
{% endhint %}

For a more detailed understanding on how a mesh is generated from input data, we strongly recommend watching [this video by Sebastian Lague](https://www.youtube.com/watch?v=M3iI2l0ltbE). In Voxel Plugin the density field is generally an approximate distance to the nearest surface in world units (cm in stock Unreal), so we often refer to it as a “distance field”.

### 1. Preparing your Graph and Voxel Actor <a href="#block-f0f3707dae9d43b48c8b2ada9466e73f" id="block-f0f3707dae9d43b48c8b2ada9466e73f"></a>

To start off, create a MetaGraph asset from the content browser right-click menu.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Select the “Empty” starting point from the pop-up window, and then press “Create”.\


<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Drag the created MetaGraph asset from the content browser into the editor’s viewport - this will create a Voxel Actor in the scene with the newly created MetaGraph assigned as generator.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Double-click the graph asset to open the MetaGraph editor. With the graph asset open, drag out from the “Root” node. Expand the “Chunk” category and select the “Spawn Chunks by Screen Size” option to spawn said node. This node will create cube-shaped chunks across the entire world in three dimensions, attempting to keep their size on screen consistent, making farther away chunks less detailed.

{% hint style="info" %}
When working with MetaGraphs, there may well be times where you are unsure what node to use. If you are unsure what to plug into a pin, drag out from it onto the graph and let go. The search menu will only show compatible nodes, making it easier to find what you're looking for.
{% endhint %}

&#x20;If we drag out from the Root node, the only functional nodes that can be placed are the “Spawn Chunks” nodes.&#x20;

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

Placing a “Spawn Chunks” node and dragging out again will again give us some clear options to choose from.

In this case, we want to add a “Render Marching Cube Mesh” from the “Mesh” category. The “Spawn Chunks by Screen Size” node will call this for every chunk it generates (more information on graph execution order can be found . In doing so, it will create a procedural mesh component for each chunk in the world.





Further reading on these topics can be found on the “[What Are Landmass & MetaGraphs](https://docs.voxelplugin.com/landmass-metagraphs/design-philosophy)” page for an outline on the design ideas behind them, and on the “[A Technical Exploration of MetaGraphs](https://docs.voxelplugin.com/landmass-metagraphs/technical-exploration)” page for a more technical explanation of how MetaGraphs work.
