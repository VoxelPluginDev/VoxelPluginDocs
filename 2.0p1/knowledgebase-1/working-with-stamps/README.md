# Working with Stamps

Stamps are used to tell the Voxel World what kind of terrain to generate, and where to generate it. Usually stamps are actors, but they can also be used as components, or they can even be [instanced](../../knowledgebase/working-with-stamps/instanced-stamps.md).

### Kinds of Stamps

&#x20;Voxel Stamps come in two primary types:&#x20;

* Volume Stamps, which affect the world in 3D, so they are able to generate a unique value for each position. They are used to create caves and overhangs. &#x20;
* Height Stamps, which work in top-down 2D. They are cheaper to generate because they can skip the third dimension, but they can't generate caves or overhangs.

{% hint style="info" %}
When talking about stamp 'performance', we specifically mean generation performance - so the time it takes for the world to generate.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>A stamp actor, its primary type set to Volume, and data source to Mesh. It writes a voxelized mesh into the scene.</p></figcaption></figure>

For each of those, there are four kinds of [data sources](#user-content-fn-1)[^1]:

* Static Data (Heightmap for Height Stamps, Mesh for Volume Stamps)
  * This read back some kind of precomputed data, so they don't need to generate any kind of information on the fly. They are the fastest type of stamp, but their data needs to be kept in RAM.
* Spline &#x20;
  * This reads back the spline data from a spline component and use it to affect the terrain. Only basic 'tubes' are supported right now, but graph support will be added in the future.
* Graph
  * This runs a graph for each position the stamp affects. Graph parameters can be seen and edited from the stamp's details panel.
* Sculpt
  * This does nothing on its own, but holds sculpt data. This stamp can then be selected from the Voxel Sculpt mode, which will write data into it.

### Stamp Behavior

Every stamp has a handful of standard settings in its details panel. These settings control how the stamp affects the world - for instance, if it affects the terrain's shape, or only its materials.

* **Behavior:** Which kinds of data a stamp affects. The three kinds of data (shape, material and metadata) can be turned off and on separately from one another.
  * **Shape** controls whether the stamp should modify the actual terrain mesh.
  * **Material** and **Metadata** control whether the stamp should replace the existing material or metadata with its own.&#x20;

{% hint style="info" %}
If a stamp has no material or metadata assigned, it will leave the existing data on the terrain, even if they are enabled in the behavior switches.
{% endhint %}

* **Layers:** What layers a stamp writes into. Layers give you better control over what brushes come before other ones. In most cases, you will only have one layer selected in a stamp's layer list.
* **Priority:** Whether the stamp should apply before or after others. Stamps with the lowest priority number are applied first, those with the highest number are applied last (so they are visually on top). You can read more about this and layer on the [Layers & Priority](../../knowledgebase/working-with-stamps/layers-and-priority.md) article.
* **Blend Mode:** How this a stamp is merged with the ones that came before it. Usually a minimum or maximum, but sometimes other approaches are used. ~~See~~ [~~Blend Modes~~](blend-modes.md) ~~for more details.~~
* **Smoothness:** How smoothly a stamp blends into the ones that came before it. This does not only affect shape - materials and metadata are also blended smoothly.

[^1]: A stamp tells the world about some data at its position. Where it comes from is called the 'data source'.
