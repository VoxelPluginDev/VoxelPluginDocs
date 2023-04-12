---
description: >-
  This article provides details on various nodes and techniques that can be used
  to design efficient MetaGraphs.
---

# Utilities for Performant Graphs

{% hint style="info" %}
This page is a stub. This means that the information on this topic will be significantly expanded in the future. What is currently on this page is an incomplete picture.
{% endhint %}

Note nodes: Cache, Query 2D, Run on GPU

Note: Single-length buffer is functionally the same as a uniform. _So you don't ever need to worry about making things uniform for perf reasons; if you plug a constant value (like a parameter or something) into a buffer pin, it'll have the exact same perf overhead as plugging that into a uniform pin._

_Read more at_ [buffers-and-uniforms.md](../deep-dives-into-metagraphs/buffers-and-uniforms.md "mention").
