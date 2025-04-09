---
icon: hammer-crash
---

# Blend Modes

## Standard Blend Modes



## Override Blend

Both Height and Volume brushes have an Override blend mode. This blend mode lets you manually control how this stamp overwrites the ones that came before it. Whatever is plugged into the output pins will entirely override all previous data within the stamp's bounds.

Override stamps have access to the result of the stamps applied before them using `Get Previous ...` nodes. Graphs built for this blend mode should usually use the `Get Previous Height` / `Get Previous Distance` nodes to control how the surfaces are blended.&#x20;

{% hint style="warning" %}
`Get Previous ...` nodes can be used in any graph, regardless of blend mode, but the results are usually undesirable on any blend mode other than override.
{% endhint %}



<figure><img src="../../.gitbook/assets/image (247).png" alt=""><figcaption><p>A height graph built for the override blend mode. It lerps with PreviousHeight based on a radius with falloff. The Lerp Alpha is also plugged into the Output node's Alpha pin. That pin controls material and metadata blending.</p></figcaption></figure>

When using Override blend modes, the Alpha pin on the graph Output node is used to control where the graph's materials and metadata are applied. If this is set to 1 (which is the default), all positions within the stamp's bounds will override their previous material and metadata with the stamp's. `Get Previous Material` and `Get Previous Metadata` can be used to specifically blend some parameters, but just plugging in the Alpha will usually be enough.

{% hint style="info" %}
The Output node's Alpha pin is hidden by default. Click the "Advanced Properties" arrow at the bottom of the node to show it.
{% endhint %}

