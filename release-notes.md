# Release Notes

<details>

<summary>2.0p-317</summary>

* Add 5.2 compatibility
* Remove 5.0 compatibility
* Remove exec flow from graph: TODO doc link
* Add brush system: TODO doc link
* Add graph-based foliage: TODO doc link
  * Existing foliage clusters & instance assets will be invalidated
* Add UFUNCTION nodes: TODO doc link
* Add blueprint getter/setters for graph parameters
* Remove GPU compute from graphs
  * The current approach was too messy & unusable
  * C++: this removes all buffer views, you can use buffers directly now
* Add smart graph diffing: only values affected by the graph changes are updated
* Add inline macros to graphs
* Remove custom content mapping
  * /VoxelPlugin is now /Voxel, existing content references will be broken
* Start adding new material layer pipeline
  * This is not ready yet, assets will be broken by the next release
* Add back voxel invokers
* Fix transition meshes
* Remove render thread throttle. All updates should feel snappier now, but you might see some frame dips. You can add the throttle back using `voxel.threading.MaxConcurrentRenderTasks 4`

</details>



## 2.0p-304

[https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0-304](https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0-304) - Released January 26 2023

* First tracked release

#### 2.0p-304.1

[https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0-304.1](https://github.com/VoxelPlugin/VoxelPlugin/tree/2.0-304.1) - Released April 5 2023

* Fix navmesh being broken due to missing UpdateBounds

