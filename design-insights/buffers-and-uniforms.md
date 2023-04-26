---
description: The differences between Buffers and Uniforms and their distinct usages.
---

# Buffers and Uniforms



{% hint style="info" %}
This page is a stub. This means that the information on this topic will be significantly expanded in the future. What is currently on this page is an incomplete picture.
{% endhint %}

`Buffers` and `Uniforms` can be distinguished by their pin visual. `Uniform` pins have standard round pins, whereas `Buffer` pins are represented by a square pin.

`Buffers` can only be used as an input for `Buffer` pins specifically. `Uniforms` can be used as an input for both `Buffer` and `Uniform` pins.&#x20;

A single-length `Buffer` is functionally identical to a `Uniform`, and using one will not have any impact on performance. Performance considerations only change if the `Buffer` contains multiple values.

The most straight-forward rule of thumb for whether a given value should be a `Buffer`or a `uniform` is this:

* If a value changes based on position, it is almost always a `Buffer`.&#x20;
* If a value is the same for every position, it is almost always a `Uniform`.&#x20;

On a technical level, a `Buffer` is simply a stack of data: a single `Buffer` pin represents the data for every point within a chunk at the same time, rather than just being a single value. Having this data in a stack allows it to be operated on in bulk, i.e. by calculating many values at once using ISPC on the CPU, or (not currently supported, but theoretically possible) GPU compute.&#x20;

`Uniform` values are constant throughout all of a chunk's positions. If needed, a `Uniform` value can change from once chunk to the next. For example, the `Get LOD` node returns a uniform. It reports the current LOD, which varies from one chunk to the next. Because the LOD value will never change within a chunk, however, it can still be a uniform.
