# Make Invoker Chunk Spawner

<div align="left" data-full-width="false">

<figure><img src="Make_Invoker_Chunk_Spawner.png" alt=""><figcaption></figcaption></figure>

</div>

Make Invoker Chunk Spawner

## Inputs

<table>
<thead><tr><th width="170">Type</th><th width="170">Name</th><th>Description</th></tr></thead>
<tbody>
<tr><td>Integer</td><td>LOD</td><td>LOD</td></tr>
<tr><td>Float</td><td>World Size</td><td>WorldSize</td></tr>
<tr><td>Name</td><td>Invoker Channel</td><td>InvokerChannel</td></tr>
<tr><td>Integer</td><td>Chunk Size</td><td>ChunkSize</td></tr>
<tr><td>Integer</td><td>Group Size</td><td>Distance checks will be done for chunks in bulk
Bulk query size (actual amount will be cubed)
Will also affect render granularity, if too many chunks are spawned at once reduce this</td></tr>
</tbody>
</table>

## Outputs

<table>
<thead><tr><th width="170">Type</th><th width="170">Name</th><th>Description</th></tr></thead>
<tbody>
<tr><td>Chunk Spawner</td><td>Spawner</td><td>Spawner</td></tr>
</tbody>
</table>