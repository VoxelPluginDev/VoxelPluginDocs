# Profiling & Debugging

A common topic of discussion when it comes to solutions for terrain rendering within Unreal Engine is how performance compares to the stock Landscape solution. Unfortunately, it is hard to give a concrete answer to exactly how performance compares. Because of the amount of moving parts involved, we would rather provide the tools needed for users to make an educate decision after testing for their usecase.

Within the plugin, we have several different stat commands to help you nail down the resources used by your worlds. These stats will vary dramatically depending on the (combinations of) nodes used within your MetaGraph.
