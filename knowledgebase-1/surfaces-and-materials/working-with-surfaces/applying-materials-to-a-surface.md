---
description: How a voxel terrain's material and surface workflows work together.
---

# Applying Materials to a Surface



<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>A graph applying two materials weighted by noise to a surface.  </p></figcaption></figure>

When merging two surfaces using the **Smooth Union**, **Smooth Subtraction** or **Smooth Intersection** nodes, the materials (and other properties applied to the surface) are blended alongside the distance field. The weights are based on the distance divided by the smoothness.&#x20;

<figure><img src="../../../.gitbook/assets/image (9) (1).png" alt=""><figcaption><p>Two brushes being blended with a high smoothness, creating a large transition area.</p></figcaption></figure>
