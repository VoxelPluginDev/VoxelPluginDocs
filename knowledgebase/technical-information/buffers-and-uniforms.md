---
description: The differences between Buffers and Uniforms and their distinct usages.
---

# Buffers and Uniforms

## 1. Usage

In the graph system, some pins are square and some pins are round. This shows whether the data is what we call a Buffer, or a Uniform.

* A buffer, which has a square pin, carries data that can be different from one position to the next.&#x20;
* A uniform, which has a traditional round pin, is used when a value is the same for every position.&#x20;

<figure><img src="../../.gitbook/assets/image (60).png" alt=""><figcaption><p>Some pins are uniform (round), others are buffers (square), and some pins (grey center) can be either.</p></figcaption></figure>

Uniforms can be plugged into any input pin, including Buffer pins. Buffers can specifically only be plugged into other Buffer pins. They cannot be plugged into a Uniform pin. Some pins can be converted from one to the other by right-clicking the pin and clicking the "convert" option in the context menu.

## 2. Technical Details

A single-length `Buffer` is functionally identical to a `Uniform`, and using one or the other will not have any impact on performance. Performance considerations only change if the `Buffer` contains multiple values, for instance because it uses Position somewhere in the node-chain to calculate a value.

The most straight-forward rule of thumb for whether a given value should be a `Buffer`or a `uniform` is this:

* If a value changes based on position, it is almost always a `Buffer`.&#x20;
* If a value is the same for every position, it is almost always a `Uniform`.&#x20;

On a technical level, a `Buffer` is simply a stack of data: a single `Buffer` pin represents the data for every point within a chunk at the same time, rather than just being a single value. Having this data in a stack allows it to be operated on in bulk, i.e. by calculating many values at once using ISPC on the CPU, or (not currently supported, but theoretically possible) GPU compute.&#x20;

`Uniform` values are constant throughout all of a chunk's positions. If needed, a `Uniform` value can change from once chunk to the next. For example, the `Get LOD` node returns a uniform. It reports the current LOD, which varies from one chunk to the next. Because the LOD value will never change within a chunk, however, it can still be a uniform.
