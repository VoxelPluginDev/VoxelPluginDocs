---
description: The differences between Buffers and Uniforms and their distinct usages.
---

# Buffers and Uniforms

## 1. Usage

Data used in the plugin can be a Buffer or a Uniform. Generally, Uniforms are used when data is the same for every position in a chunk.&#x20;

Both types have their own pin visual. Uniform pins have round pin icons, whereas Buffer pins have a square pin icon.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Some pins are uniform (round), others are buffers (square), and some pins (grey center) can be either.</p></figcaption></figure>

Buffers can only be used as an input for Buffer pins specifically. They cannot be plugged into a Uniform pin. Uniforms can be used as an input for both Buffer and Uniform pins.&#x20;

## 2. Technical Details

A single-length `Buffer` is functionally identical to a `Uniform`, and using one will not have any impact on performance. Performance considerations only change if the `Buffer` contains multiple values.

The most straight-forward rule of thumb for whether a given value should be a `Buffer`or a `uniform` is this:

* If a value changes based on position, it is almost always a `Buffer`.&#x20;
* If a value is the same for every position, it is almost always a `Uniform`.&#x20;

On a technical level, a `Buffer` is simply a stack of data: a single `Buffer` pin represents the data for every point within a chunk at the same time, rather than just being a single value. Having this data in a stack allows it to be operated on in bulk, i.e. by calculating many values at once using ISPC on the CPU, or (not currently supported, but theoretically possible) GPU compute.&#x20;

`Uniform` values are constant throughout all of a chunk's positions. If needed, a `Uniform` value can change from once chunk to the next. For example, the `Get LOD` node returns a uniform. It reports the current LOD, which varies from one chunk to the next. Because the LOD value will never change within a chunk, however, it can still be a uniform.

Additionally, there are a handful of special types, such as Surfaces and Point Sets, which are treated as uniform under the hood, despite them being intuitively 'different per position'. In these cases, the uniform is actually a struct that contains a set of buffers. The entire struct is uniform per chunk, but this single uniform struct holds a value for each position within its chunk inside the buffer elements. To avoid confusion, these types have visually distinct icons and are not presented to the user as uniforms.
