# Working with Metadata

Metadata lets you set float-based values per voxel, by name. This data can then be passed into materials through vertex attributes (UVs or Vertex Color), and can be queried by name through C++ and Blueprint for gameplay purposes.&#x20;

Stamps can write their own information into these named Metadata values. Graphs can be used to write complex values into Metadata.

#### Passing  Metadata to Materials

<table data-header-hidden data-full-width="false"><thead><tr><th></th><th></th><th data-hidden></th></tr></thead><tbody><tr><td><p>Metadata can be passed into materials by configuring the Materials Channels settings on the Voxel World. A single Metadata attribute can be tied (by name) to a single channel of a vertex-attribute (Vertex Color or a UV).</p><p>The Metadata can then be accessed by getting those vertex attributes using the standard material nodes (VertexColor / TexCoord).<br><br><br><br></p></td><td><img src="../../.gitbook/assets/image (3).png" alt="" data-size="original"></td><td></td></tr></tbody></table>

