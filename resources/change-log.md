---
description: Changes made for releases up to this point.
cover: ../.gitbook/assets/image (9).png
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

### 2.0p6

{% hint style="info" %}
This release is only available for 5.5. It should also compile on the engine's main branch.
{% endhint %}

This release brings a long list of small but important features and workflow improvements.&#x20;

#### Migration Notes

{% hint style="warning" %}
If moving from an older preview version, update version by version.&#x20;

For example, if moving from 2.0p4, move the project to 2.0p5 first, and only then move it to 2.0p6.

Migration helpers are usually removed after one preview.
{% endhint %}

* Metadata is now asset-based. Existing uses of metadata should be automatically migrated. The auto-created assets can safely be renamed and moved.
* Material Channels (Metadata passed into materials as vertex data) have been removed. Materials will have to be manually updated to use the new `Get Voxel Metadata` node.
* `VoxelStamp` can no longer be used as a Blueprint parent class. Instead, create an actor with a `Voxel(Instanced)Stamp` component
* The Add and Remove blend modes have been removed for Height stamps. Use an override stamp instead.

#### General Improvements

* Preview meshes no longer duplicate when a stamp is duplicated
* Actor names are no longer copied to the Label field when a stamp is duplicated
* Parameter default changes now properly update the Voxel World
* Metadata is now asset-based
* Voxel Graph call stacks have been improved
* Significantly improved MegaMaterial UI
  * By default, a pop-up now asks to add a new material to the MegaMaterial when first used
* Various improvements to in-viewport stamp selection
  * Stamps can now be marked as viewport-unselectable. This can be useful for 'post-process' style Override stamps
* MegaMaterialCache now lives in the Developer folder as it should be per-user
* Added a Voxel dropdown menu with helpful settings
* Added the ability to upload a Bug Report with an asset and its dependencies by right-clicking it in the content browser
* Fixed excessive `VoxelCharacter` inheritance warnings
* Fixed various crashes and back-end issues
  * See full Github changelog for specifics
* Voxel Graphs are now created as a single asset type through a Blueprint-style type picker
* Added a Voxel Stamp drawer (Ctrl-Q to open)
* MaxLOD is now respected
* Various back-end optimizations
  * Improved stamp tree build performance
  * Improved dependency performance
* Added experimental runtime sculpting
* Add WaitForVoxelWorld to Collision Invokers to prevent players falling through the world
  * True by default, does have overhead if you have hundreds of invokers

#### Materials

* Added Layered Materials for automated slope blending
  * This will likely be extended to include other blending factors (like height) in the future
  * PCG/BP queries will currently require the layered material for queried positions, not the material selected by the layered material
* **(Experimental)** Added post-process material function to megamaterials&#x20;
  * This has a range of known issues related to the use of material expressions present in the rest of the MegaMaterial
* MegaMaterials now update without shader recompile when changing float-based parameters
* Lumen MegaMaterials now account for emissive materials
* Metadata is now queried using the `Get Voxel Metadata` node
  * Material channels (vertex data) have been removed
  * Textures are automatically generated to represent any metadata needed for a given chunk
* Fixed `SampleCurve` nodes not working in MegaMaterials
* Non-Nanite MegaMaterials now consider Masked rendering mode (disabled by default)
* Made removing vertices with translucent materials toggleable

#### Voxel Graphs

* Main Graph details panel is now shown when nothing else is
* Added optional graph-controlled blend mode and layer overrides
* Added Sample Stamp node
* Nodes now show their current value when a preview position is selected
* Fixed copy-pasted Make Value and Parameter nodes not retaining their values
* Add Editor Scripting for parameter nodes
  * Use this to control the state of a parameter in the details panel based on other parameters
  * This is likely to be extended in the future
* The `Unpack Materials` node now outputs materials rather than indices
  * This can now easily be used for checking whether a material is present, or even one for another
  * Added a `Pack Materials` node to re-pack edited materials in the same way
* Updated GetGradient so step size is specified on the node
* Added modulo operator for floats
* Added `Get Transform Along Spline` node
  * This doesn't currently respect the spline's tangents, only actual control point transforms
* All function input (sub-)categories called "Advanced" are now collapsed by default

#### PCG

* The PCG integration and dependency tracking has been refactored to be faster and much more consistent/reliable
* Safer workflows for stamp spawners and samplers, which previously risked generation loops:
  * Stamp Spawners now register their stamps immediately, rather than waiting a frame
  * Voxel Samplers now have a Dependency input pin. This can be used to ensure other parts of the graph are completed first
  * Dependency tracking can now be disabled per Voxel-querying node
    * This shouldn't usually be necessary, but can be helpful for specific usecases
  * Generation loops should be automatically caught, with a warning being thrown on detection

#### Platform support

Just like with Preview 5, Mac & iOS are natively supported and a build is available with the Voxel Plugin Installer. This preview also adds back Android support.

#### Blueprint API

