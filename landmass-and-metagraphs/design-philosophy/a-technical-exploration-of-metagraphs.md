---
description: >-
  This article explores the technical background for MetaGraphs, and how this
  impacts usage. It also covers the basics of the C++ node API.
---

# A Technical Exploration of MetaGraphs

### 1.1 Core Concepts <a href="#block-eb917f190cef4fc783629eb78bf110ec" id="block-eb917f190cef4fc783629eb78bf110ec"></a>

MetaGraphs are general purpose: they are not limited to generating voxels, but rather specialize in generating large amount of data for any kind of usage. They can be used to generate voxel terrains, but also heightmap terrains, procedural foliage, custom meshes, and all manner of other things which for now are unexplored.

Unlike previous iterations, they do not assume anything about what generation algorithm you might want to use. If you want to use marching cubes to mesh your distance field, you can place a Marching Cube node. If you want to instead mesh using dual contouring, one only needs to program a self-contained node which implements dual contouring, after which it will work with any other plugin system without issues.

Finally, MetaGraphs can work at varying granularities: they are able to generate data per voxel, per chunk, per 5x5km square, or any other data frequency that might be appropriate. This makes it very straight forward to do computation at different detail levels across different resolutions, and makes it easy to e.g. place one village every 5km like you’d see in Minecraft.

### 1.2 An Example Graph <a href="#block-1c155de50f544a768ad6f125b55dbdc5" id="block-1c155de50f544a768ad6f125b55dbdc5"></a>

Before getting any further, let’s break down one example:

<figure><img src="https://docs.voxelplugin.com/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F3447097e-0903-4554-a104-60a763ec7cdf%2Fimages%2F9335e2e5-c927-4394-b9f1-1eeb1fb963cb.png&#x26;w=3840&#x26;q=80" alt=""><figcaption></figcaption></figure>

Meta Graphs, unlike most graphs, execute **right to left**. This is an important note to keep in mind, as it dramatically impacts the way data flows between different nodes and sections within the graph.

When the voxel actor starts generating, the following happens:

1. The **Root** node is executed. To do so, it executes all of the nodes connected to it.
2. The **Spawn Chunks by Screen Size** node is executed by the **Root** node. This node will generate chunks of different sizes to fill the screen somewhat uniformly — chunks further away will be lower resolution. For each generated chunk, it will execute its input pin.
3. **For each chunk**: the **Render Marching Cube Mesh** node is executed by the **Spawn Chunks** node. To execute, it will query its Mesh Data and Material pin, and when they are done it will render a new mesh using them.
4. The **Make Marching Cube Mesh Data** node is executed by the **Render Mesh** node. To execute, it will:1. Query its Voxel Size pin2. Generate positions to be queried using the chunk bounds passed down from the **Spawn Chunks** node, according to the Voxel Size and the LOD.3. Pass these positions to the Density pin4. Wait for the Density pin to return an array of densities. It will return an array because the pin is an array pin: this is denoted by the square pin icon5. Once densities are computed, it will run a marching cube meshing algorithm on them to finally output a mesh
5. The **Make Density from Height** node is executed by the **Make Marching Cube Mesh Data** node, as part of step 4.3. To execute, it will query its Position pin — which will return the positions passed down by the **Make Marching Cube Mesh Data** node — and will then run some math on the resulting array to compute a density (in this case, `Density = Position.Z - Height` )

As you can see, the execution of a simple graph like this is already pretty complex: this is due to the data going both ways (eg, the positions to query going right to left and the density going left to right).

However, this simple graph does a LOT on its own: it defines how chunks are generated, defines how they are meshed, and what density to use for the meshing. All the generation parameters are defined in the graph itself: there’s no hidden setting on the voxel actor to define the voxel size, the chunk size etc.

### 2.1 Right-to-Left Execution <a href="#block-67b14488379f4ebe8187fd2602016dcb" id="block-67b14488379f4ebe8187fd2602016dcb"></a>

The graph executing right to left can be pretty confusing, but enables it to be a lot more powerful.

By doing so, it allows nodes to consider their input pins as functions they can query however they want: in the example above, the Marching Cube node decides how to query its Density pin.

This will allow nodes to have very complex behavior: if a node needs more info about the voxel or the chunk next by for example, it can just query it.

If we consider Minecraft-like tree generation, a tree generator node sometimes needs to access neighboring chunks data if a tree overlap them. With this new API, the node can simply query the neighboring chunks as required.

### 2.2 GPU Execution <a href="#block-df08673e6488498cabf0ed603eeef9d4" id="block-df08673e6488498cabf0ed603eeef9d4"></a>

The graph execution is fully asynchronous: each node can decide to run on a voxel thread, on the render thread or on the game thread. Voxel buffers (arrays of data, like an array of density values) automatically work with both the CPU and the GPU, copying themselves to the correct destination if needed.

Typically, you could decide to do this:

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

In this case, the Make Density from Height node will be run in a GPU compute shader. The data will then be copied back to the CPU to be processed by Make Marching Cube Mesh Data.

Of course, data copy between the CPU and GPU takes valuable PCIe bandwidth and should be avoided as much as possible. In order to do that, several of our pipelines can execute entirely on the GPU.

For example, you can generate detail voxel textures on the GPU. The data doesn’t need to be copied back to the CPU, because it’s stored in textures and used by the GPU during rendering.

### 3.1 C++ API <a href="#block-e153c937da1a48819d44b43c64863846" id="block-e153c937da1a48819d44b43c64863846"></a>

All the plugin logic is now centered around nodes. Nodes can use any USTRUCT as pin types.

Defining a new node is fairly straightforward, let’s have a look at the heightmap brush nodes.

First we define a new USTRUCT to be used as pin type — we want to pass brushes between nodes:

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

We then declare a `FindLandmassHeightmapBrushes`  node that will find heightmap brushes in the scene:

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

This node references a subsystem (`FVoxelLandmassHeightmapSubsystem`) which is used to track active brushes. It has one input pin: `LayerName`, which defaults to “Main”, and one output pin, `Brushes`.

To define the node, we have this in the corresponding .cpp file:

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

1. `ResolveVoxelQueryData(FVoxelBoundsQueryData, BoundsQueryData);`Query data is the name of the data being passed from right to left by the node callers. In this case, we’re looking for a `FVoxelBoundsQueryData`: this is telling us where to find brushes. If that query data is not found, `ResolveVoxelQueryData` will raise a user-friendly error and exit.
2. `const TVoxelFutureValue<FName> LayerName = LayerNamePin.Get(Query);`Since the graph is executing async, querying a pin returns a future value. We then need to wait for these values before doing anything with them. Here, we query `LayerNamePin` for its value, potentially starting a new background task to do so.
3. `return VOXEL_ON_COMPLETE(AsyncThread, BoundsQueryData, LayerName)`Since the graph is executing async, we need to wait for the pin we queried. This is what the `VOXEL_ON_COMPLETE` will do for us.The first argument, `AsyncThread`, tells it we want to be executed on a voxel thread once we’re done waiting.`BoundsQueryData` and `LayerName` need to be passed to the macro to be available in the function body below. Since `LayerName` is a `TVoxelFutureValue`, it will automatically be waited on.
4. Once `LayerName` is computed, we can access it using `LayerName` or `LayerName->`. The subsystem we declared in the header is available anywhere within the node definition as `Subsystem`.

### 3.2 Dependencies <a href="#block-30cedcb6e3794fa2a557163f5b4fe0e1" id="block-30cedcb6e3794fa2a557163f5b4fe0e1"></a>

`This section is not yet done.`
