# Choosing the right Chunk Spawner

{% hint style="info" %}
This page is a stub. This means that the information on this topic will be significantly expanded in the future. What is currently on this page is an incomplete picture.
{% endhint %}

The "Spawn Chunks by Screen Size" node will create cube-shaped chunks across the entire world in three dimensions, attempting to keep their size on screen consistent, making farther away chunks less detailed.&#x20;

The "Spawn Chunks by Range" nodes do not support varying levels of detail, and instead just spawn a set number of chunks at a given size. This is mostly useful for things like foliage (this would use the 2D variant, or potentially blocky terrains (which would use the 3D variant).
