---
description: >-
  Material Definitions are similar to Unreal's Landscape Layers, but for
  voxel-based terrains.
---

# Material Definitions

## 1. The Purpose of Material Definitions&#x20;

Materials can be applied to voxel terrains using the Material Definition workflow. &#x20;

With the Material Definition workflow, it is possible to use hundreds of material layers on a voxel terrain. Within voxel graphs, you can assign different layers based on noise or other values. The three highest-weight layers will then be automatically sent to and processed by the material using some voxel-specific nodes. Each layer runs the same logic. This means that a snow layer will have to use the same instructions as a rock layer - it can only change the float and textures values used.

{% hint style="warning" %}
Landscape materials (found, for example, on the Unreal Engine Marketplace) can not be used with voxel terrains. Landscape materials are built specifically for native Landscapes, which use their own specialized nodes.&#x20;

These nodes are incompatible with Voxel Plugin, so Landscape layers will not function. Additionally, most of these materials are not triplanar, leading to texture stretching on slopes.

We will be working with creators to create Voxel Plugin compatible versions of asset packs.&#x20;
{% endhint %}

## 2. Workflow Overview

Getting a terrain set up with multiple material layers, you will end up with the following assets:

* One 'master' Material Definition (MD), where you configure what parameters layers will have.

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p>A material definition with a handful parameters, and a correctly configured material assigned.</p></figcaption></figure>

* One 'master' material, which is used both for the preview and the actual terrain. This material uses voxel helper nodes, and will reference the Material Definition.

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption><p>A simple color material, manually blending the result of each of the three active layers.</p></figcaption></figure>

* As many Material Definition Instances (MDI) as you want to have layers. There isn't a practical limit to the amount of layers you can use.&#x20;

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption><p>As many </p></figcaption></figure>

If you want to simply add a new layer, you only have to create a new MDI. This MDI can then immediately be used in voxel graphs (using the [Set Surface Material ](../../../resources/api/surface/set\_surface\_material.md)node), without needing any further set-up. The voxel graph will automatically pass the needed information to the material.

The 'master' Material Definition and 'master' material only need to be worked on if you want to make changes to the terrain's material logic. They do not need to be edited in order to add more layers.

More detailed information about the inner workings of this system can be found on the [technical-details.md](technical-details.md "mention") and [detail-textures.md](detail-textures.md "mention") pages.
