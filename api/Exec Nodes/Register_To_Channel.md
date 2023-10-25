# Register To Channel

<div align="left" data-full-width="false">

<figure><img src="Register_To_Channel.png" alt=""><figcaption></figcaption></figure>

</div>

Voxel Write Channel Exec Node

## Inputs

<table>
<thead><tr><th width="170">Type</th><th width="170">Name</th><th>Description</th></tr></thead>
<tbody>
<tr><td>Boolean</td><td>Enable Node</td><td>If false, the node will never be executed</td></tr>
<tr><td>Channel</td><td>Channel</td><td>Channel</td></tr>
<tr><td>Integer</td><td>Priority</td><td>Priority</td></tr>
<tr><td>Bounds</td><td>Bounds</td><td>Bounds</td></tr>
<tr><td>Surface</td><td>Value</td><td>Value</td></tr>
<tr><td>Float</td><td>Smoothness</td><td>Use this if you're using a SmoothUnion/SmoothIntersection/SmoothSubtraction node and you're seeing glitches
Previous brushes will be queried with additional precision - ie, their bounds will be increase by Smoothness</td></tr>
</tbody>
</table>

## Outputs

<table>
<thead><tr><th width="170">Type</th><th width="170">Name</th><th>Description</th></tr></thead>
<tbody>
<tr><td>Exec</td><td>Exec</td><td>If not connected, will be executed automatically</td></tr>
</tbody>
</table>