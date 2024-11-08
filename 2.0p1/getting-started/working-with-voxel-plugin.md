---
description: >-
  What Voxel Plugin does and doesn't do, and how the different systems interact
  with each other.
---

# Working with Voxel Plugin

Voxel Plugin 2 specifically focuses on the creation and generation of interactive terrains.

#### World Creation & Graphs

Worlds are authored by adding 'stamps' to the world. Stamps can generate height (2D, top-down) or volume (3D) data. Some stamps are driven by static data, like Heightmap stamps, while other stamps generate their data procedurally using Voxel Graphs.&#x20;

{% hint style="info" %}
All procedural generation in Voxel Plugin is deterministic, not random. This means that as long as the seeds used are the same, the plugin will always generate the same world.
{% endhint %}

World generation is always done on the fly. There is not currently any kind of support for static/pre-baked data.&#x20;

#### Rendering & Materials

The plugin targets Nanite as main rendering pipeline, and comes with an [advanced material framework](../knowledgebase-1/materials.md) for that purpose. This pipeline can render any material anywhere. Non-Nanite rendering is supported, but does not support any material; it uses a single material for the entire terrain, similar to a stock Unreal Landscape.

Tessellation and displacement are supported as part of the Nanite pipeline.

#### Foliage

Foliage generation is no longer handled by Voxel Plugin directly. Instead, the plugin has a strong integration with Unreal Engine's PCG framework. Voxel terrains (and their metadata) can be queried from PCG. Additionally, a PCG graph can call a Voxel Graph to off-load complex math or noise-based operations. This way, Voxel Graphs can be used as an extension of PCG.&#x20;

{% hint style="info" %}
Foliage interaction is not handled by Voxel Plugin, and instead needs to be implemented through PCG.
{% endhint %}



