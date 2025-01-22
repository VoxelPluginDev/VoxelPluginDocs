---
description: How Voxel Graphs can be called directly from PCG.
---

# Voxel PCG Graphs

To make it easier to synchronize workflows between terrain generation and PCG, Voxel PCG Graphs (VPGs) exist. These are a type of Voxel Graph which can be called directly from a PCG graph using the `Call Voxel PCG Graph` node. Depending on the usecase, VPGs may also help improve foliage generation performance.

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption><p>A Voxel PCG Graph (VPG) filtering the input points from PCG based on a point attribute.</p></figcaption></figure>

VPGs have one Input, and potentially multiple Outputs, which are of the Point Set type. On the PCG-side, these are seen as standard point collections, and both hold the same data. Attributes can be read and set by name - some attributes have default get/set nodes to make things easier.

Parameters can be created and customized as usual. They're shown in the details panel of the `Call Voxel PCG Graph` node, so their values can be changed per call. It's not currently possible to set VPG parameters using a pin from PCG. To pass in dynamic data, use PCG's standard point attributes.

### Performance Considerations

Aside from workflow convenience, you may want to move (parts of) your PCG graph to VPCG instead. If a PCG graph contains lots of nodes that each do small operations, there may well be performance gains in moving it to a VPCG, as the overhead per-node is lower in Voxel Graphs than in PCG.

If you do use VPCGs, it's important to consider that while the operations in the graph are usually cheaper, initially _calling_ a VPCG has overhead. This overhead is based on the amount of points being passed into it. This means that it's usually a good idea to put as much logic as possible into _one_ big VPCG, rather than having multiple less-complex VPCG calls in a row. Calling multiple less-complex graphs would incur that initial call overhead multiple times.

{% hint style="info" %}
Large VPCGs can be hard to maintain. Voxel Graph Function Libraries can be used to deal with this.

Create re-usable logic blocks in your function libraries to keep things easy to maintain as you build out more complex VPCGs.
{% endhint %}
