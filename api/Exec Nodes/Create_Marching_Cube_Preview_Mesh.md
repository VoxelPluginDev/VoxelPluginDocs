# Create Marching Cube Preview Mesh

<div align="left" data-full-width="false">

<figure><img src="Create_Marching_Cube_Preview_Mesh.png" alt=""><figcaption></figcaption></figure>

</div>

Voxel Marching Cube Preview Exec Node

<table>
<thead><tr><th width="250">Type</th><th width="200">Name</th><th>Description</th></tr></thead>
<tbody>
<tr><td>Boolean</td><td>Enable Node</td><td>If false, the node will never be executed</td></tr>
<tr><td>Surface</td><td>Surface</td><td>Surface</td></tr>
<tr><td>Boolean</td><td>Only Draw if Selected</td><td>Disable if this is a sculpt tool</td></tr>
<tr><td>Bounds</td><td>Bounds</td><td>If not set will be automatically computed from Surface</td></tr>
<tr><td>Integer</td><td>Size</td><td>The size of the mesh to create in voxels</td></tr>
<tr><td>Material</td><td>Material</td><td>Material</td></tr>
<tr><td>Body Instance</td><td>Body Instance</td><td>Body instance for the collision, ignored in editor worlds</td></tr>
<tr><td>Exec</td><td>Exec</td><td>If not connected, will be executed automatically</td></tr>
</tbody>
</table>