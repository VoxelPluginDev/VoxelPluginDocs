---
description: How materials can be blended smoothly, as opposed to the default height blend.
---

# Smooth (Alpha) Blends

{% hint style="warning" %}
Nanite must be disabled on the Voxel World to use smooth blends. \
This also means displacement is not supported when using smooth blends.
{% endhint %}

To implement alpha blends, Nanite needs to be disabled on the Voxel World. This will force the terrain to render with the MegaMaterial's base material.

In the MegaMaterial's base material, the `Voxel Fallback Weights` can be used to directly interface with the current pixel's layer IDs and weights.

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption><p>The <code>Voxel Fallback Weights</code> node returns the current pixel's layer data.</p></figcaption></figure>

Every Voxel Parameter node has a `Layer` input pin. To set up alpha blending, the material logic needs to be copy-pasted three times, with each copy using one of the layers. At the end of each layer's logic, multiply the values by that layer's weight, and then add the three layers' values together.

<div><figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Three layers blended together.</p></figcaption></figure> <figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure></div>

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption><p>The above logic used on the Nanite Materials example.</p></figcaption></figure>
