# Release Notes

## Upcoming

* Split buffer storage in chunks & add custom allocator
  * Much better performance when using BinnedMalloc2 (ie, packaged dev or shipping)
  * Packaged games should generate 2x as fast
* Disable foliage distance fields
  * This causes huge hitches when generating foliage
  * Foliage should be a lot faster now
  * Will be exposed as a setting in a future release
* Add IsGameWorld/IsEditorWorld nodes
* Fix standalone
* Fix VR shaders
* Add CreatePreviewInGame to brush actors, letting you use their preview meshes in game
* Fix node interaction after placing a comment
* Add custom blueprint node QueryVoxelChannel to query any value type
* Add MaxLOD to screen size chunk spawner
* Add GetDistanceToCubemapPlanet

## 2.0p-317.1

[https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0p-317.1](https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0p-317.1) - Released May 2 2023

* Add Sample Heightmap node
* C++: you can now do `VoxelMetaActor->ParameterCollection.Set("ParameterName", ParameterValue);`

## 2.0p-317

[https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0p-317](https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0p-317) - Released April 26 2023

* Add 5.2 compatibility
* Remove 5.0 compatibility
* Add downloadable example content
  * You can right click the content browser -> Add voxel content
* Add brush/channel system
  * [brush-and-channels.md](basics/brush-and-channels.md "mention")
* Add back voxel invokers
  * [navmesh-and-collision.md](basics/navmesh-and-collision.md "mention")
* Add graph-based foliage
  * Existing foliage clusters & instance assets will be invalidated
  * Foliage collision is not yet supported
  * [foliage.md](basics/foliage.md "mention")
* Add graph search
  * You can right click any parameter or any node to see where it's used
  * You can also search through all assets by clicking the global search button top right of the search tab
* Add back density canvases
  * [density-canvas.md](basics/density-canvas.md "mention")
* Add blueprint getter/setters for graph parameters
  * [setting-graph-parameters.md](basics/blueprints/setting-graph-parameters.md "mention")
* Allow querying Voxel Graphs from blueprints
  * Limited to floats for now
  * [querying-voxel-graphs.md](basics/blueprints/querying-voxel-graphs.md "mention")
* Add UFUNCTION nodes
  * Example here: [https://github.com/VoxelPlugin/VoxelPlugin/blob/2.0p/Source/VoxelGraphNodes/Public/VoxelBasicFunctionLibrary.h](https://github.com/VoxelPlugin/VoxelPlugin/blob/2.0p/Source/VoxelGraphNodes/Public/VoxelBasicFunctionLibrary.h)
* Remove exec flow from graph
* Remove GPU compute from graphs
  * The current approach was too messy & unusable
  * C++: this removes all buffer views, you can use buffers directly now
* Add smart graph diffing: only values affected by the graph changes are updated
* Add inline macros to graphs
  * You can create graph macros using the graph left panel
* Remove custom content mapping
  * /VoxelPlugin is now /Voxel, existing content references will be broken
* Start adding new material layer pipeline
  * This is not ready yet, assets will be broken by the next release
* Fix transition meshes
* Remove render thread throttle. All updates should feel snappier now, but you might see some frame dips. You can add the throttle back using `voxel.threading.MaxConcurrentRenderTasks 4`
* Voxel Meta Actors cannot be moved anymore
  * This means setups such as rotating planets are currently not possible
  * This will be fixed in a later release with the introduction of Voxel Domains
  * If your voxel actor was not at origin, your entire world will be shifted with this update!
* Mac is fully supported (tested on an M1 Mac Mini)

## 2.0p-304

[https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0p-304](https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0p-304) - Released January 26 2023

* First tracked release

#### 2.0p-304.1

[https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0p-304.1](https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0p-304.1) - Released April 5 2023

* Fix navmesh being broken due to missing UpdateBounds

