# Using PCG on Voxel Terrains

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption><p>A large world with PCG-based procedural foliage, masked by a voxel stamp (writing metadata which is then used to filter in PCG).</p></figcaption></figure>

## Voxel Sampler & Projection

The `Voxel Sampler` and `Voxel Projection` node (can) produce a similar result, but they're doing (and often used for) very different things.

### Voxel Sampler

When looking to scatter points on a voxel terrain, the `Voxel Sampler` node is usually the way to go. It is in most cases relatively fast compared to a lot of other operations you might be doing in an average PCG graph.&#x20;

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption><p>A Voxel Sampler, and its points being filtered based on material weights from an attribute.</p></figcaption></figure>

&#x20;The sampler effectively meshes a chunk, with the cell size being the resolution. It spits out a point at each cell.

If you are using the Voxel Sampler, it's worth knowing that:

* The amount of points initially generated (and therefore the cost) is determined by the Cell Size.
* Points are then filtered out until the density matches your Points Per Square Meter (PPSM) setting.

This means that lowering the PPSM setting doesn't actually make the sampler cheaper - it's just there to tweak the visual result. If you want to keep your sampler as cheap as possible, set your PPSM to a high value (say 100, because you'll likely never want >100 points per square meter). From there, use the cell size to control the density of your points (lower cell size = more points).

It's theoretically possible to spawns points without the sampler, and instead spawn a grid of points manually and project them onto the terrain using the `Voxel Projection` node. The cost of the projection will likely be higher than the cost of just using the `Voxel Sampler` though, due to the large amount of steps needed to get the points to stick to the terrain.

{% hint style="info" %}
It's very difficult to get a consistent point density on your terrain without using the Voxel Sampler node.

The only real alternative is to use a PCG node to reduce overlapping points, which is itself very expensive.
{% endhint %}

By default, the sampler rotates points to be aligned to the terrain's surface, and it writes material weights into the point attributes. It can optionally be configured from the details panel to query (and write into the point attributes) metadata

### Voxel Projection

The projector takes input points, evaluates the distance field (the shape of the terrain) and its normal for each of those points, and moves each point by distance x gradient x speed (configurable in the details panel). It will continue doing that repeatedly for each point, until each point a) reaches the maximum step count, or b) its distance is less than the 'tolerance' in the details panel.&#x20;

This means the projection node is sampling the DF many times for every point. As such, the cost of this node scales based on the amount of steps. The farther your points originally were from the surface (and the less precise your distance field), the more likely you'll need a lot of steps (= DF samples) per point to get them to actually sit on the surface.

{% hint style="info" %}
The 'stepping' operation done by the `Voxel Projection` node is called Raymarching.&#x20;

A node that does this same thing also exists in Voxel PCG Graphs, where it's called `Raymarch Layer Distance Field`.
{% endhint %}

By default, the projection node does not write modify any point attributes other than position. It can optionally query (and write into the point attributes) rotation, material weights and metadata values. This can be configured in the details panel.&#x20;

### Voxel PCG Graphs

VPCGs can be useful for simplifying PCG-related workflows, but they can also provide substantial speed-ups for math-heavy graphs. Read more about them [here](voxel-pcg-graphs.md).&#x20;

### Querying Arbitrary Voxel Data

Sometimes you may want to query data from the voxel terrain on points that aren't (and shouldn't be) spawned on or projected onto the voxel terrain. For this, the `Voxel Query` node can be used.

This node can query any voxel layer, and will write the information from it into point attributes (optionally including materials and metadata). It will not affect any of the points' other attributes.





