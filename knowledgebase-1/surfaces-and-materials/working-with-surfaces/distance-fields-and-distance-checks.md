# Distance Fields & Distance Checks

## 1.  Distance Fields

A distance field is a set of data where each the value at any point is the distance to the nearest surface. In our case, each voxel keeps track of the distance to the nearest surface in centimeters. When a given position has a distance of zero, that means the surface is at that position. When the distance is negative, we are inside a surface.

This video about Marching Cubes by Sebastian Lague is a great primer on the approach the plugin uses to generate a mesh from distance fields.

{% embed url="https://www.youtube.com/watch?pp=ygUObWFyY2hpbmcgY3ViZXM=&t=7s&v=M3iI2l0ltbE" %}

Most plugin system assume that a distance is 'perfect', which is to say that at any point, the distance is actually exactly the distance to the surface. In practice, that is often not the case.&#x20;

Small inaccuracies aren't usually an issue, but as the inaccuracy increases, for example when converting heightmaps to distances (which is very inaccurate if slopes are steep, as it only considers the vertical distance, not horizontal), problems will start to appear. For example, the terrain normals may end up not matching the geometry, as they are determined from the distance field, not the generated geometry. Additionally, holes may start to appear in the terrain. This is generally caused by distance checks, and there are ways to compensate for this.

## 2. Distance Checks

Distance Checks are an important optimization step. When the terrain is being generated, large amounts of empty chunks are being created. To avoid wasting time on chunks that won't actually contain a surface, each chunk checks whether it is empty before computing all its values.&#x20;

To do this, the the distance field value in their corners is computed. If this distance is larger than the size of the chunk, there 'cannot' be a surface in the chunk, as for that the distance would have to be smaller than the size of the chunk. This quickly falls apart if the distance field is not completely accurate.

{% hint style="info" %}
Nodes that use distance checks (such as the **Generate Marching Cube Surface** node) have a "Distance Checks Tolerance" pin.&#x20;

This is the chunk size which the corners check against is multiplied with this. Increasing this value from 1 will therefore generate more potentially empty chunks, but it will prevent chunks with inaccurate distances at their corners from being skipped when they do contain a surface.

Keep this value as low as possible -  increasing it will increase time spent generating empty chunks, which can blow up generation times.
{% endhint %}

&#x20;

<div align="left">

<figure><img src="../../../.gitbook/assets/DistanceChecks (3).png" alt="" width="375"><figcaption></figcaption></figure>

</div>

In the illustration above, a chunk (2D for simplicity's sake) with a size of 3200cm is running its distance checks, computing the distance at each corner. Distance A, C, and D read back as 1000cm, which could be inside the chunk. Distance B, though, reads back as 5000cm. The distance from Corner B to the farthest point in the chunk (Corner C) is 4525.5cm.&#x20;

Distance B suggests that there cannot be a surface because of corner B's distance. Its distance of 5000cm is larger than the distance to the farthest point within this chunk (4525cm away).

Distance checks are, however, conservative; if one corner passes, the chunk is processed. In this case, distance A, C and D have a distance smaller than the size of the chunk. Because of this, the chunk is computed, regardless of distance B. The chunk would only be skipped if distance A, B, C, and D all reported a distance larger than the size of the chunk.&#x20;
