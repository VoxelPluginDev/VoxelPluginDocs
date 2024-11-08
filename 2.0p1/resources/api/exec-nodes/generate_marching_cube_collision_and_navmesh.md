# Generate Marching Cube Collision & Navmesh

<div align="left" data-full-width="false">

<figure><img src="../../../.gitbook/assets/Generate_Marching_Cube_Collision_&#x26;_Navmesh.png" alt=""><figcaption></figcaption></figure>

</div>

Voxel Marching Cube Collision Exec Node

## Inputs

<table><thead><tr><th width="170">Name</th><th>Description</th></tr></thead><tbody><tr><td>Enable Node</td><td>If false, the node will never be executed</td></tr><tr><td>Surface</td><td>Surface</td></tr><tr><td>Body Instance</td><td>BodyInstance</td></tr><tr><td>Invoker Channel</td><td>InvokerChannel</td></tr><tr><td>Voxel Size</td><td>VoxelSize</td></tr><tr><td>Compute Collision</td><td>ComputeCollision</td></tr><tr><td>Compute Navmesh</td><td>ComputeNavmesh</td></tr><tr><td>Physical Material</td><td>PhysicalMaterial</td></tr><tr><td>Distance Checks Tolerance</td><td>DistanceChecksTolerance</td></tr><tr><td>Chunk Size</td><td>ChunkSize</td></tr><tr><td>Priority Offset</td><td>Priority offset, added to the task distance from camera Closest tasks are computed first, so set this to a very low value (eg, -1000000) if you want it to be computed first</td></tr></tbody></table>

## Outputs

<table><thead><tr><th width="170">Name</th><th>Description</th></tr></thead><tbody><tr><td>Exec</td><td>If not connected, will be executed automatically</td></tr></tbody></table>
