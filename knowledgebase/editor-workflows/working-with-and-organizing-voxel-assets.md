# Working With & Organizing Voxel Assets

### Thumbnail Customization

Many of the asset types added by Voxel Plugin have support for thumbnail customization. This allows you to configure custom icons and colors to be used in the Content Browser and asset pickers. Use this to make it easy to tell apart different assets of the same type.

Thumbnail customization is currently supported on these asset types:

* Metadata
* Layers
* Voxel Graphs

To edit the thumbnail settings for Metadata and Layer assets, simply open the asset. A section with thumbnail settings will be immediately visible.

To edit the thumbnail settings on a Voxel Graph, open the graph asset and make sure the Main Graph is selected in the top-left. The details panel (bottom-right) should now show various settings, including the thumbnail customization.

### Voxel Stamp Drawer & Tags

Voxel stamps can be easily placed using the Voxel Stamp Drawer, which can be opened by pressing `Ctrl-Q`. This works exactly like the Content Browser drawer, but instead shows every kind of voxel stamp in your project. This includes stock stamps, like the Shape ones, but it also includes every Voxel Graph and Voxelized Mesh Asset.&#x20;

{% hint style="info" %}
Non-drawer versions of this panel can be added to the editor layout manually.&#x20;
{% endhint %}

By default, this drawer sorts assets only based on their type (Shape, Graph, Voxelized Mesh, etc.), but it also supports user filters. To use those filters, tags can be assigned to any stamp asset. Just open any stamp asset, and there should be a tag field somewhere in the asset settings. In the case of Voxel Graphs, make sure the Main Graph is selected.
