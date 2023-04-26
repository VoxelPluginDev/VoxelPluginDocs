# Navmesh & Collision

When you use a Generate Marching Cube Surface node, collision will be computed on visible chunks. You can disable that by using a body instance with no collision:

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Visible chunks being used as a source for collision means collision will have lower resolution the further away you move from the camera. This is not desirable when you have NPCs or other colliding bodies moving around the map.

To fix that, the plugin has Voxel Invokers: components you can add to actors to generate collision (or navmesh) around them.

## Adding a voxel invoker

Add a new Box Invoker or Sphere Invoker to your actor:

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

You can then move/rotate/scale the invoker as needed.

## Setting up a Physics Scene

To generate collision or navmesh from invokers, we need to setup a new physics scene.

First, create a new Voxel Physics Scene in your content browser:

