---
description: How to use graph stats, and what they do and do not show.
---

# Generation Performance

In order to allow for easy profiling of generation performance, the graph system allows for the recording of generation stats.&#x20;

## 1. Building performant graphs

There are some general considerations when it comes to building graphs that function quickly.

### &#x20;  1.1 Node settings

Many of the more complex nodes within the plugin come have a variety of settings that will affect performance. Using these properly will lead to more performance for often similar results. An example of this would be the octave count on the Advanced Noise node. Noise nodes with lower octave counts will generate faster.

#### &#x20;      1.1.1 Stamp bounds

Stamps only affect the terrain inside their bounds. Using bounds that are as small as possible will allow graphs to be skipped when they aren't relevant. Bounds are increased by the smoothness of the stamp, and in the case of graph-based stamps, whatever is plugged into the bounds pin in the graph.

## 2. Integrated stats

### &#x20;  2.1 Enabling stats

If the **Enable Stats** toggle in the top-left is enabled (showing blue, instead grey like when disabled), supported (not all) nodes will start tracking their execution times.&#x20;

When tracking stats is enabled, world generation may be slower.&#x20;

### &#x20;  2.2 Interpreting stats

Each node will show a yellow banner underneath, showing how often the node was run, how long a single run took on average, and how much time has been spent on the node in total. These values do not reset until stats are disabled and re-enabled. When re-generating the world, the stats will simply continue counting upwards without resetting their state.

Because world generation may take longer when stats are enbaled, the times reported are not always representative of the true time generation takes. The stats do give a concrete idea of how long one node takes compared to another node.

<figure><img src="../../../.gitbook/assets/image (188).png" alt=""><figcaption><p>A graph with stats enabled, showing execution times per node. The total times are not always accurate, but this does show that the cost of multiplying is way lower than the cost of generating 2D noise.</p></figcaption></figure>

There is not currently any way to see the total time a graph has taken to generate, or to see how an inclusive time, i.e. how long a node plus its inputs took to run.

## 3. Deep profiling using Insights

While the above stats can be useful for quickly getting a feel of where a graph is spending its time, the system doesn't provide a complete picture of where all costs are going.&#x20;

For deeper profiling, Unreal Insights is the most effective tool. Voxel Plugin generation tasks are fully visible to Insights, as long as the Voxel trace channel is enabled. Insights can be manually configured, or the console command `voxel.StartInsights` can be used to start a trace. An Insights window will be opened, and the active trace will be marked as Live. The trace can be ended using `voxel.StopInsights`.

Generation tasks are run on separate threads. To keep profiling straight-forward, we recommend using two threads for generation. Using more threads will make it harder to see where time is being spent. Using a single thread can create unrepresentative performance data as it may hide memory bottlenecks.

<figure><img src="../../../.gitbook/assets/image (189).png" alt=""><figcaption></figcaption></figure>

If there is noticeable hitching in the framerate during world generation, this will not be coming directly from the voxel threads, but instead will have its source in the render or game threads. If those don't show show anything clearly responsible for the hitches, it may be caused by a lack of RAM.
