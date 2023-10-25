# Generate Point Collision

<div align="left" data-full-width="false">

<figure><img src="Generate_Point_Collision.png" alt=""><figcaption></figcaption></figure>

</div>

Voxel Point Collision Exec Node

<table>
<thead><tr><th width="250">Type</th><th width="200">Name</th><th>Description</th></tr></thead>
<tbody>
<tr><td>Boolean</td><td>Enable Node</td><td>If false, the node will never be executed</td></tr>
<tr><td>Chunked Point Set</td><td>Chunked Points</td><td>ChunkedPoints</td></tr>
<tr><td>Body Instance</td><td>Body Instance</td><td>BodyInstance</td></tr>
<tr><td>Name</td><td>Invoker Channel</td><td>InvokerChannel</td></tr>
<tr><td>Float</td><td>Distance Offset</td><td>Will be added to invoker radius</td></tr>
<tr><td>Integer</td><td>Chunk Size</td><td>In cm, granularity of collision
Try to keep high enough to not have too many chunks</td></tr>
<tr><td>Name</td><td>Attributes to Cache 0</td><td>AttributesToCache</td></tr>
<tr><td>Boolean</td><td>Generate Overlap Events</td><td>If true, this component will generate overlap events when it is overlapping other components (eg Begin Overlap).
Both components (this and the other) must have this enabled for overlap events to occur.</td></tr>
<tr><td>Boolean</td><td>Multi Body Overlap</td><td>If true, this component will generate individual overlaps for each overlapping physics body</td></tr>
<tr><td>Double</td><td>Priority Offset</td><td>Priority offset, added to the task distance from camera
Closest tasks are computed first, so set this to a very low value (eg, -1000000) if you want it to be computed first</td></tr>
<tr><td>Exec</td><td>Exec</td><td>If not connected, will be executed automatically</td></tr>
</tbody>
</table>