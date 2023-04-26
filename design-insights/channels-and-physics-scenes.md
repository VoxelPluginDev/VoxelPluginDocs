---
description: >-
  How Channels and Physics Scenes are used to pass data between different graphs
  and plugin and engine systems.
---

# Channels & Physics Scenes

From a UX perspective, Channels and Physics Scenes feel largely similar, but there are key differences.

### Channels

Channels are effectively redirectors. They do not come with any form of caching, they simply indicate to the task system which tasks are intended to operate at which times, relative to other tasks.

### Physics Scenes

Similarly to Channels, Physics Scenes function mostly like redirectors. Unlike Channels though, Physics Scenes do come with caching. They are written into at a static level of detail across chunks, allowing chunk outputs to be re-used by any source that might need them.

### Why assets?

Channels and Physics Scenes are, somewhat unintuitively, assets. Most commonly, systems like this use dropdowns, names with autocomplete, or definitions in a settings panel somewhere. Initially, the intent was to do something like that for these systems.&#x20;

Not long after, though, we were faced with the problem of source control; if multiple users across studios are making changes to the channel or physics scene definitions at the same time, and it's all stored in a single file, how do we prevent merge conflicts? The conclusion is that we can't, as there are always edge cases to cover. Instead, we now use assets. It can get a little messy in the Content Brower, but from a practical perspective it covers every edge case and works well with source control.
