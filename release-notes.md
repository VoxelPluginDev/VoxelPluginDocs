# Release Notes

<details>

<summary>Upcoming Changes</summary>

## Breaking changes

* Advanced Noise 2D may eventually be deprecated in favor of a more versatile Noise node
  * The Noise node would use Noise Layers with customizable logic

## Other changes

</details>

## 2.0p-335

* [Foliage](knowledgebase/foliage.md) has been refactored into a point-based spawning system, following design patterns similar to Unreal 5.2's PCG
  * This will likely require some changes to the first few nodes of existing foliage graphs
  * Existing per-instance logic will still be valid, only requiring nodes to be replaced with their point-type equivalent
* Materials are entirely redesigned
  * Voxel Material assets are redesigned from scratch, existing ones (which weren't functional) will be invalidated
  * This might affect the way detail textures work
* The Distance pin type has been superceded by the [Surface ](knowledgebase/surfaces-and-materials/)type

**New features:**

* Macro Libraries
* You can now selectively override parameters in instances now, with a tickbox just like in material instances

**Migration notes:**

* Parameter override tickboxes are disabled by default when loading an old asset. You will have to check all the parameters you do want to override.
* The Graph property of Voxel Components is now private. You'll need to use the `Set Graph` function
* `ConstructBrush`/`UpdateBrush`/`DestroyBrush` are now `CreateRuntime`/`DestroyRuntime`. `UpdateBrush` doesn't need to be called anymore when moving a voxel brush.

## 2.0p-320.2

[https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0p-320.2 ](https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0p-320.2)- Released June 9 2023

**New features:**

* Foliage Settings can now be set on the Spawn Foliage node
  * This can be used to set AffectDistanceField, which is false by default
* Foliage Collision is in & can be configured on the Spawn Foliage node&#x20;

**Migration notes:**

* Foliage collision is now enabled by default. Pass a body instance with collision disabled to Spawn Foliage to disable it

#### Bug fixes:

* Fix VOXEL\_DEBUG=1 on clang
* Fix stack overflow when too many brushes are overlapping
* Fix dedicated server crash (`FSlateApplication::Get`)
* Fix crash when making a level instance with a voxel actor in it
* Fix bug with bool when using clang
  * When optimizations are enabled, clang expects a bool to be 0 or 1
  * ISPC doesn't follow such rules, and thus invalid code gen was occurring
* Add `-voxelCheckNaNs` and `voxel.CheckNaNs 1` to check for NaNs

## 2.0p-320.1

[https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0p-320.1](https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0p-320.1) - Released May 22 2023

#### Release notes:

* Add a Migrate to Voxel Scene context menu option to Voxel Meta Graphs
* Fix setting object parameters in C++
* Add back macro graphs

## 2.0p-320

[https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0p-320.0](https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0p-320.0) - Released May 18 2023

#### Migration notes:

* VoxelMetaGraphs are gone. You will have to make a new VoxelScene and copy the graphs over
* Foliage & Brush Instances are their own asset types now. Existing instances will have their parameter reset - you will need to make new assets
* C++: buffers are now using `FVoxelBufferStorage` instead of array. To make a float buffer, use `FVoxelFloatBufferStorage` instead of `TVoxelArray<float>`
* C++: `AVoxelMetaActor` is renamed to `AVoxelSceneActor`&#x20;

#### Release notes:

* Add GetDistanceToCubemapPlanet: checkout the new Mars example!
  * [Broken link](broken-reference "mention")
* Split buffer storage in chunks & add custom allocator
  * Much better performance when using BinnedMalloc2 (ie, packaged dev or shipping)
  * Packaged games should generate 2x as fast
* Disable foliage distance fields
  * This causes huge hitches when generating foliage
  * Foliage should be a lot faster now
  * Will be exposed as a setting in a future release
* Switch pin values to store object as hard references instead of soft references
  * This should fix loading & GC issues we've been seeing
* Add IsGameWorld/IsEditorWorld nodes
* Fix standalone
* Fix VR shaders
* Add CreatePreviewInGame to brush actors, letting you use their preview meshes in game
* Fix node interaction after placing a comment
* Add custom blueprint node QueryVoxelChannel to query any value type
* Add MaxLOD to screen size chunk spawner
* Fix GetGradient costing too much
* Meta Graphs are renamed to Voxel Scenes, you'll have to manually copy them over
* AVoxelMetaActor is renamed to AVoxelSceneActor
* VoxelFoliage is renamed to VoxelSpawner
* Existing brush & foliage instance parameters (ie, assets with a Parent being set) will be invalidated
* Enable Distance Checks is gone: increase the Distance Checks tolerance to achieve the same effect
* Update includes to follow the new 5.2 includes: this significantly improves build times & should speed up any file including a voxel file
* Add Perfect Transitions option to marching cube node: this will query each LOD separately, which can help reduce holes if the distance is different for 2 LODs
* Add blueprint-like advanced display to voxel nodes
* Default to screen size chunk spawner when no chunk spawner is plugged

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
  * [channels.md](knowledgebase/channels.md "mention")
* Add back voxel invokers
  * [navmesh-and-collision.md](knowledgebase/navmesh-and-collision.md "mention")
* Add graph-based foliage
  * Existing foliage clusters & instance assets will be invalidated
  * Foliage collision is not yet supported
  * [foliage.md](knowledgebase/foliage.md "mention")
* Add graph search
  * You can right click any parameter or any node to see where it's used
  * You can also search through all assets by clicking the global search button top right of the search tab
* Add back density canvases
  * [sculpt-volumes.md](knowledgebase/sculpt-volumes.md "mention")
* Add blueprint getter/setters for graph parameters
  * [setting-graph-parameters.md](knowledgebase/blueprints/setting-graph-parameters.md "mention")
* Allow querying Voxel Graphs from blueprints
  * Limited to floats for now
  * [querying-voxel-graphs.md](knowledgebase/blueprints/querying-voxel-graphs.md "mention")
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

