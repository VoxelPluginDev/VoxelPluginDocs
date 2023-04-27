# Density Canvas

Density canvases can be used to let players do voxel edits.

## Setting up the density canvas

Start by following the [brush-and-channels.md](brush-and-channels.md "mention") page to setup a new voxel channel.

Once you have a channel, create a new Density Canvas:

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

In the Density Canvas, assign the channel to your voxel channel:

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

In your meta graph, instead of sampling a channel, sample your density canvas:

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

Here I also create an infinite flat brush to have a flat ground.

## Editing the density canvas

In the blueprint you want to use to edit your canvas, add a new variable of type Voxel Density Canvas and set its default value to be our canvas asset:

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

You can then use a variety of functions to edit your canvas at runtime:

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

The Position/Radius are in world space.

By default, edits made in PIE will be persisted. To avoid that, you can make a new canvas at runtime & assign it to your graph:

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Canvases do not currently support save/loads
{% endhint %}
