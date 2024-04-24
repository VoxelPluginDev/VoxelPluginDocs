---
description: >-
  It can be difficult to work with the Complex Material sample, but it is
  designed to be used and customized as template.
---

# Extending the Complex Material Sample

{% hint style="warning" %}
This article assumes moderate knowledge about Unreal Engine's material editor and [Material Definitions](./). We also recommend exploring the BasicMaterial sample first.
{% endhint %}

Some changes - for example, adding support for another material property like roughness - are relatively straight-forward. Others, such as customizing triplanar projection blends, can get very complicated. As such, this article is split into the following two sections:

* 1.) Step-by-step guides on some straight-forward extensions to the material, i.e. adding support for additional texture channels (roughness, AO, etc.)
* 2.) Deep explanation of the material design, providing the perspective needed to make more complex changes. &#x20;

{% hint style="info" %}
The Complex Material sample does not currently fully support planetary rendering, as the top and sides textures will behave as if the world is flat.&#x20;
{% endhint %}

## 1. Straight-Forward Changes

### 1.1 Supporting additional material properties

For the purposes of this guide, support for roughness textures will be added to the material. To get started, open **VMD\_ComplexMaterial**, **MF\_CompleteLayer** and **MF\_FinalOutput**.

First off, within the **VMD\_ComplexMaterial** asset, duplicate the `Diffuse - Top` and `Diffuse - Sides` parameters by pressing Ctrl-D. Rename the two to Roughness, and change their default textures as needed. Save the VMD asset.

<figure><img src="../../../.gitbook/assets/image (157).png" alt=""><figcaption></figcaption></figure>

These are the only changes necessary on this side, so the VMD asset can now be closed.&#x20;

From here, move to **MF\_CompleteLayer**. This function contains a handful of different comment blocks. Zoom in on, and then copy the Color comment block on the left-hand side. Paste it in the empty space to the side.&#x20;

<figure><img src="../../../.gitbook/assets/image (149).png" alt=""><figcaption></figcaption></figure>

Select the Voxel Parameter nodes, and in the details panel, select the top and side Roughness texture parameters from the VMD. Delete the Static Bool plugged into the `UseModifier` pin. Drag out from the `Result` pin and create a local variable declaration called `RoughnessOut`.&#x20;

Lastly, for the sake of organization, rename the comment block to Roughness.

<figure><img src="../../../.gitbook/assets/image (150).png" alt=""><figcaption></figcaption></figure>

While the core logic is now in place, the data from it still needs to actually be fed into the Material Function's output.&#x20;

To do this, shift the view to the `Output Result` node on the right-hand side, and select the `SetMaterialAttributes` node. Click the plus in the details panel, and change the last entry in the list to Roughness using the selector. Drag out from the newly created pin, and connect a `Get RoughnessOut` (our previously created named reroute) to it. Click the `Apply` and `Save` buttons to make sure the changes are applied.

<figure><img src="../../../.gitbook/assets/image (152).png" alt=""><figcaption></figcaption></figure>

The roughness information will now be fed through the entire material, and be blended per layer. All that is left is to make sure it's properly assigned before the final output. Close **MF\_CompleteLayer**, and move to **MF\_FinalOutput** instead.&#x20;

Within **MF\_FinalOutput**, add a roughness pin to the `GetMaterialAttributes` node, and connect it to the existing roughness pin on the `SetMaterialAttributes` node. The nodes that are now disconnected can be deleted. Once again, click the `Apply` and `Save` buttons to make sure the changes to this function are applied.

<figure><img src="../../../.gitbook/assets/image (153).png" alt=""><figcaption></figcaption></figure>

With these steps completed, the terrain should now be using the default roughness textures. Textures can be selected per-layer inside the VMDIs[^1].&#x20;

#### &#x20;     1.1.1 Supporting channel-packed textures

In the case of channel-packing, the entire workflow is largely identical, except that the **MF\_CombinedProjection** output should be split into its respective channels.

There is one exception to this when trying to channel-pack additional texture into the Height texture. In this case, the `Result` pin will only output the height channel (selected through the parameters list).

Instead, use [the disconnected output from the right-most `Lerp` node](#user-content-fn-2)[^2] to access the channel-packed result.

<figure><img src="../../../.gitbook/assets/image (154).png" alt=""><figcaption></figcaption></figure>

Do make sure to connect the individual texture channels to appropriate material properties, both at the output node in **MF\_CompleteLayer**, as well as in the **MF\_FinalOutput** configuration.

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

### &#x20;  1.2 Increase texture size on distant terrain

{% hint style="info" %}
This section is still to be written.
{% endhint %}

## 2. Explanations for Complex Changes

The Complex Material sample is built out of a handful of separate Material Functions.

<figure><img src="../../../.gitbook/assets/image (156).png" alt=""><figcaption></figcaption></figure>

* Three **MF\_CompleteLayer** functions, each of which contains the logic for one the three layers processed per-pixel.&#x20;
* An **MF\_ThreeLayerBlend** function, which run a height-blend between the three layers.
* An **MF\_FinalOutput** function, which takes the MaterialAttributes from the blended layers and routes them into a second, cleanly configured, MaterialAttributes pin. This is to avoid unnecessary values being computed.

{% hint style="info" %}
This section is still to be finished.
{% endhint %}



[^1]: Voxel Material Definition Instances - these inherit from VMDs and can have their parameter values changed. They're used as terrain/texture layers.

[^2]: To circumvent the above issue, the individual projections have to be manually blended again using the BlendWeights. This is handled by the `Lerp` nodes underneath the named reroute declaration nodes.
