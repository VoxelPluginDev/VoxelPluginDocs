---
description: A step-by-step guide on creating a basic procedural terrain from scratch.
---

# First Steps with Voxel Worlds

The very first step when working with Voxel Plugin 2 is to place a `Voxel World` actor in the level. Make sure its transform is set to zero so the terrain isn't offset. You'll notice that placing this actor hasn't actually made any terrain show up yet. A Voxel World acts as a manager for other voxel-related actors. It is used to control rendering and collision settings from a central place, but it won't do anything by itself.

<figure><img src="../../.gitbook/assets/image (216).png" alt=""><figcaption></figcaption></figure>

In order for anything to actually render, stamps need to be placed in the level. Voxel Stamps are actors that tell the Voxel World that some kind of data needs to be generated (and in most cases, rendered) in a particular place. The terrain will be generated anywhere that has a stamp, and it will not be generated where there aren't any stamps.

The plugin supports height stamps and volumetric stamps. Both of these can be read from a static data source (heightmap assets for height, voxelized meshes for volumetrics), but they can also be generated from a graph. Ideally, height stamps should be used whenever possible, because they are cheaper to generate.

In this case, the goal is to generate a base terrain with a procedural height stamp. To do so, right-click in the content browser, and then click the `Voxel Height Graph` type in the `Voxel` category. Open the created asset. A graph with a single `Output Height` node will appear.

<figure><img src="../../.gitbook/assets/image (199).png" alt=""><figcaption></figcaption></figure>

This graph will run as part of the Voxel World once it's placed in the level, but it won't actually do anything unless it has a set of bounds assigned.&#x20;

To assign bounds, drag out from the Bounds pin and place a `Make Box 2D From Radius` node. Drag out from the `Radius` input pin and click `Promote to parameter`. This makes it configurable from the details panel on placed actors. With the parameter selected, set the default value to something like 5000 in the details panel on the right. Use the same flow to create a parameter for the `Material` pin on the `Output Height` node.

As the graph is now, it will just write a flat plane as height data. To make the terrain more interesting than that, we can use noise. Drag out from the `Height` pin and place an `Advanced Noise 2D` node. Attach `Get Position2D` to its position pin.

{% hint style="info" %}
`Get Position2D` can be set to Local Space or World Space. Local Space will use position relative to the stamp's transform. World Space won't be affected by the stamp's transform.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (217).png" alt=""><figcaption></figcaption></figure>

With this done, minimize the graph editor and drag the graph from the content browser into the level. You'll see a terrain appear, the size being controlled by the radius parameter. The stamp only prompts terrain to generate in an area the size of its bounds around its position.

The height of the terrain can  be adjusted by simply moving the stamp up and down in the level.&#x20;

Right now, the terrain will be grey, but any material can be assigned to the material parameter, and it will immediately show up on the terrain. For details on how to work with materials on voxel terrains, see the [Materials](../../knowledgebase/materials/) section.

<figure><img src="../../.gitbook/assets/image (212).png" alt=""><figcaption></figcaption></figure>

Stamps will automatically blend together when placed around each other, with the way they blend being controlled by their Blend Mode, Smoothness and Priority settings.&#x20;

{% hint style="info" %}
When stamps are set to the Override blendmode, they will override all existing information in their bounds. But they can access the previous surface information in their graphs, allowing for manually configured blending behavior through their graph. Read more here.
{% endhint %}
