---
description: The possibilities in working with distance fields versus pure meshes.
---

# Complex Usecases for Landmass Brushes

{% hint style="info" %}
This page is a stub. This means that the information on this topic will be significantly expanded in the future. What is currently on this page is an incomplete picture.
{% endhint %}

Landmass brushes are a distance field - this means you can use them as a mask as well, simply divide by your desired falloff, potentially add or subtract a uniform value to grow or shrink the mask and then use it as alpha in a lerp.
