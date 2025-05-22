---
icon: hammer-crash
---

# Instanced Stamps

Instanced Stamp Components (or ISCs) allow you to have a large amount of stamps in your world without the extra performance cost associated with having large amounts of actors or components.

Stamps added to an ISC will use worldspace transforms. This means that when an ISC is moved, any stamps added to it to it will stay where they are, rather than moving along.&#x20;

{% hint style="info" %}
Instanced stamp components are automatically used when stamps are spawned using the `Voxel Stamp Spawner` node in PCG.
{% endhint %}
