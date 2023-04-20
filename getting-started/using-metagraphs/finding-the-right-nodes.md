---
description: Making sense of graph nodes when next steps are unclear.
---

# Finding the Right Nodes

When working with MetaGraphs, it is easy to be unsure what nodes to use.&#x20;

When this happens, use whatever input and output pins you have in your graph by dragging from them into the graph and letting go. This will open a search menu, which will only show compatible nodes, making it easier to find what you're looking for.&#x20;

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Dragging out from a pin will provide contextual recommendations at the top of the search menu.</p></figcaption></figure>

At the top of the search menu, you will find a few recommendations for nodes which are specifically designed to work with the type you're dragging out from. These will often be the nodes which are key building blocks for creating a functional graph.

This strategy makes it very straight-forward to, for instance, determine that the `Generate Marching Cube Surface` nodes take a `Create Spawner` node as their first input.

The same also works when simply right-clicking in empty space; the recommendations list will be made up out of **Meta Nodes** - nodes which produce world outputs from the graph.
