# Foliage

## Setting the meta graph

For this example, we'll be using the following meta graph as base:

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

To spawn the foliage, we will need to do line traces against a physics scene. Create a new one:

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

Add a new Generate Marching Cube Collider node to your meta graph, assigning the physics scene you just made to it:

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

This node will populate the physics scene with marching cube colliders we can use to line trace our foliage against.

## Setting up the foliage asset

Create a new foliage asset:\


<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

In the foliage asset, switch to the Foliage Graph at the top right:

<figure><img src="../.gitbook/assets/image (22) (1).png" alt=""><figcaption></figcaption></figure>

Add the following nodes:

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

To convert the Vector2D output of the Generate Halton Position 2D node to a Vector, just drag the Vector2D pin to the Vector one. Make sure to set the Z value to 1000000.0.

Make sure to also assign your physics scene and a mesh.

This graph does the following:

* Generate some 2D positions using a Halton random generator
* Line trace against the colliders we generated earlier
* Assign a mesh to the hit positions
* Render the hit positions

## Wrapping up

Back into your meta graph, create the following node:

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

You should now see spheres being spawned in your level!

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

If you increase your Distance Between Positions in your foliage graph to 500:

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

You'll get less dense spheres:

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

## Restricting the spawning slope

To restrict the slope at which your foliage spawns, you can use the following logic:

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

20 is the max spawn angle in degrees here. This gives you the following (with tuned noise settings):

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

## Controlling foliage density with brushes

Follow [brush-and-channels.md](brush-and-channels.md "mention"), but keep our current meta graph. This should get you setup with some simple brushes and a voxel channel. Alternatively, follow [installing-example-content.md](../getting-started/installing-example-content.md "mention").

Add the following nodes at the end of your foliage:

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

This will query the distance from MyChannel at the foliage position, and only select foliage which are less than 10m away from the brush.

This results in this behavior:

{% embed url="https://youtu.be/32z28gwEZGI" %}

## Spawning foliage on foliage

Spawning foliage on foliage is just a matter of outputing foliage colliders into a new physics scene, and tracing against that physics scene.

Make a new physics scene, name it FoliagePhysicsScene:

<figure><img src="../.gitbook/assets/image (23) (1).png" alt=""><figcaption></figcaption></figure>

Duplicate your foliage graph, and add the following at the end:

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

Make sure to add your new foliage in the Generate Foliage node of your meta graph.

You should be getting something like this:

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

In your "small foliage" graph, add a new line trace node tracing against our foliage physics scene. Merge the result back into our main attributes:

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

You should now see foliage being spawned on top of other foliage!

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

## Tips & tricks

* You can create foliage collection to make it easier to reference your foliage in your meta graph
* You can toggle Debug on the line trace nodes. This will show you the traces being done
* Just like with brush assets, you can make foliage asset instances!
* You can increase the LOD of your line traces. Going from LOD 0 to LOD 1 will be \~8x faster
