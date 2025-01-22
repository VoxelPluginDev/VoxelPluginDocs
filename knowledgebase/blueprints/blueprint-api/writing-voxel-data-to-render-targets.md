---
icon: hammer-crash
---

# Writing Voxel Data to Render Targets

Various nodes exist for writing data from voxel layers into render targets. This makes it possible to, for example, easily cache voxel data for use in materials. Each channel in the render target can be configured to retrieve particular data from the target layer. Both sync (game-thread blocking) and async (the game will continue running while this processes) versions of these nodes exist.

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>
