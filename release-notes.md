# Release Notes

<details>

<summary>Upcoming Changes</summary>

## Breaking changes

* Advanced Noise 2D may eventually be deprecated in favor of a more versatile Noise node
  * The Noise node would use Noise Layers with customizable logic

</details>

## 2.0p-339

[https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0p-339.0](https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0p-339.0) - Released October 1 2023

This release drops 5.1 support and adds 5.3 support.

**New features:**

* New material pipeline
  * The new voxel material pipeline is now ready, letting you assign materials per voxel
  * See [Material Definitions](knowledgebase/surfaces-and-materials/material-definitions/)
* New foliage pipeline
  * Foliage has been refactored into a point-based spawning system, following design patterns similar to Unreal's PCG
  * Existing foliage nodes will be broken, you will need to manually migrate to the new system
  * This pipeline supports invoker-based collision and has been stress tested in large projects
    * Collision is only generated near the players to reduce the load on the Chaos scene and avoid hitching
  * Each foliage is assigned a unique Point Id that can be used as globally unique identifier
  * The pipeline supports attribute overrides: you can use MakePointHandle to get a point handle from a hit result. This handle is replicable & can be sent through RPCs
    * You can then use SetAttribute to change point attributes
    * Rendering will be updated automatically on the fly
    * Typical use cases: having a HasBerries attribute for a bush
    * New instances will be detected & rendered through an instanced static mesh component to not trigger a full rebuild of the hierarchical instanced mesh component tree
  * Here is a simple graph:
  * See [Foliage](knowledgebase/foliage.md)
* Fast curve sampling: sampling curves now uses SIMD logic, making it 10x faster than previous curve sampling logic
  * Fast curves doesn't support advanced curve features such as weighted tangents.
  * You can now also define curves inline without having to make a new asset for each curve.
* Parameter override tickboxes
  * You can now selectively override parameters in instances, with a tickbox similar to material instances
  * Existing parameters will be automatically enabled.
* New exec flow
  * All exec nodes (red nodes) now have an optional execution pin
  * Exec nodes in the main graph that have their exec pin unplugged will be executed automatically
  * You can use the Execute node to call exec nodes that are located in macros
  * You can use the Select node on execution wires to selectively enable/disable parts of your graphs
* Detail texture improvements
  * Detail textures are now pooled into large textures
    * This reduces render thread cost when voxel chunks are updated
    * Use `stat voxelmemory` to see detail texture memory usage
  * You can easily sample detail textures in materials using the new SampleVoxelFloatDetailTexture node
  * You can easily assign detail textures in graphs using SetSurfaceAttribute and BindFloatAttributeDetailTexture
* Graph preview is fixed
  * Use R on any node to preview its output
* New graph debug
  * Press D to debug a node
  * For marching cube: will output the node value as color on the mesh itself
  * For foliage: will preview the points at that stage
* Distance Field & Lumen support
  * Lumen & distance field are now supported
  * Tick the "Generate Distance Fields" checkbox on the Generate Marching Cube Surface node to enable Lumen
  * Be mindful that this will use GPU memory
  * We recommend doing something like "LOD == 0" as input to the checkbox to only enable Lumen on nearby chunks
  * There might still be issues with Lumen in packaged games - we will iron this out in the future
* Density canvases are gone and replaced by a unified sculpt pipeline
  * All voxel actors now hold a Sculpt Storage and can be sculpted into
  * You can sculpt using the new Voxel Sculpt editor mode
  * You can also sculpt at runtime using the Apply Sculpt node
    * This currently requires spawning another Voxel Actor to handle the sculpting
    * We will add easier to use nodes down the road
  * To enable sculpting in your graph, use the Set Sculpt Source and Get Sculpt Surface nodes
  * Sculpt tools are defined through graph. Here is a simple Sphere Sculpt graph. We will provide examples with sculpt graphs in the future.
* Voxel Actors can now be moved and fully support actor transforms
  * Brush transforms are detected relative to the actor they affect
  * Typically, if you have a planet with brushes attached to it, moving the planet will not trigger any refresh as the brush transforms relative to the planet will not change
* Voxel Graphs now support thumbnails
  * You need to tick EnableThumbnails in your graph settings
  * Note that thumbnails might cause additional crashes
  * Use with CreatePreviewMesh
* LLM memory tracking & Insights memory tracking are now fully supported
  * We did a full pass on memory allocations and fixed several leaks in the plugin
* Recursive graph parameters
  * You can now have foliage recursively call on itself
  * This is useful for clustering
  * Detail panels are generated on-demand
* Array support in graphs
  * You can now use arrays in voxel graphs
  * This is currently used by the Random Select node
* Graph stats
  * Graphs now have per-node stats and per-exec node stats
  * Exec node stats are great to easily see which parts of your graph are taking time
