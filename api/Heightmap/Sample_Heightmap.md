# Sample Heightmap

<div align="left" data-full-width="false">

<figure><img src="Sample_Heightmap.png" alt=""><figcaption></figcaption></figure>

</div>

Will clamp position if outside of the heightmap bounds
Heightmap is centered, ie position is between -Size/2 and Size/2
@param        BicubicSmoothness       Smoothness of the bicubic interpolation, between 0 and 1

## Inputs

<table>
<thead><tr><th width="170">Type</th><th width="170">Name</th><th>Description</th></tr></thead>
<tbody>
<tr><td>Heightmap Ref</td><td>Heightmap</td><td>Will clamp position if outside of the heightmap bounds
Heightmap is centered, ie position is between -Size/2 and Size/2
@param        BicubicSmoothness       Smoothness of the bicubic interpolation, between 0 and 1</td></tr>
<tr><td>Vector 2D Buffer</td><td>Position</td><td>Will clamp position if outside of the heightmap bounds
Heightmap is centered, ie position is between -Size/2 and Size/2
@param        BicubicSmoothness       Smoothness of the bicubic interpolation, between 0 and 1</td></tr>
<tr><td>Heightmap Interpolation Type</td><td>Interpolation</td><td>Will clamp position if outside of the heightmap bounds
Heightmap is centered, ie position is between -Size/2 and Size/2
@param        BicubicSmoothness       Smoothness of the bicubic interpolation, between 0 and 1</td></tr>
<tr><td>Float</td><td>Bicubic Smoothness</td><td>Smoothness of the bicubic interpolation, between 0 and 1</td></tr>
</tbody>
</table>

## Outputs

<table>
<thead><tr><th width="170">Type</th><th width="170">Name</th><th>Description</th></tr></thead>
<tbody>
<tr><td>Float Buffer</td><td>Return Value</td><td>Will clamp position if outside of the heightmap bounds
Heightmap is centered, ie position is between -Size/2 and Size/2
@param        BicubicSmoothness       Smoothness of the bicubic interpolation, between 0 and 1</td></tr>
</tbody>
</table>