# Variables and Parameters

Variables and parameters can be created in two ways. The first is to click the plus button in the appropriate section on the bottom-left side of the graph editor. The second is to drag out from a pin and select `Promote to Local Variable` or `Promote to Parameter`.&#x20;

`Promote to Local Variable` will create a local variable - this will also show up on the left side of the graph editor. The details panel right above the variable will let you configure the variable's default value. You can get the value of a local variable as many times as you want. You can also set a local variable once from within your graph, overriding the default value.&#x20;

If a value needs to be editable per Voxel Actor in your world, rather than from within the graph, use a parameter instead; these function like variables, but they will show up in your Voxel Actor's details panel rather than being set within the graph.

Parameters can only be uniforms, but local variables can be uniform or buffer depending on their usage. More information on when to use which can be found on the [buffers-and-uniforms.md](../../landmass-and-metagraphs/deep-dives-into-metagraphs/buffers-and-uniforms.md "mention") page.
