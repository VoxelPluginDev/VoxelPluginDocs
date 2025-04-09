# Spline Stamps

{% hint style="info" %}
There is an [example ](../../../getting-started/installing-voxel-content.md)available on this topic!
{% endhint %}

Currently only Height Splines are supported. Volume Splines will be added in the future. Additionally, generation performance will be improved in future releases.

{% hint style="warning" %}
Spline stamps always use the [Override blend mode](../blend-modes.md#override-blend), so they override all data within their bounds. They need to be manually configured to blend with previous data in the graph.&#x20;
{% endhint %}

#### Per-Point Control with Spline Parameters

Standard graph parameters are set once for an entire stamp component. When working with splines, you often want more granular control. For this reason, spline graphs have access to special `Spline Parameter` types for float-based values.

<figure><img src="../../../.gitbook/assets/image (250).png" alt=""><figcaption><p>A basic Height Spline with its a TopWidthBias spline parameter, allowing it to have the width increased for single control points.</p></figcaption></figure>

While regular parameters are shown in the actor's Spline Parameters will be shown in the details panel when a control point is selected. The value for the parameter can then be changed for each individual control point.

{% hint style="info" %}
Spline parameters do not currently support defaults. We recommend using a regular parameter to create a 'default', and a spline parameter to add/subtract from that for per-point control.
{% endhint %}

#### Spawning Spline Stamps from PCG

Height Spline Stamps can be spawned from PCG using the `Create Voxel Spline`, but parameter overrides are not yet supported. Spline Stamps spawned using the `Voxel Stamp Spawner` node will not function. The functions for handling voxel splines on the PCG side will be refactored in an upcoming release.

