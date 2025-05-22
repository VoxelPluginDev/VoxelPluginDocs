# Override Stamps & Get Previous Nodes

## Override Blend

Both Height and Volume brushes have an Override blend mode. This blend mode lets you manually control how this stamp overwrites the ones that came before it. Whatever is plugged into the output pins will entirely override all previous data within the stamp's bounds.

Stamps with the Override blend mode can use the `Get Previous ...` nodes. These nodes return the value of every stamp that comes befoire this one combined. Note, this doesn't come at any additional cost - it's using data that's already there anyway, this is just a way to control the blending that would otherwise be done under the hood.

{% hint style="info" %}
Graphs can be forced to only be placeable with the Override blend mode by expanding the advanced properties on the output node, and enabling the "Enable Blend Mode Override" tickbox.
{% endhint %}



<figure><img src="../../../.gitbook/assets/image (247).png" alt=""><figcaption><p>A height graph built for the override blend mode. It lerps with PreviousHeight based on a radius with falloff. The Lerp Alpha is also plugged into the Output node's Alpha pin. That pin controls material and metadata blending.</p></figcaption></figure>

{% hint style="info" %}
The Output node's Alpha pin is hidden by default. Click the "Advanced Properties" arrow at the bottom of the node to show it.
{% endhint %}

When using Override blend modes, the Alpha pin on the graph Output node (under advanced properties) is used to control where the graph's materials and metadata are applied. If this is set to 1 (which is the default), all positions within the stamp's bounds will override their previous material and metadata with the stamp's. If you want to control the way your materials or metadata are blended more specifically, `Get Previous Material` and `Get Previous Metadata` can be used to manually blend those values.&#x20;

