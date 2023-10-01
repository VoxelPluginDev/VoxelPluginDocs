# Navmesh & Collision

When you use a Generate Marching Cube Surface node, collision will be computed on visible chunks. This can be disabled by switching the Body Instance pin's value to No Collision. In order to customize this (or any) value's complex defaults, a **Make Value** node can be plugged into it, or a parameter can be created for and plugged into it. In either of those cases, the value can then be adjusted in that node's details panel.&#x20;

<figure><img src="../.gitbook/assets/image (131).png" alt=""><figcaption><p>Complex structs can have their defaults assigned in the details panel through a Make Value node.</p></figcaption></figure>

Visible chunks being used as a source for collision means collision will have lower resolution as the terrain is farther from the camera. This can be undesirable when have NPCs or other colliding or simulated bodies moving around the map.

This can be addressed using Voxel Invokers. These components can be added to actors to cause them to generate collision (or navmesh) around themselves.

## 1. Using invokers

Generation is controlled by **Box Invoker** and **Sphere Invoker** components attached to actors:

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

These invokers can be moved, rotated and scaled as needed.

In order to make a graph actually generate colliders and navmesh data around an invoker, add a **Generate Marching Cube Collision & Navmesh** node.&#x20;

<figure><img src="../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

In order for Navmesh generation to function properly, make sure that navmesh generation is set to Dynamic in the project settings. \


<figure><img src="../.gitbook/assets/image (91).png" alt=""><figcaption></figcaption></figure>

## 2. Debugging collision & navigation

By default, no collision is generated in the editor. You can use `voxel.collision.EnableInEditor 1` in the console to enable collision generation in the editor.

Once this is done, you should be able to see colliders generated when switching the viewport to the Player Collision view:

<figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption><p>A shot from the Player Collision view of a voxel terrain using invoker-based collision. </p></figcaption></figure>

To debug navmesh in the editor, call `voxel.navigation.EnableInEditor 1` in the console.

## 3. Multiplayer logic

Usually, you want to have different behavior on clients & servers. To do so, the plugin let you access the current net mode in the graph.

A typical setup could be:

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>