{% embed url="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fh87drYhFYUREtE7JAt8V%2Fuploads%2FYfg5ZheY4siuSi8RSr0s%2FUnrealEditor_6LFG5Q7bcd.mp4?alt=media&token=0396f93f-b67e-438c-881f-acbd0524d07d" %}
The first **(Experimental!)** iteration of the runtime edit system in action&#x20;
{% endembed %}

* **(Experimental!)** Added first iteration of [runtime sculpting](../knowledgebase/blueprints/blueprint-api/runtime-edits-and-sculpting.md)
* VoxelStamp can no longer be used as a Blueprint parent class



***

### 2.0p5

{% hint style="danger" %}
This release is **only** available for 5.5. The plugin will not support 5.4 for any future releases.
{% endhint %}

This release brings a multitude of new features alongside significant usability and stability improvements.

One of the big new features is the addition of **Height Spline Graphs** (docs TBD).

The release also adds (**experimental**!) in-editor **sculpting tools** (docs TBD).

{% embed url="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FWdLLHg9aFXUtKtWlKWMP%2Fuploads%2FtLAjxPnDsk1w03ZMC7eP%2F2025-03-04%2019-04-13.mp4?alt=media&token=14bf3ea2-ddd2-4ef0-864e-3f983f1b2617" %}
Blockiness (when enabled on the Voxel World) metadata can be used to make terrain blocky, everywhere or just in some spots.
{% endembed %}

Lastly, we're introducing an (**experimental**!) block rendering solution, as well as flat normals (docs TBD).

As extra worthy mentions, this release:

* Stabilizes the automatic **MegaMaterials** that were initially added in 2.0p4
  * Materials should no longer randomly render black
  * Material-related errors should be visibly raised on-screen
  * Various GPU and render crashes have been fixed
* Adds support for Android and MacOS
  * [Voxel Plugin Installer](https://www.fab.com/listings/d3f93b8f-1718-406a-96ac-f96755678f1e) is now available for MacOS

{% hint style="warning" %}
**Deprecations & Removals:**

* Non-asset **layer and stack** references have been removed.&#x20;
  * If you still need to migrate your layers to assets, upgrade to **2.0p4** to do this first.
* The previously **deprecated Voxel Graphs** are now fully disabled. Graphs built in the deprecated ("Complex") pipeline can still be opened as read-only assets, but they will not be useable anymore.
* **Biome Map** assets have been removed.
{% endhint %}

#### **General Improvements**

{% embed url="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FWdLLHg9aFXUtKtWlKWMP%2Fuploads%2FRjURia0ROovjIE1EKKlD%2F2025-03-04%2022-15-56.mp4?alt=media&token=ce738dfc-801d-462d-add3-9a86e98ab563" %}
Diff-based sculpting allows sculpt data to be retained even when the sculpt stamp is moved, or its surroundings are changed.
{% endembed %}



* Improved Layer and Stack UX
* Improve Voxelized Mesh placement mode (toggled with Alt-V)
* Add world-state stamp debugger

#### Voxel Graphs

<figure><img src="../.gitbook/assets/image (246).png" alt=""><figcaption><p>Height Spline Graphs allow the creation of complex, spline-based terrain modifiers like roads.</p></figcaption></figure>

* BiomeMaps have been deprecated
* Metadata now only has one name field
  * Named metadata pins will need to be reconnected, and their names manually migrated to the Name field. The old pins should still be visible as (red) orphaned pins
* Function nodes will have an orphaned `Run Execution` pin appear. These can be removed by right-clicking the pin
* Improved graph preview UI
  * Nodes now show their current value when a position is selected in the preview

#### PCG

{% hint style="info" %}
Height Spline Stamps can be spawned from PCG using the `Create Voxel Spline`, but parameter overrides are not yet supported. Spline Stamps spawned using the `Voxel Stamp Spawner` node will not function.
{% endhint %}

* VPCG point scatter nodes have been refactored
  * Some manual migration will be required
* Ensure StampSpawner stamps are spawned the moment the node finishes
  * This guarantees stamps will be ready for nodes depending on them that come after
  * To avoid regeneration loops, dependency tracking needs to be optional. This will be added in a future release
*   Remove VPCG point normal getter/setters. Use PointRotation with GetUpVector instead



#### **Blueprint API**

* Add GetProgress to VoxelWorld
* Refactored all stamp-related API
  * Any existing systems using Stamp APIs will need to be manually migrated. See the [this documention page](https://docs.voxelplugin.com/2.0p5/knowledgebase/blueprints/blueprint-api/setting-graph-parameters) for a (very) brief guide to the new API

#### **Back-end**

* Lots of back-end clean-up and optimizations
* Disable progressive refinement when the game thread is stalled
* Don't compute the editor voxel world when in PIE
* Fix various ensures
* Fix various shader compilation errors
* Don't comple ISPC if plugin is in engine
* Fix mobile compile/packaging errors
* Add more test cases



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

