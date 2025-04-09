# Flat Normals & Blocky Rendering

The plugin has support for basic blocky rendering. To make use of this, tick the Blockiness box on your Voxel World.&#x20;

{% embed url="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FWdLLHg9aFXUtKtWlKWMP%2Fuploads%2FtLAjxPnDsk1w03ZMC7eP%2F2025-03-04%2019-04-13.mp4?alt=media&token=14bf3ea2-ddd2-4ef0-864e-3f983f1b2617" %}
A mesh stamp with shape disabled and Blockiness metadata assigned, being dragged around a world with Blockiness enabled.
{% endembed %}

The world won't immediately turn blocky; instead, the world will turn blocky in places that have Blockiness metadata assigned. This can be done in a lot of different ways, but the easiest is to place a mesh stamp, disable Shape in the behavior toggles and add an entry called "Blockiness" with a value of 1 to the Metadata list.

{% hint style="info" %}
To avoid visual glitches, the Flat Normals toggle on the Voxel World should (in almost all cases) be enabled when using blocky rendering. Note that flat normals will cause significant seams when using Nanite Tessellation.&#x20;

Flat normals will, in a future release, be controllable through Metadata instead of being a global toggle.
{% endhint %}

