---
description: This article dives into the various stat commands available within the plugin.
---

# Memory Usage & Render Performance



Profiling memory use and render performance is largely separate from generation performance.

### 1. Debug and Rendering - `stat voxelcounters`

`stat voxelcounters` brings up a display of general counters related to Voxel Plugin functionality.

<figure><img src="../../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

* `Num Foliage Instances` is the current amount of instances generated by the foliage nodes within your graph. This counts all active instances, including culled instances. More statistics on foliage rendering can be found using the stock `stat foliage` and `stat rhi` commands.&#x20;
* `Detail Textures Num` is the amount of unique texture resources that have been created by your graph. This includes some amount of indirection textures required for detail textures to work.
* `Voxel Material Instances Used` is the amount of unique material instances created for your active terrain chunks.
* `Num Subsystems` is the number of unique subsystems running within the plugin at this time.
* `Voxel Material Instances Pool` should not be a huge number (>1k). If it is, post a support query on the Voxel Plugin Discord.

### 2. (V)RAM Usage - `stat voxelmemory` / `stat LLM`

`stat voxelmemory` reports how much memory is currently being used by some of the various plugin systems. &#x20;

<figure><img src="../../.gitbook/assets/image (180).png" alt=""><figcaption></figcaption></figure>

Some of the memory tracked here, such as brush data, is CPU-side (RAM), whereas other parts of it, like detail textures, are GPU-side (VRAM). It does not provide a complete picture of CPU-side memory use either, as not all allocations are tracked.

For more precise CPU-side memory tracking, low level memory counters have been implemented. This can be enabled by running the project with -LLM as commandline instruction. This will allow for every single RAM allocation done by Voxel Plugin systems to be tracked. These counters can be displayed using `stat LLM`.



-LLM in commandline, enables tracking, stat LLM will show currently allocated memory usage from all voxel usage, whereas some stuff is missing from stat voxelmem
