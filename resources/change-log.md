---
description: Changes made for releases up to this point.
cover: ../.gitbook/assets/image (6).png
coverY: 65
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

# Change Log

### 2.0p4

This release brings mostly back-end improvements like the move from our own threadpool to Unreal's task system, the foundation of the new sculpting system (not yet usable in this release) and layers and stacks now being asset-based. \
It also introduces **automatic MegaMaterials**, which lets you use any material that's set up for Nanite displacement as an automatically displaced and height-blended material on voxel terrains.&#x20;

Unrelated to this release, but worth mentioning, is that we've spun up a new [public issue tracker](https://voxelplugin.youtrack.cloud/dashboard?id=208-1), where we track bugs, crashes and (small) features we're planning to add.&#x20;

**General Improvements**

* Remove voxel threads and move to the Unreal task system
  * This should help avoid scheduling conflicts between voxel and unreal threads
  * Voxel tasks are scheduled as BackgroundLow, so they shouldn't block the game or render thread in any way
  * `voxel.SetNumWorkerThreads` can be used to lower the amount of worker threads to simulate lower-end hardware. Be mindful that this now affects the entire engine's thread pool, not just the voxel threads
* **Layers** and **Stacks** are now asset-based, instead of being defined in the project settings&#x20;
* Allow for removing parts of the terrain entirely by assigning translucent materials
* Fix override blend mode on mesh stamps
* Disable bulk data in StampComponents to fix packaged game crash
* Fix issues with applying shader hooks
* Fix tooltips
* Fix Mac compilation

#### **Stamps**

* Add static metadata to height and volume graph stamps
* Add material overrides to placed heightmap stamps

#### **PCG**

* Fix 2D PCG Projection node metadata values with Update Rotation

#### **Materials**

* **Add automatic MegaMaterials**
* Add tangents (to allow use of tangent space transforms)
* Remove support for baking WPO to collision

#### **Blueprint API**

* Add Async Query Layer nodes
* Update Sync/Async Export Data to Render Target nodes, to support multiple channels

#### **Back-end**

* Add modifiers as sculpting framework
  * These will be the backbone for the new sculpting system, which is partially implemented but not yet usable
* Refactor Get/SetVoxelGraphParameter
* Fix stamp serialization
* Fix various warnings
* Refactor voxel futures
* Remove outdated ensures
* Add automated tests
* Remove
  * VOXEL\_ALLOW\_MALLOC\_SCOPE
  * VOXEL\_ENQUEUE\_RENDER\_COMMAND
  * TVoxelUniquePtr
  * MakeVoxelShared
  * MakeVoxelShareable
  * FVoxelMemory
* Fix shutdown crash
* Add VOXEL\_DEV\_WORKFLOW
* Improve DEFINE\_PRIVATE\_ACCESS
* Fix deadlock in FVoxelMaterialManager::FindOrAddMaterialIndex
* Fix segmented bitmask enum undetermined value
* Fix StampBehavior enum
* Exclude RF\_MirroredGarbage in ForEachObjectOfClass





### 2.0p2

This release brings substantial UX and performance improvements. The biggest new features are P**rogressive Refinement** and the **Instanced Stamp Component** (and its accompanying `Spawn Voxel Stamps` PCG node).

**General Improvements**

{% embed url="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FA44hxjxX1U0Id0HHhhkd%2Fuploads%2FzHTX9gNSYUraoS7ZAR3S%2FUnrealEditor_nP7P52h3JX.mp4?alt=media&token=bba50d65-d54d-44c0-aa72-55738195121b" %}
Progressive refinement keeps updates snappy when making large changes. \
It temporarily drops the terrain's resolution and slowly increases it again.
{% endembed %}

* Add progressive LOD refinement
  * Change LODs to be set by Quality (higher is better but slower) instead of Screen Size
  * Quality is set using a range slider, with progressive refinement starting at Min and stopping at Max
  * Add separate LOD settings for Editor and Game worlds
* Improve Stack and Layer picker widgets across the plugin
* Add `Wait For Lumen` toggle to decouple Lumen Scene updates from mesh updates
* Add `Wait On BeginPlay` toggle to stall the game until the voxel world is done generating
* Add automatic in-editor thread allocation
* Move stacks to be named, and defined in project settings
* Substantially speed up navigation and collision generation by adding dedicated meshers (>3.5x)
* Substantially improve details panel performance when many stamps are selected&#x20;
* Add Nanite Component settings (includes VSM Invalidation Behavior, which is now set to Rigid by default to prevent VSM invalidation)
* Add LOD setting to all Query nodes

#### **Voxel Graphs**

* Add Query Height/Distance/Material/Metadata nodes
* Add RaymarchLayerDistanceField node (VPG only), similar to PCG's Projection nodes
* Fix suggestions in the node search menu in 5.5
* Add MakeRotationFromX/Y nodes
* Re-add support for GetLOD
* Add UnpackMaterial node for retrieving material weights
* Add GetPreviousMaterial and GetPreviousMetadata nodes
  * Add IsValid output pin to GetPreviousHeight, GetPreviousDistance and add IsFinite node
