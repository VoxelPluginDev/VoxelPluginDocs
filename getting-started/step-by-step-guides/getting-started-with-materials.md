---
description: >-
  This guide covers the basics of how to configure materials to work with a
  voxel terrain.
---

# Getting Started with Materials

{% hint style="info" %}
This page is a stub. This means that the information on this topic will be significantly expanded in the future. What is currently on this page is an incomplete picture.
{% endhint %}

The plugin currently does not support any specialized material data framework. Creating something along the lines of 1.2's Multi-Index material framework is a priority.

Until this is done, materials are fairly straight forward; pass layer weights into a material using vertex data or detail textures, sample these in the material and use them as weights for interpolating between parts of a single large material with all layers defined inside it. Using more than a handful of layers in this way will have a very significant impact on GPU performance.

More detail on this topic can be found on the [passing-data-to-materials.md](../../landmass-and-metagraphs/deep-dives-into-metagraphs/passing-data-to-materials.md "mention") page.
