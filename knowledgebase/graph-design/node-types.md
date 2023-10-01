---
description: >-
  The different types of nodes that exist within Voxel Plugin and what their
  distinct uses are.
---

# Node Types

### 1. Simple Nodes

The most common operations in generation graphs are math operations, or straight-forward logical operations. The nodes responsible for these types of operations are called Simple Nodes, colour-coded green. These nodes take an input, do some straight-forward operation on this input, and then output the result of that operation. Operations done on these nodes can generally be optimized and cached very effectively.

<figure><img src="../../.gitbook/assets/image (51).png" alt=""><figcaption><p>Simple nodes do basic operations on input pins to produce output pins.</p></figcaption></figure>

### 2. Complex Nodes

While simple nodes make up the bulk of a graph, there are many more advanced operations that they cannot do. They make it possible to do basic operations on data, but they can't create complex data from scratch. Unlike simple nodes, complex nodes can mutate Query Data as it flows through, and execute asynchronous operations, allowing them to do far more advanced tasks. The key caveat that stems from this, is that these nodes have potential impacts on a graph's ability to cache and re-use values. As a rule of thumb, complex nodes generally have more significant performance implications that simple nodes.&#x20;

<figure><img src="../../.gitbook/assets/image (109).png" alt=""><figcaption><p>Complex nodes can mutate Query Data and do complex, sometimes asynchronous, operations.</p></figcaption></figure>

### 3. Execution Nodes

As a general rule, any node within the Voxel Plugin graph system take some input, and outputs it to another node through a pin. MetaNodes, colour-coded red, are the single exception to this rule. Rather than outputting data to other plugin systems, MetaNodes are the bridge to the core engine systems; they are responsible for creating outputs which can be referenced and interacted with in other places. They create components which interact with, and can be interacted with by, the rest of Unreal Engine.

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption><p>MetaNodes are the only nodes which can output data from a specific graph into other plugin and engine systems.</p></figcaption></figure>

The inner workings of Execution Nodes can be found on the [execution-flow-and-query-data.md](execution-flow-and-query-data.md "mention") page.

