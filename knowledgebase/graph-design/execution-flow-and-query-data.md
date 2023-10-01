---
description: >-
  The way individual graphs are executed, how they come together and what the
  premises and impacts behind Query Data are.
---

# Execution Flow & Query Data

### 1. Graph Execution

#### &#x20;    1.1 | Calling an Execution node

Any (red) MetaNodes that exist in a graph with nothing plugged into their execution pin will be run, but only when that graph is called directly from the world. MetaNodes will not automatically run when a graph is called as a function from within another graph.

Any nodes that have an active execution pin hooked up to an Execute node in the top-level graph will be called when said top-level graph is executed.&#x20;

{% hint style="info" %}
If a graph called as function should run its MetaNodes, the execution pins can be made into an output, which can be plugged into an Execute node in the graph calling it.&#x20;
{% endhint %}

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption><p>MetaNodes are the drivers of graph execution.</p></figcaption></figure>

Any called MetaNodes will call whatever nodes are connected to their input pins. These nodes will then call the nodes connected to their input pins, and so on until the end of the chain is reached.

#### &#x20;  1.2 | Right-to-left execution

Graph execution flows right to left.&#x20;

When a node is called, it will evaluate whether it has all the data it needs readily available. If the node has no connected input pins, all data is present on the node, it simply returns its requested output data.&#x20;

However, if any of its input pins do have connections, those nodes need to be evaluated in order to produce the requested output. The node will therefore call the nodes that are connected to its input pins. The next node will go through the same steps, and this cycle continues until a node is reached that has no connected input pins.

While most data therefore flows left-to-right, the 'inverted' execution flow is crucial because it allows us to also send some data right-to-left. We call this Query Data.

### 2. Query Data

A node calling another node can send information downstream, called Query Data. A node providing Query Data will send along this piece of information whenever it calls a node. This data can therefore be read back by any downstream node. It can be also mutated by [complex nodes](#user-content-fn-1)[^1]. Some downstream nodes will require specific Query Data to be present, and errors will be thrown if this is not the case. Generally, the fix for this is to add another node upstream.&#x20;

For example, the **Generate Surface Points** node takes a bounds pin.&#x20;

<figure><img src="../../.gitbook/assets/image (130).png" alt=""><figcaption><p>When generating surface points, you generally want to use Spawnable Bounds, which is provided by the chunk spawner.</p></figcaption></figure>

When using this node, you will generally want to use **Get Spawnable Bounds**, which will look for the SpawnableBounds Query Data. This Query Data is created and added to the downstream information flow by the **Spawn Chunks** node, which will switch the state of this Query Data for each chunk so that it matches said chunk's bounds. Thanks to this, every chunk's **Generate Surface Points** node will only generate points within that chunk's bounds.&#x20;

Interesting to note is that without the [right-to-left execution flow](execution-flow-and-query-data.md#1.2-or-right-to-left-execution), the Get Spawnable Bounds node would have no way to know anything about what the Spawn Chunks node is doing.

[^1]: Complex nodes are a node type identifiable by blue node headers. Read more about them in the [#complex-nodes](node-types.md#complex-nodes "mention") section.
