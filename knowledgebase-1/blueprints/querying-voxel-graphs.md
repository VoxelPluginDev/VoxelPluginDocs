# Querying voxel graphs

You can query any voxel channel using the QueryVoxelChannel node:

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

Currently only float channels are supported.

Multi-queries are preferable in almost every case due to overhead. All graphs registered within bounds are called.
