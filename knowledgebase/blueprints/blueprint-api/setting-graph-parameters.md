---
description: >-
  How to spawn and configure stamps from gameplay code, including how to
  configure graph parameters.
icon: hammer-crash
---

# Spawning & Configuring Stamps

Stamps can be spawned and configured using the `Set Stamp` and `Add Stamp` nodes in Blueprint. The `Set Stamp`node is used when setting values on an existing stamp actor or component. The `Add Stamp` node is used with [Voxel Instanced Stamp components](../../working-with-stamps/instanced-stamps.md). The voxel world is automatically refreshed where needed when stamps are added or updated.

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption><p>A complex blueprint used to spawn a lot of voxel stamps (and spline meshes) along a spline in-editor. <br>An instanced stamp component is used to avoid unnecessary actor/component overhead. </p></figcaption></figure>
