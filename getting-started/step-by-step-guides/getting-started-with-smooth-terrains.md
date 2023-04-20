---
description: >-
  The basic functionality of MetaGraphs, using noise and Landmass brushes to
  create a smooth (Marching Cubes) terrain.
cover: ../../.gitbook/assets/image (4) (1).png
coverY: 0
---

# Getting Started with Smooth Terrains

{% hint style="info" %}
This guide was made using Unreal Engine 5.1, with a plugin release from early December.
{% endhint %}

For a more detailed understanding on how a mesh is generated from input data, we strongly recommend watching [this video by Sebastian Lague](https://www.youtube.com/watch?v=M3iI2l0ltbE), which explains the Marching Cubes algorithm we use. In Voxel Plugin the density field used for this is generally an approximate distance to the nearest surface in world units (cm in stock Unreal), so we refer to it as a distance field, or as the "Distance" type.

### 1. Preparing your Graph and Voxel Actor <a href="#block-f0f3707dae9d43b48c8b2ada9466e73f" id="block-f0f3707dae9d43b48c8b2ada9466e73f"></a>

To start off, create a MetaGraph asset from the content browser right-click menu.

<figure><img src="../../.gitbook/assets/image (4) (2).png" alt=""><figcaption></figcaption></figure>

Select the “Empty” starting point from the pop-up window, and then press “Create”.\


<figure><img src="../../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

Drag the created MetaGraph asset from the content browser into the editor’s viewport - this will create a Voxel Actor in the scene with the newly created MetaGraph assigned as generator.

<figure><img src="../../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

Double-click the graph asset to open the MetaGraph editor. With the graph asset open, right-click and search for "On Construct" in the search bar. Click the matching result to place it as a node. Next, drag out from the "On Construct" node and let go on open space in the graph to again open a search menu.&#x20;

{% hint style="info" %}
More information on how to find the right nodes to use can be found here: [using-metagraphs](../using-metagraphs/ "mention")
{% endhint %}

In this search menu, have a look at the available options. The `Flow Control` and `Misc` categories are not useful for generation, as they just contain utilities. TThat leaves us with the `Chunk` category. If we expand this, we get a few options. In this case - and for most terrains - we'll be choosing the `Spawn Chunks by Screen Size` node, as it works in 3D and creates LODs.

{% hint style="info" %}
For an extended explanation on Chunk Spawners and when to use which, see the [Broken link](broken-reference "mention") page.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (8) (3).png" alt=""><figcaption></figcaption></figure>

Having placed a chunk spawner, there are two visible execution pins. The top one can be used for creating more chunks, and the bottom one is used for the instructions on what each chunk created by this node should do. In this case, to start creating instruction on what these chunks should look like, drag out from the second pin.

To render the terrain, a mesh is required. Open the `Mesh` category in the search panel, and add a `Create Mesh Component` node. The `Spawn Chunks by Screen Size` node will call this for every chunk, resulting in a procedural mesh component being created per chunk.

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

If desired, Collision and Navigation can be created per chunk (potentially with a different chunk spawner for different LODs) in the same way.

&#x20;In this case, let's add a `Spawn Chunks by Range` node and make that generate collision for the mesh. This will create an area of LOD0 collision around the player at all times, avoiding physics glitches that come with colliders changing with LODs. Try finding and placing these nodes by yourself, and then use the image below to check if your graph is correct.&#x20;

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

`Create Mesh Component` takes a `Mesh` input. Drag out from this pin, and place `Create Mesh from Marching Cube Surface`. Marching Cubes is the algorithm used to generate smooth terrains from a density field - [this video](https://www.youtube.com/watch?v=M3iI2l0ltbE) is good explanation of this topic.

For `Create Collision Component`, we need a `Collider` and a `Body Instance`. Dragging out from the `Collider`, place a `Create Collider from Marching Cube Surface` node.

For the `Body Instance` (often called "Collision Preset" in the engine UI), dragging out will give no options other than `Flow Control` and `Misc`. This is because `Body Instance` is a complex struct, which needs to be configured as a variable. In the search menu, pick the `Promote to Local Variable` option. Its default collision values are fine for this example.

{% hint style="info" %}
More information on working with variables and similar UX topics can be found on the [using-metagraphs](../using-metagraphs/ "mention") page.
{% endhint %}

The graph should now look mostly like this:

<figure><img src="../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

With this, the groundwork for the world generation has been done. From here, all that is left is to actually tell these chunks what their mesh surfaces need to look like. That means that from this point on, the `Create Collider` and `Create Mesh` nodes will take the same surface inputs.

Start by dragging out from the `Surface` pins on either the collider or the mesh node, and placing the `Generate Surface` node. Make sure to connect the `Surface` pin on the newly placed node to both the other nodes - otherwise your collider or mesh will not generate. Once again, the default settings are fine, and we will only worry about the top-most pin; `Density`.

The `Density` pin is a different kind of pin from the others in this graph so far. Rather than having the standard round pin icon, it instead has a square as pin icon. This signifies that the `Density` pin is a `Buffer`, rather than a `Uniform` like all the other pins so far. A `Buffer` is simply a stack of data: that single pin represents all the density points within a chunk at the same time, rather than just being a single value.&#x20;

{% hint style="info" %}
If that `Buffer` explanation makes little sense, just know this:&#x20;

* If a value changes based on position, it is almost always a `Buffer`.&#x20;
* If a value is the same for every position, it is almost always a `Uniform`.&#x20;

`Buffers` can only be used as an input for `Buffer` pins specifically. `Uniforms` can be used as an input for both `Buffer` and `Uniform` pins.

If you want to learn more about buffers and uniforms, have a look at the [buffers-and-uniforms.md](../../landmass-and-metagraphs/deep-dives-into-metagraphs/buffers-and-uniforms.md "mention") page.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

In this graph, the intent is to create a basic heightfield terrain. Drag out from the `Density` pin, and place the `Make Density from Height` node. This converts a height input to a density, or distance field, output. Drag out from `Position` to place a `Get Position 3D` node. Next, in order to start defining the shape of the height field, drag out from `Height`. Before doing anything with noise, though, place a `Query 2D` node. This will tell the graph that this section of the logic is not volumetric, and makes it significantly cheaper to run.

{% hint style="info" %}
More information on `Query 2D` and other nodes like it can be found on the [utilities-for-performant-graphs.md](../../landmass-and-metagraphs/optimizing-metagraphs/utilities-for-performant-graphs.md "mention") page.
{% endhint %}

Now it is time to place a noise[^1] node. Drag out from `Query 2D` and place an `Advanced Noise 2D` node. Drag out from its `Position` input (note that it takes a blue position, a Vector2, due to its 2D nature, rather than a yellow Vector3) and place `Get Position 2D`.

At its default settings, this node runs ten octaves (a technical word for layers) of Perlin noise, with 200 meters (amplitude times two) of height difference between its lowest and highest point, and its large shapes something like 1000 meters apart (feature scale). Note that all world units are specified in Unreal Units, which equates to centimeters.

Right-click the `Value` output on the noise node, and select `Preview pin`. This will draw the resulting height from this node in the preview in the top-left of the graph. The graph now looks something like this:

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

And with that, the graph is _almost_ finished.&#x20;

Looking at the viewport with the Voxel Actor, there is definitely a terrain mesh, but it has all sorts of rendering artifacts on it. This is because the mesh does not have any kind of vertex data like Vertex Normals right now, so the engine does not know how to light the terrain.&#x20;

<figure><img src="../../.gitbook/assets/image (7) (3).png" alt=""><figcaption></figcaption></figure>

Going back to the graph, look back to the `Create Mesh` node. Drag out from its `Vertex Data` pin and create a `Make Vertex Data` node. Its vertex colour being black is fine, and its texture coordinates, or UVs, being zero is also fine. The issue is that the terrain normals are not being created. Drag out from the `Normal` pin, and place a `VMGM_GetGradient` node.

{% hint style="info" %}
`VMGM_GetGradient` is a MetaGraph Macro - read more about macros here: [metagraph-macros.md](../../landmass-and-metagraphs/deep-dives-into-metagraphs/metagraph-macros.md "mention")&#x20;
{% endhint %}

Now, try to drag from the `Density` pin into the `GetGradient`'s `Value` pin. Notice that a pop-up appears, saying `Convert Float Buffer (Density) to Float Buffer`. Connect the pins, and a new node will appear in between, converting your `Density` type to a `Float` type. These nodes are how type conversions appear in MetaGraphs.

{% hint style="info" %}
Learn more about how float and density types work together on the [Broken link](broken-reference "mention") page.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>

With all this connected, the graph is now entirely functional and rendering a fully configured mesh.&#x20;

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

From here, next steps could be:

* Material creation, with detail textures for better distant terrain - [passing-data-to-materials.md](../../landmass-and-metagraphs/deep-dives-into-metagraphs/passing-data-to-materials.md "mention")&#x20;
* Using Landmass brushes to modify your terrain manually - [working-with-landmass](../../landmass-and-metagraphs/deep-dives-into-metagraphs/working-with-landmass/ "mention")
* Spawning foliage on your terrain - [getting-started-with-foliage.md](getting-started-with-foliage.md "mention")

[^1]: Noise is what we call procedural, or predictably random, patterns. We use noise with smooth gradients, with larger and smaller shapes, to define terrain shapes.
