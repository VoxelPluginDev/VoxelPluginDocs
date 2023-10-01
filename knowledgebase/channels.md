---
description: Fundamentals on how channels work and how they are used.
---

# Channels

Channels are used to send data from one voxel graph to another, and can also be [queried by gameplay logic](blueprints/querying-voxel-graphs.md).

## 1. How Channels Work

**Channels do not store or cache any data**. The data is **re-generated with each query**. This means generation times can quickly increase if queried repeatedly.

A channel is at its core just a list of nodes to call with bounds, in a particular order (decided by priority).&#x20;

When called, a **Register to Channel** node will add the node plugged into its Value pin to the selected channel's list of nodes. Its position in the list is based on the assigned priority.

When called, a **Query Channel** node will run the highest-priority node (up to its Max Priority, see image below) in a channel's node list. It only considers nodes whose bounds overlap the current position.

<figure><img src="../.gitbook/assets/image (136).png" alt=""><figcaption><p>This graph writes into a channel, but reads what the state of that same channel. The Max Priority being set to this brush' priority makes sure it will not be affected by brushes that are supposed to come after it. </p></figcaption></figure>

We do tend to call this 'reading' or 'writing' data for convenience, but that is not technically correct; the outputs are never stored or cached anywhere.

## 2. Channel Management

Channels can be stored in two ways.&#x20;

By default, they are part of the Voxel Plugin section in the Project Settings. By default, each project starts with a Surface channel configured.&#x20;

When portability or multi-user collaboration is needed, storing data in a single file can be a problem. To deal with that, channels can also be stored in Voxel Channel Registry assets. There is no limit to the amount of Channel Registries that can exist in a project, so these can be used wherever convenient without drawbacks. &#x20;

Channels can be managed from the place they are stored (the Project Settings or a Channel Registry asset). From there, channels can be created, renamed (this will break existing references), and their types and defaults can be changed. Note that a channel can be either a [uniform or a buffer](graph-design/buffers-and-uniforms.md), and this is also configurable here.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption><p>Channels can be managed from the place they are stored.</p></figcaption></figure>

Alternatively, channels can be managed from the channel management dropdown which appears when clicking a channel field. From this widget, channels can be added from scratch in any location, as well as have their type and defaults edited by clicking the pen icon in the list, regardless of where they're stored.&#x20;

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption><p>The channel management dropdown (shown here with add/edit section expanded by clicking the arrow) streamlines channel setup.</p></figcaption></figure>
