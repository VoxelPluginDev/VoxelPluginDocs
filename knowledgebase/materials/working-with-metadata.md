---
description: General information on using Metadata on stamps, in graphs and in materials.
---

# Working with Metadata

Metadata lets you set float-based values per voxel, by name. This data can then be passed into materials through vertex attributes (UVs or Vertex Color), and can be queried by name through C++ and Blueprint for gameplay purposes. It can also be queried by Voxel PCG nodes.

Stamps can write their own information into these named Metadata values. Graphs can be used to write complex values into Metadata.

#### Using Metadata to Control Generation

In some cases, you may want to use metadata to control terrain generation; for instances, you may want to apply some extra noise over the entire world based on a NoiseStrength metadata. To do this, you can use a **Query Metadata** node.

Note that the layer assigned on the `Query Metadata` node **has to** come before the layer the stamp is assigned to. You may need to add a new layer to do this. If so, you likely need to change your Voxel World `Layer to Render` and layer queries in other places accordingly.

{% hint style="warning" %}
**Common errors when using metadata in Voxel Graphs:**

* `Cannot query ...: already queried in the stack. Queried layers: ...`
  * The `Query Metadata` node is querying the same layer the stamp is assigned to. Set the layer to an earlier layer, or set the stamp to a later layer.
* `... cannot query previous metadata ... when metadata ... is not being queried`
  * You are using the `Query Previous Metadata` node. This can only be used in the graph network leading into a Metadata graph output pin. Use `Query Metadata` instead.
{% endhint %}

#### Passing Metadata to Materials

{% hint style="warning" %}
Passing metadata to materials is not currently supported for non-Nanite rendering.
{% endhint %}

Metadata can be passed into materials by configuring the Materials Channels settings on the Voxel World. A single Metadata attribute can be tied (by name) to a single channel of a vertex-attribute (Vertex Color or a UV).

The Metadata can then be accessed by getting those vertex attributes using the standard material nodes (VertexColor / TexCoord).

<div align="center"><figure><img src="../../.gitbook/assets/image (11).png" alt="" width="375"><figcaption><p>Metadata applied to vertex colors in the Voxel World details panel.</p></figcaption></figure></div>

