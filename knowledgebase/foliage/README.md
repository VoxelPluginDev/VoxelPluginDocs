---
description: Generating foliage onto an existing surface.
cover: ../../.gitbook/assets/image (216).png
coverY: 66.19718309859152
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Foliage & PCG

{% hint style="info" %}
An [example ](../../getting-started/installing-voxel-content.md)is available on this topic!
{% endhint %}

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption><p>A large world with PCG-based procedural foliage, masked by a voxel stamp (writing metadata which is then used to filter in PCG).</p></figcaption></figure>

The previous Voxel Plugin foliage system has been replaced with a [PCG ](https://dev.epicgames.com/documentation/en-us/unreal-engine/procedural-content-generation-overview)integration. This gives full control over scattering behavior, and allows for spawning actors (including voxel brushes) as well as the usual static meshes.

{% hint style="info" %}
PCG updates from the Voxel World can be paused using the `voxel.DisablePCGRegen 1` console command.
{% endhint %}

With PCG comes the ability to analyze points and do contextual operations on them. This opens the door to things like combining multiple smaller cliff points into a single larger cliff mesh. The logic that would be used for that kind of behavior is generic to any PCG-based system.&#x20;



