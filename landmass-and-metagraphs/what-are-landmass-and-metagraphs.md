---
description: >-
  This article explores the design framework behind Voxel Plugin 2.0, why it
  exists and how it evolved from previous iterations of the plugin.
---

# What are Landmass & MetaGraphs

### 1. Voxel Plugin’s initial design <a href="#block-f11edb69e82b4b5bb3d30f0a00cc7309" id="block-f11edb69e82b4b5bb3d30f0a00cc7309"></a>

Voxel Plugin 1.0 was our first deep dive into working with voxel technology. Working on VP 1.0 through VP 1.2 was an incredible journey, and taught us a lot about where the power of voxels really lie.

Because VP 1.0 was our first foray into the tech, however, the foundation of what we built was, to put it bluntly, not the best it could’ve been.

The easiest explanation for the source of many of 1.0’s issues is simply that its codebase was monolithic. Rather than having a modular system design where different systems can easily communicate with each other, all plugin systems were hardcoded to do specific things, and relied on other hard-coded systems to connect together.

Because of this, implementing new features, changing or improving existing systems, or replacing them entirely became incredibly difficult. For every feature had in the codebase, the complexity of adding or reworking another increased exponentially.

VP 1.2 was progressively built on top of VP 1.0’s foundation. While it added a lot of cool tech, that foundation was flawed and a lot of the systems were disjointed and did not interact well together.

Following the release of VP 1.2, it was clear to us that there was too much systemic overhead to continue working on this foundation. VP 2.0 was born as its successor.

### 2. The new design for VP 2.0 <a href="#block-722d3dc9cd5b49a1908dc1d3c64343c9" id="block-722d3dc9cd5b49a1908dc1d3c64343c9"></a>

The 2.0 update was initially intended as a partial refactor of the plugin’s core system. As we progressed, it slowly became clear that a partial refactor was not going to be enough to meet our targets. Instead, VP 2.0 evolved to become a complete rewrite of VP 1.2’s systems, with many systems being added, removed or replaced to create a more consistent and homogenous plugin.

The initial architecture decided on for VP 2.0 was a subsystem framework; a subsystem would be a mostly self-contained system responsible for a single specific task (for instance, turning a distance field into a mesh), with hooks for other subsystems it’s designed to work with. This design premise fixed the monolithic design limitations of VP 1.2, and allowed us to bring VP 2.0 close to the finish line.

When the time came to re-implement graph-based world generation, we knew we wanted users to be able to do the same as in VP 1.2’s voxel graphs, and more on top, while also being faster. As we developed to graph workflow, we realized that if we really wanted to make graphs as flexible as possible, we had to solve the problem of arbitrary data flow within graphs.

### 3. The premise of MetaGraphs <a href="#block-a0fc4c7144f94fee8325082873fe9075" id="block-a0fc4c7144f94fee8325082873fe9075"></a>

Let’s say a user wants to generate a set of points over top of a base heightmap, and then turn areas around those points into, say, swamps. The user will want to change the terrain to be mostly flat, but they’ll also want to do things like spawn fog and water planes in those areas, as well as change foliage spawning.

This poses an issue. Within the original VP 2.0 architecture, each of those tasks is ‘owned’ by a different subsystem. The graph would then have to talk through these subsystems through hard-coded bridging points. As it turns out, while subsystems solved the issue of monolithic architecture, communication between subsystems was still painful. It was vastly easier to create new systems, remove existing ones or make improvements to small sections of the codebase without unexpectedly affecting others, but the bridging points at which these systems talked to each other were still hard-coded. This meant every niche combination of subsystems would have to be supported manually.

MetaGraphs were developed as a solution to this problem; instead of hard-wiring subsystems to talk to each other in set ways, all these subsystem actions would become graph nodes. Subsystems still exist as part of the plugin, but are only used as bridges with the Unreal API now; a subsystem will handle pushing a mesh through the renderer, but generating that mesh is handled by graph nodes.

The graph executes like a call-stack, right-to-left from the Root node, with each pin on each node passing a set of information down to the others it calls.

For example, a “Sample Landmass Brushes” node takes a position as input. The Position node used for this does not magically know its position. Instead, that information, called PositionQueryData, has to be provided by another node, such as a “Spawn Chunks” node, to work. If no nodes upstream (to the right) of the Position node were to provide a PositionQueryData, the Position node will throw an error noting it’s not being passed its required QueryData, and it will not execute.

The “[A Technical Exploration of MetaGraphs](a-technical-exploration-of-metagraphs.md)” article goes deeper on these subjects.

This dynamic information dependency and passthrough framework makes it incredibly easy to bridge between different systems in the plugin. If a graph section need to bridge between two systems, the node simply needs to enforce that it requires information from both systems; if it only receives one, it will error out, if it receives both it will work. So long as those systems have a node set up to pass information into graphs in the first place, any set of systems can communicate together through graphs.
