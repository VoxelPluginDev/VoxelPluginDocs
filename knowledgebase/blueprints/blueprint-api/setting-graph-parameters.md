---
description: >-
  How to spawn and configure stamps from gameplay code, including how to
  configure graph parameters.
icon: hammer-crash
---

# Spawning & Configuring Stamps

Stamps can be spawned and configured using the `Set Stamp` and `Add Stamp` nodes in Blueprint. The `Set Stamp`node is used when setting values on an existing stamp actor or component. The`Add Stamp` node is used with [Voxel Instanced Stamp components](../../working-with-stamps/instanced-stamps.md). The voxel world is automatically refreshed where needed when stamps are added or updated.

<figure><img src="../../../.gitbook/assets/image (245).png" alt=""><figcaption><p>A basic overview of the main nodes used to configure stamps.<br>An instanced stamp component is used to avoid unnecessary actor/component overhead. </p></figcaption></figure>

{% hint style="warning" %}
**Use the `Make Copy` node!**&#x20;

Stamp pins work like references. If you call a `Set X` node on the variable directly, it will change the value of X on the variable itself, not just on the stamp you're spawning with it. The `Make Copy` node copies the variable into a new one, so you can change values on it without changing the original variable.
{% endhint %}

