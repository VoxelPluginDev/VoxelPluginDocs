# Collision, Navmesh & Invokers

By default, visibility collision is enabled for the terrain. This can be disabled on the Voxel World. Make sure to do this when using collision invokers, otherwise the terrain will create two overlapping colliders.

Collision and Navmesh Invokers can be used to control where non-visibility collision and navmesh data is generated.&#x20;

Simply add an invoker component to an actor that needs to have collision/nav data around itself, and the plugin will automatically generate the data as needed. Invokers do not need to be manually registered. Position changes do not need to be manually registered either.
