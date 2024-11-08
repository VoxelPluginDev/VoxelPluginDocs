# Displace Surface

<div align="left" data-full-width="false">

<figure><img src="../../../.gitbook/assets/Displace_Surface.png" alt=""><figcaption></figcaption></figure>

</div>

Displace a surface by some amount Useful to eg add noise to a surface

## Inputs

<table><thead><tr><th width="170">Name</th><th>Description</th></tr></thead><tbody><tr><td>Surface</td><td>Surface</td></tr><tr><td>Displacement</td><td>In local space ie, if this brush is scaled up, will increase accordingly</td></tr><tr><td>Max Displacement</td><td>Max amount to displace by, used to compute bounds. Displacement will be clamped between -MaxDisplacement and +MaxDisplacement Will increase bounds, so try to keep small</td></tr></tbody></table>

## Outputs

<table><thead><tr><th width="170">Name</th><th>Description</th></tr></thead><tbody><tr><td>New Surface</td><td>NewSurface</td></tr></tbody></table>
