---
icon: hammer-crash
---

# Sculpt Stamps

Sculpt data is stored on a stamp actor set to the Sculpt type (either Volume or Height). The stamp can then be moved around or deleted while keeping its sculpt data (mostly) intact. This also means the terrain can be changed around a sculpt stamp (i.e. other stamps near it can be moved) without the sculpt data being entirely ruined.

To sculpt, add a stamp actor to your scene and set it to a sculpt type. Assign a resolution on the stamp as desired.&#x20;

{% hint style="info" %}
If the resolution is lower than the voxel world's, the data will be interpolated. In a lot of cases, your sculpts can be (quite a bit) lower resolution than your voxel world's voxel size.
{% endhint %}

With a sculpt stamp added to your scene, use the Mode switcher in the top left of the main editor window to switch to Voxel Sculpt mode. A panel will appear. In that panel, select the Stamp actor you want to sculpt onto (you can have multiple separate sculpt stamps in your scene). A handful of different tools will appear. You can use these to sculpt onto the world.

{% hint style="warning" %}
Like any other stamps, sculpt stamps to a layer with a priority. Make sure this is set to something appropriate!&#x20;

If you're not sure what to do with layers and priorities, just set the sculpt stamp's priority to a high number to start.
{% endhint %}

