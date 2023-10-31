---
description: Fundamentals on how channels work and how they are used.
---

# Channels

Channels are used to send data from one voxel graph to another, and can also be [queried by gameplay logic](blueprints/querying-voxel-graphs.md).

## 1. How Channels Work

At its core, a channel is just a list of nodes, called in a particular order (decided by priority).&#x20;

When a **Register to Channel** node is called, it will add itself to the assigned channel's list of nodes. Its position in the list is based on the assigned priority.&#x20;

Then, when a **Query Channel** node is called, the most high-priority node in the assigned channel's list will be called. When querying, only nodes whose bounds overlap the current position are processed.

<figure><img src="../.gitbook/assets/image (136).png" alt=""><figcaption><p>This graph writes into a channel, but reads what the state of that same channel. The Max Priority being set to this brush' priority makes sure it will not be affected by brushes that are supposed to come after it. </p></figcaption></figure>

**Channels do not store or cache any data**. The data is **re-generated with each query**. This means generation times can quickly increase if queried repeatedly.

We do tend to call this 'reading' or 'writing' data for convenience, but that is not technically correct; the outputs are never stored or cached anywhere.

## 2. Creating and Modifying Channels

In order for a graph to reference a channel, it needs to first be created somewhere. Channels can be added (and stored) in two places.&#x20;

By default, they are part of the Voxel Plugin section in the Project Settings. Each project starts with a Surface channel configured. More channels can be added here as needed. These channels are stored in a config file.

When something needs to work across projects, or source control is involved, storing data in a per-project config file can be a problem. To get around that, channels can also be stored in Voxel Channel Registry assets. There is no limit to the amount of Channel Registries that can exist in a project, so these can be used wherever convenient without drawbacks. &#x20;

Channels can be managed from the place they are stored (the Project Settings or a Channel Registry asset). From there, channels can be created, renamed (this will break existing references), and their types and defaults can be changed. Note that a channel can be either a [uniform or a buffer](using-graphs/buffers-and-uniforms.md), and this is also configurable here.

<figure><img src="../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption><p>Channels can be managed from the place they are stored.</p></figcaption></figure>

Alternatively, channels can be managed from the channel management dropdown which appears when clicking a channel field. From this widget, channels can be added from scratch in any location, as well as have their type and defaults edited by clicking the pen icon in the list, regardless of where they're stored.&#x20;

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption><p>The channel management dropdown (shown here with add/edit section expanded by clicking the arrow) streamlines channel setup.</p></figcaption></figure>
