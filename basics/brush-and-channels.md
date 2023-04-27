# Brush & Channels

Voxel Channels allow you to read & write arbitrary data in a 3D space. Typical use cases include:

* Having a channel to store the terrain height
* Having a channel to store the terrain distance field
* Having a channel to store the distance to the closest river

Voxel Brushes allow you to write into a channel from a brush actor that you can move in your world.

## Setting up a new channel

&#x20;Create a new Voxel Channel asset:

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

Leave the channel type as Distance Buffer. If you wanted to have a height channel, this would typically be a Float Buffer.

<figure><img src="../.gitbook/assets/image (6) (3).png" alt=""><figcaption></figcaption></figure>

## Setting up a simple meta graph

For the purpose of this tutorial, we'll need a simple meta graph.

Create a new meta graph asset:

<figure><img src="../.gitbook/assets/image (30) (1).png" alt=""><figcaption></figcaption></figure>

Add the following nodes to it:

<figure><img src="../.gitbook/assets/image (23) (1) (1).png" alt=""><figcaption></figcaption></figure>

The Sample Channel node will sample our channel for us. Make sure to set the channel reference to the asset we just created.

## Setting up a new brush asset

Create a new brush asset:

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

At the top right of the asset window, switch to the Brush Graph:

<figure><img src="../.gitbook/assets/image (4) (2).png" alt=""><figcaption></figcaption></figure>

Add the following nodes:

<figure><img src="../.gitbook/assets/image (20) (1).png" alt=""><figcaption></figcaption></figure>

All this is doing is reading the previous channel value, and writing it as the new channel value. To have a brush do something, we need to do something to that distance!

Add the following nodes:

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

For the mesh I've used the EditorSphere from engine content, but any mesh will work.

In this graph we do quite a few things:

* We voxelize a mesh
* We sample it to get the distance to the voxelized mesh
* We transform that distance from local mesh space into world space&#x20;
  * This is where the brush actor transform is used
* We apply a smooth union to blend that new distance with the existing one

You can then drag & drop your brush asset into the scene. Make sure you also dragged your meta graph into that same scene. You should see the following:

<figure><img src="../.gitbook/assets/image (19) (1).png" alt=""><figcaption></figcaption></figure>

If you duplicate that brush actor & scale it, you can get this:

<figure><img src="../.gitbook/assets/image (29) (1).png" alt=""><figcaption></figcaption></figure>

## Optimizing the brush asset

In our brush asset graph, we currently do not set the Bounds pin. This means the brush asset is sampled everywhere in the world.

As we will add more and more brushes, the cost will start to increase significantly.

To improve that, we can set the Bounds pin to the mesh bounds. This will make it so the brush is only sampled when needed!

Add the following nodes:

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

The world will now generate faster!

{% hint style="danger" %}
Be careful when setting the bounds. If you set bounds that are too small, you might start to get rendering glitches/holes in the voxel mesh
{% endhint %}

## Brush priorities

You might have noticed the Write Channel node has a Priority pin. This is used to sort brushes.

The brush with the highest priority will be queried first. If that brush calls SampleChannel, the brush with the second highest priority will then be queried, and so on.

Brush priority can be useful to tune when dealing with smoothness: usually, you want to apply Smoothness after everything else has been computed. If you have a brush whose smoothness doesn't seem to be working properly, try increasing its priority!

### Determinism

In order to always have different priorities & make the sorting deterministic, brushes have an internal priority computed as follows:

* Compare the WriteChannel Priority first
* If equal, fall back to a priority based on the path of the graph asset the node is in
* If equal, fall back to a priority based on the path of the brush actor
* If equal, fall back to a priority based on the node id hash

You usually don't need to care about any of that EXCEPT if you're spawning brushes at runtime. if you do so, make sure your brush actor name is the same on client & servers, otherwise you might get mismatches!

## Exposing brush smoothness

You might have noticed the Smooth Union node has a Smoothness pin. This controls how smooth the blend with the previous distances is.

Right click that pin and select Promote to parameter:

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

This will create a new parameter. That parameter will automatically copy the pin name and default value!

If you select a brush actor in your scene, you will see you can now set the smoothness per brush:

<figure><img src="../.gitbook/assets/image (21) (1).png" alt=""><figcaption></figcaption></figure>

Increasing the smoothness should make the brushes blend together!

{% embed url="https://youtu.be/YWNkaGaWMIo" %}

## Creating a brush asset instance

When you have a lot of parameters, sometimes you want to create templates for these parameters - just like material instances in the engine.

This is possible in the plugin too! Create a new brush asset. Instead of going to the graph, assign its Parent to the first asset we created:

<figure><img src="../.gitbook/assets/image (18) (1).png" alt=""><figcaption></figcaption></figure>

The smoothness parameter is now showing up, allowing you to tune your asset!

## Fixing glitches when the smoothness is too high

If you increase your smoothness a lot, you might start to notice holes in the voxel mesh:

<figure><img src="../.gitbook/assets/image (28) (1).png" alt=""><figcaption></figcaption></figure>

This is because due to the high smoothness, we're going outside of the brush bounds!

To fix that, we can tell what smoothness we're using to the plugin:

<figure><img src="../.gitbook/assets/image (16) (1).png" alt=""><figcaption></figcaption></figure>

Once that's done, the rendering auto-magically fixes itself!

<figure><img src="../.gitbook/assets/image (22) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Adding brush preview

You might have noticed you can't easily select brushes in the level, and that when you do so no overlay appears.

This is because we haven't setup a preview mesh for our brush.

Add the following nodes to your brush graph:

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

You should now be able to click a brush in your level to select it!

By default, the preview mesh is a perfect match of the rendered mesh. This is not always desirable as you'll get some Z fighting/overlapping meshes.

Add the following node to your graph:

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

This will extend the distance field by 1m. Your brush preview should now look like this:

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

## Improving the brush asset preview

If you go back to the Preview tab in your brush asset, you will see your brush is transparent:

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

if you'd like to use a different material, you can do so like this:

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

Which will get you this:

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

## Including the smoothness in the brush preview

With our current setup, the preview mesh does not account for any smoothness:

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

We can fix that with a bit of math:

<figure><img src="../.gitbook/assets/image (33) (1).png" alt=""><figcaption></figcaption></figure>

Here, we take the result of the Smooth Union and subtract from it the value of the channel before we added our brush. This effectively gives us the true delta created by our brush.

Much better!

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>