* New graph compilation pipeline
  * Graph compilation has been fully refactored to be more stable
  * You can now bulk compile all voxel graphs in your project using the Compile All button. It will open all graphs that failed to compile.
  * Graphs are now re-compiled on cook and graph errors are logged as warnings to avoid cook failures
* Optimized voxel invokers
  * Only sphere invokers are supported now
  * The system is designed to smoothly handle up to 500 invokers in a scene
* New performance monitoring
  * If the average framerate of the last 128 frames is below 8fps, all voxel runtimes will be destroyed
  * This is useful to avoid freezes when using incorrect foliage spawning settings as it's very easy to generate too many instances
  * This can be disabled in project settings
* New blueprint editor utility functions to create voxel graph instances from existing actors

Technical notes:

* New task bypass option to make it easier to debug voxel task: pass -VoxelBypassTaskQueue or use voxel.BypassTaskQueue
* VOXEL\_ON\_COMPLETE doesn't take a Thread anymore. Use VOXEL\_ON\_COMPLETE\_GAMETHREAD to run on the game thread
* SceneNode are now ExecNodes
* You can use -checkVoxelNaNs or voxel.CheckNaNs to raise ensures whenever a NaN is generated
* DEFINE\_VOXEL\_NODE is now DEFINE\_VOXEL\_NODE\_COMPUTE
* On runtime shutdown, the game thread will stall until all voxel tasks are completed or cancelled.  This shouldn't create major hitches and will help having cleaner logic in subsystems.
* The move to Channel Registry assets means we need to wait for them to be loaded before creating any voxel runtime. See `FVoxelRuntime::CanBeCreated`
* Dense/morton-order position queries are now gone as using them to computed derivatives proved inaccurate
* Foliage FVoxelPointId are technically only unique within a single foliage chunk. FVoxelPointHandle is globally unique.
* You can now use the `ConstantPin` tag in exec node pins. This will compute the pin value before the exec node runtime is created and will recreate the runtime on value change. Use `GetConstantPin(Node.PinName)` to get the pin value in the exec runtime.
  * This replaces DECLARE\_VOXEL\_PIN\_VALUES which is now removed
* Any allocation done inside a `VOXEL_FUNCTION_COUNTER` will have the voxel LLM tag
* The rotator pin type has been removed and Quaternion pins now break into euler angles instead
* You can now use the voxel.get and voxel.set console commands to easily set voxel parameters at runtime
* All voxel singletons now inherit from FVoxelSingleton and are destroyed on shutdown
  * This helps with memory leak detection as we can now ensure that all allocations made through the voxel allocator are freed on shutdown
  * This might cause crashes if you try to access any voxel singleton after the voxel module is unloaded (typically through an async task)
* FVoxelTicker is now manually registering instances and they are all ticked together in FVoxelTickerManager::Tick
* This release includes multiple bug fixes for running Unreal on a Linux system with Vulkan
* `VOXEL_CONST_CAST` is now `ConstCast`
* `voxel.StartInsights` and `voxel.StopInsights` can be used to easily start/stop an insights capture with the right settings.

**Migration notes:**

* You can now use the Compile All button in graphs to easily see graphs with errors
* Exposed Data nodes are gone - please use Register to Channel/Query Channel instead
* Voxel Scene, Voxel Brush and Voxel Foliage graphs are now all Voxel Graphs
  * They will be automatically migrated
  * Voxel Meta Actor and Voxel Brush Actor are now Voxel Actor
* The Distance pin type is gone and is replaced by Surfaces
  * Surfaces are a high-level way of defining 3D shapes
  * Make Height Distance is now Make Height Surface
  * To create a surface from a SDF, use Create Volumetric Surface
  * See [Working with Surfaces](knowledgebase/surfaces-and-materials/working-with-surfaces/)
* Voxel Channel are now project settings and can also be defined in Channel Registry assets. Existing channels & their references will be automatically migrated to the new system.
* Foliage nodes are broken and will need to be migrated to the new system
* Voxelized Mesh Assets will have their VoxelSize reset to 20cm
  * This is due to the removal of the project settings BaseVoxelSize
* Physics Scene are removed, you can now directly use the Generate Marching Cube Collision & Navmesh node
* The Graph property of Voxel Components is now private. You'll need to use the `Set Graph` function
* `ConstructBrush`/`UpdateBrush`/`DestroyBrush` are now `CreateRuntime`/`DestroyRuntime`. `UpdateBrush` doesn't need to be called anymore when moving a voxel brush.
* Local Variables must always have a declaration now. Existing local variables without a declaration will have one added on load at the center of the graph.
* The `Voxel/MaterialFunctions` content folder has been entirely removed. If you are missing material functions, copy them over from a previous voxel release.&#x20;

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