* Add more keywords to various nodes for better searchability
* Add double positions to graphs

#### **Stamps**

* Add [Instanced Stamp Component](../knowledgebase/working-with-stamps/instanced-stamps.md)
* Improve stamp details panel UX
  * Add customization for blend mode
  * Update property order to make more sense
* Add LODRange to control which LODs are affected by any given stamp
* Update and improve Voxelized Mesh Asset preview
  * Fix endless compute loops in Voxelized Mesh Assets
* Allow drag-and-dropping Voxelized Mesh assets into the world
* Add stamp placement mode where static meshes dragged into the world are automatically placed as mesh stamps
  * This mode is activated by pressing Alt-V
* Add automatic stamp actor labeling to automatically renames stamp actors when their configuration changes
  * &#x20;This functionality can be tweaked or disabled entirely from the Project Settings

#### **PCG**

{% embed url="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FA44hxjxX1U0Id0HHhhkd%2Fuploads%2FExtDOPzxb2sdsorsBQ7F%2F2024-12-19%2017-34-47.mp4?alt=media&token=980f4779-7183-4b2e-97ce-6bacddd8ff2b" %}
A large amount of volume stamps being (de)spawned using a Voxel Stamp Spawner, creating Instanced Stamp Components
{% endembed %}

* Improve Call Voxel Graph
  * Use the graph name as node title
  * Expose graph as pin
  * Allow to have multiple point set outputs
  * Refresh PCG graph on Voxel PCG Graph change
* Add Voxel Query node for querying arbitrary layer data
* Add Voxel Stamp Spawner node, which spawns stamps using an [Instanced Stamp Component](../knowledgebase/working-with-stamps/instanced-stamps.md)
* Add support for updating rotation when projecting to Height layer

#### **Materials**

* Add automatic conversion between G8 and G16 compression
  * This means MegaMaterial compression type errors are no longer thrown when all textures are set to Grayscale compression&#x20;
* Fix Marching Cube shader hook on 5.5

#### **Blueprint API**

* Add Query Voxel Layer nodes
* Add Stamp configuration nodes, allowing data source, layers, smoothness, etc. to be modified
* Allow VoxelStampComponent access in blueprints

#### **Back-end**

* Rename VoxelGraphCore to VoxelGraph
* Fix CoarseStreaming Nanite crash
* Fix FVoxelTextureBufferPool::ProcessUploadsImpl\_AnyThread
* Fix seed migration
* Unify VisibilityCollision, Nanite and Mesh rendering into one subsystem
* Rename VoxelRenderChunkTreeSubsystem to VoxelRenderSubsystem
* Add voxel.MaxChunksToProcessPerPass
* Add ShouldGenerateChildren
* Fix promise tracking on Linux
* Add FVoxelUtilities::ResetPreviousLocalToWorld for static meshes
* Switch to FForkProcessHelper::CreateForkableThread to support forking on Linux
* Refactor FVoxelStampQueryParameter
* Return infinite bounds if intersecting
* Check IsLoadingAssets in stamp component
* Make UVoxelMeshStampPreviewComponent transient
* Cleanup StampComponent child components
* Fix UVoxelVoxelizedMeshAssetUserData::GetAsset when asset has invalid tags
* Fix UVoxelMegaMaterial::PostLoad
* Cleanup task dispatchers and add voxel.VerboseTaskCount
* Remove voxel.DebugNaniteMaterials to fix materials when Substrate is enabled
* Add customizations for stamp blend modes
* Fix height stamps being offset
* Fix invalid reserve in FVoxelRenderSubsystem
* Remove FVoxelStampArray
* Add FVoxelLayerRef
* Fix invalidation issue
* Move query code to FVoxelQuery
* Fix MipData crash
* Fix DumpToLog
* Fix PreviousChunkKeyToNavigationMesh not being cleared
* Fix voxel tickers not ticking during loading screen
* Remove VoxelActorBase and disable VoxelWorld tick by default
* Move components to voxel runtime
* Add SegmentedEnumCustomization and SVoxelSegmentedControl
* Add framework for automated testing
* Add ForEachAssetDataOfClass&#x20;
* Add FVoxelTestManager
* Execute Then\_GameThread inline if possible
* Add implicit TVoxelFuture casting
* Add geometry utilities
* Add GetEnumDisplayName
* Fix DEFINE\_VOXEL\_COUNTER in namespaces on Mac
* Allow having TVoxelSet
* Faster zip writer
* Increase log verbosity
* Optimize FVoxelMesher::CreateMesh
* Fix FVoxelInvokerCollisionSubsystem::Compute not being parallel
* Add heightmap weights edit support
* Add FVoxelTransformRef::Make constant support
* Remove FVoxelTransformRef from stamp components
  * Change graph stamps LocalToWorld to not use component transform
* Optimize bulk stamp invalidation
* Rename AABBTree functions
* Fix UVoxelStampComponent::FindComponent
* Fix expand structs button
* Fix SegmentedEnumCustomization perf
* Update VoxelLayerRef customization
* Update LayerName and LayerRef editors

