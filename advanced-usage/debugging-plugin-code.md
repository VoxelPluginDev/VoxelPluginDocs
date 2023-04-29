# Debugging plugin code

By default, the plugin is always compiled with optimizations enabled. If you'd like to debug the plugin, you need to do the following:

* Make sure the plugin is in your project (not engine), and that you're in DebugGame
* Create an empty text file in `%appdata%\Unreal Engine` named `VoxelDevWorkflow_Debug.txt`
* Rebuild the plugin. This will also disable Unity build for the plugin, so you should see >600 actions.
* You can check VOXEL\_DEBUG is enabled in the engine startup log: `VOXEL_DEBUG=1` will be printed
* The code handling this logic is here: [https://github.com/VoxelPlugin/VoxelPlugin/blob/dev/Source/VoxelCore/VoxelCore.Build.cs#L31](https://github.com/VoxelPlugin/VoxelPlugin/blob/dev/Source/VoxelCore/VoxelCore.Build.cs#L31)
