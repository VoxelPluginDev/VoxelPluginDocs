# Cellular Noise 2D

<div align="left" data-full-width="false">

<figure><img src="Cellular_Noise_2D.png" alt=""><figcaption></figcaption></figure>

</div>

Two dimensional cellular noise

## Inputs

<table>
<thead><tr><th width="250">Type</th><th width="200">Name</th><th>Description</th></tr></thead>
<tbody>
<tr><td>Vector 2D Buffer</td><td>Position</td><td>Position at which to calculate output noise</td></tr>
<tr><td>Float Buffer</td><td>Amplitude</td><td>Amplitude</td></tr>
<tr><td>Float Buffer</td><td>Feature Scale</td><td>FeatureScale</td></tr>
<tr><td>Float Buffer</td><td>Jitter</td><td>How irregular the cells are, needs to be between 0 and 1 to avoid glitches</td></tr>
<tr><td>Seed Buffer</td><td>Seed</td><td>Used to randomize the output noise</td></tr>
</tbody>
</table>

## Outputs

<table>
<thead><tr><th width="250">Type</th><th width="200">Name</th><th>Description</th></tr></thead>
<tbody>
<tr><td>Float Buffer</td><td>Value</td><td>Distance to nearest cell center at position</td></tr>
</tbody>
</table>