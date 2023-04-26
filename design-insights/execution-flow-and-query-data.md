---
description: >-
  The way individual graphs are executed, how they come together and what the
  premises and impacts behind Query Data are.
---

# Execution Flow & Query Data

{% hint style="info" %}
This page is a stub. This means that the information on this topic will be significantly expanded in the future. What is currently on this page is an incomplete picture.
{% endhint %}

Execution is right to left. A MetaNode will be called when the graph is executed, after which it will call whatever nodes are connected to its input pins. These nodes will then call the nodes connected to their input pins, and so on until the end of the chain is reached.

A node calling another node can send information downstream, called Query Data. This data can be mutated by [complex nodes](#user-content-fn-1)[^1].

[^1]: Complex nodes are a node type identifiable by blue node headers. Read more about them in the [#complex-nodes](node-types.md#complex-nodes "mention") section.
