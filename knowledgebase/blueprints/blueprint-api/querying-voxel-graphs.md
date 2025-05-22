---
description: >-
  A brief rundown on how data from the voxel world can be queried from gameplay
  code.
icon: hammer-crash
---

# Querying Voxel Data

Any data from the Voxel World can be queried from gameplay code using the `Query Voxel Layer` and `Multi Query Voxel Layer` nodes. The two nodes do the same thing, but a multi-query queries multiple positions at the same time.

Query nodes output a struct containing the distance of the assigned volume layer (or height in the case of a height layer) at the given position. If `Query Materials` is enabled, the `Material` pin in that struct will contain a reference to the most prominent material at the query position. Metadata can also be queried by making an array with the relevant names plugged into the `Metadatas to Query` pin. They'll then be part of the `Metadata` map in the output struct.

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption><p>The two <code>Query Voxel Layer</code> nodes, with the output struct broken out into its contents.</p></figcaption></figure>

