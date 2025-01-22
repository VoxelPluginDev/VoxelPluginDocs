---
description: >-
  A brief rundown on how data from the voxel world can be queried from gameplay
  code.
icon: hammer-crash
---

# Querying Voxel Data

Any data from the Voxel World or isolated graphs can be queried from gameplay code. For this purpose, a `Query Voxel Layer` and a `Multi Query Voxel Layer` exist. The two nodes function largely the same, but a multi-query can query multiple positions at the same time.

By default, a query will output a struct containing the distance of the assigned volume layer (or height in the case of a height layer) at the query position in its `Value` pin. If `Query Materials` is enabled, the `Material` pin will contain a reference to the highest-weight material present at the query position. If any metadata names were fed into the node, those metadata values will be in the `Metadata` map pin in the output struct.

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption><p>The two <code>Query Voxel Layer</code> nodes, with the output struct broken out into its contents.</p></figcaption></figure>

