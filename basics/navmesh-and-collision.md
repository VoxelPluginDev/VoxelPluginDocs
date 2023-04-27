# Navmesh & Collision

When you use a Generate Marching Cube Surface node, collision will be computed on visible chunks. You can disable that by using a body instance with no collision:

<figure><img src="../.gitbook/assets/image (6) (2).png" alt=""><figcaption></figcaption></figure>

Visible chunks being used as a source for collision means collision will have lower resolution the further away you move from the camera. This is not desirable when you have NPCs or other colliding bodies moving around the map.

To fix that, the plugin has Voxel Invokers: components you can add to actors to generate collision (or navmesh) around them.

## Adding a voxel invoker

Add a new Box Invoker or Sphere Invoker to your actor:

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

You can then move/rotate/scale the invoker as needed.

## Setting up a Physics Scene

To generate collision or navmesh from invokers, we need to setup a new physics scene.

First, create a new Voxel Physics Scene in your content browser:

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

This is an empty asset that will be the link between nodes creating physics objects, and nodes processing them.

Then add a new Generate Marching Cube Collider node, link its distance & assign the physics scene to it:

<figure><img src="../.gitbook/assets/image (10) (3).png" alt=""><figcaption></figcaption></figure>

This node will now generate marching cube colliders in the physics scene, on demand. We now need a node that will take these colliders & create actual collision components from them.

To do so, add a new Generate Collision from Physics Scene node, and set its physics scene:

<figure><img src="../.gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>

## Debugging

By default, no collision is generated in the editor. You can use `voxel.collision.EnableInEditor 1` in the console to enable collision generation in the editor.

Once this is done, you should be able to see colliders generated when switching the viewport to the Player Collision view:

<figure><img src="../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

## Multiplayer

Usually, you want to have different behavior on clients & servers. To do so, the plugin let you access the current net mode in the graph.

A typically setup could be:

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

## Navmesh

Navmesh works pretty much the same way as collision:

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

To debug navmesh in the editor, you can use `voxel.navigation.EnableInEditor 1`

Make sure to set the navmesh generation to Dynamic in your project settings:\


<figure><img src="../.gitbook/assets/image (6) (3) (1).png" alt=""><figcaption></figcaption></figure>
