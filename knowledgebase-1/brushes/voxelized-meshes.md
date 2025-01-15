# Voxelized Meshes

{% hint style="info" %}
An example of how to use static meshes as voxel brushes can be found in the Landmass Brushes [example content](../../getting-started/installing-voxel-content.md).
{% endhint %}

The plugin supports the use of meshes as voxel data. This works by generating a voxelized version of a mesh, which is then stored as a VoxelizedMeshAsset. The static mesh points to this asset, so artists can simply use static mesh references in the level creation process.

Each unique voxelized mesh used in the world will be kept in memory. In other words, using multiple different meshes will use more memory with each added mesh. Because of this, the amount of unique meshes used for brushes is kept relatively low. Using the same mesh multiple times will not take up any additional memory.

{% hint style="warning" %}
There is not currently any way to access information from meshes, such as UVs or vertex colors, as part of the voxelized data.
{% endhint %}

