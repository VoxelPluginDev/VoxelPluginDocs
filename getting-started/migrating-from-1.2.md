---
description: >-
  Key information and things to consider when moving from Voxel Plugin Legacy
  (1.2) to Voxel Plugin 2.
---

# Migrating from Voxel Plugin Legacy

### Key Differences

There are several key differences to consider when migrating from Voxel Plugin Legacy (1.2) to VP2:

* Assets created in VPL will not be useable in VP2 (such as graphs, materials, and foliage assets).&#x20;
* Voxel-specific functions called on the Voxel World in VPL do not exist in VP2.&#x20;
* The cubic and MagicaVoxel workflows from VPL are currently unsupported in VP2.

{% hint style="danger" %}
Voxel World save files from 1.2 cannot be migrated to 2.0!
{% endhint %}



### Missing or Overhauled Features

(We will add to this list as we determine more features that underwent significant changes or that exist in 1.2 but not VP2)

* The graph system in VP2 has been entirely re-written; VPL graphs cannot be migrated to VP2.
* Voxel Spawner Actor / Voxel Foliage Actor have been replaced by a [PCG ](https://dev.epicgames.com/documentation/en-us/unreal-engine/procedural-content-generation-overview)integration.
* Materials have been overhauled; the plugin no longer uses Single Index or Multi Index materials. VPL materials must be reworked to work with VP2.
* Cubic rendering is not yet supported in VP2.
* Data Items and Data Assets have been removed and replaced by Voxel Stamps
* Landscape importer (and most other importers) do not currently exist in VP2.
* Editor tools (for sculpting and painting voxel actors) are not yet available.
* Voxel Physics do not currently exist or have a replacement in VP2.



### Removed Classes

There are some important core classes that have been changed or removed in VP2.&#x20;

Any actors that are sub-classes of these classes will need to be removed **before upgrading** your project from VPL:

* Voxel Spawner Actor & Voxel Foliage Actor (and their spawner configs) should be removed from the project before migrating.

These classes should be updated **after upgrading** your project to VP2:

* Voxel World has been reimplemented. Any project-level Blueprint/C++ references to Voxel Worlds likely need to be reimplemented.&#x20;
* Voxel Procedural Mesh Component has been removed. Access to a Voxel Actor's collision will be replaced.
