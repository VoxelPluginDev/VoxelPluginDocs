---
icon: hammer-crash
description: How Voxel Graphs can be called directly from PCG.
---

# Voxel PCG Graphs

To make it easier to synchronize workflows between terrain generation and PCG, Voxel PCG Graphs (VPGs) exist. These are a type of Voxel Graph which can be called directly from a PCG graph using the `Call Voxel PCG Graph` node. Depending on the usecase, VPGs may also help improve foliage generation performance.

VPGs have one Input and one Output, which are of the Point Set type. These pins contain the same data as the point sets passed in/out in PCG. Attributes can be retrieved and set by name - some attributes have default get/set nodes to make things easier.

Parameters can be created and customized as usual. These are shown in the details panel of the `Call Voxel PCG Graph` node for customization. These values cannot be currently turned into a pin, it has to be statically defined. To pass in dynamic data, an attribute has to be used instead.&#x20;
