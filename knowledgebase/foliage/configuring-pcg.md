# Configuring PCG

When using PCG with Voxel Plugin, we strongly recommend:

* On any PCG graph:
  * Setting `Use Hierarchical Generation` to true.
    * Note that PCG's spline sampling does not work well with Hierarchical Generation. Use Voxel Metadata instead.
* On any in-world graph actor:
  * Setting `Generation Trigger` to `Generate at Runtime`.
  * Setting `Is Partitioned` to true.
* On the PCG World Actor:
  * Setting `Treat Editor Viewport as Generation Source` to true.
  * If the world isn't largely a heightfield (i.e. it contains lots of caves, or is a planet), setting `Use 2DGrid` to false.
* On your player character:
  * Add a PCG Generation Source component.
    * This lets the Hi-Gen system know where to generate points.&#x20;
