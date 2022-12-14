# Passing Data to Materials

{% hint style="info" %}
This page is a stub. This means that the information on this topic will be significantly expanded in the future. What is currently on this page is an incomplete picture.
{% endhint %}

Any data that can be represented as a float can be passed into materials from a MetaGraph. Information can be passed using Vertex Data, or using detail textures.

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

The `Vertex Data`'s `Color` and `Texture Coordinate` pins can be used to transmit vertex data. The `Detail Texture`'s `Color` pin will create a texture at a higher resolution than the mesh' vertex density, and can then be retrieved in a material using the `Voxel_GetDetailTextures` `UV` pin to sampe a Texture Parameter named `VoxelDetailTextures_<NAME>Texture`, with `<NAME>` being identical to your MetaGraph node's `Name` pin. &#x20;
