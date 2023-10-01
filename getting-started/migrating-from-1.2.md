---
description: >-
  Key information and things to consider when moving from Voxel Plugin 1.2 to
  Voxel Plugin 2.0.
---

# Migrating from 1.2

### Key Differences

There are several key differences to consider when migrating from 1.2 to 2.0:

* Assets created in 1.2 will not be useable in 2.0 (such as graphs, materials, and foliage assets).&#x20;
* Voxel-specific functions called on the Voxel World in 1.2 do not exist in 2.0.&#x20;
* The cubic and MagicaVoxel workflows from 1.2 are currently unsupported in 2.0.

{% hint style="danger" %}
Voxel World save files from 1.2 cannot be migrated to 2.0!
{% endhint %}



### Missing or Overhauled Features

(We will add to this list as we determine more features that underwent significant changes or that exist in 1.2 but not 2.0)

* The graph system in 2.0 has been entirely re-written; 1.2 graphs cannot be migrated to 2.0.
* Voxel Spawner Actor / Voxel Foliage Actor have been replaced by the [point-based foliage nodes for Voxel Graphs](../knowledgebase/foliage.md).
* [Materials ](../knowledgebase/surfaces-and-materials/material-definitions/)have been overhauled; the plugin no longer uses Single Index or Multi Index materials. 1.2 materials must be updated to work with 2.0.
* Cubic rendering is not yet supported in 2.0.
* Data Items and Data Assets have been removed and replaced by [Voxel Brushes](../knowledgebase/brushes/).
* Landscape importer (and most other importers) do not currently exist in 2.0.
* Editor tools (for sculpting and painting voxel actors) are not yet available.
* Voxel Physics do not currently exist or have a replacement in 2.0.



### Removed Classes

There are some important core classes that have been changed or removed in 2.0.&#x20;

Any actors that are sub-classes of these classes will need to be removed **before upgrading** your project from 1.2:

* Voxel Spawner Actor & Voxel Foliage Actor (and their spawner configs) should be removed from the project before migrating.

These classes should be updated **after upgrading** your project to 2.0:

* Voxel World has been removed. Any blueprint casts to Voxel World can safely be replaced with casts to Voxel Actor.&#x20;
* Voxel Procedural Mesh Component has been removed. Access to a Voxel Actor's collision will be replaced.



