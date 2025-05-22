# Multiplayer Support

Generation is deterministic, which means worlds will be generated identically across servers and different clients as long as the input parameters are the same.

To avoid prediction issues for custom Characters, use the VoxelCharacter class as base class.

Add [invoker components](collision-navmesh-and-invokers.md) to player actors to make sure collision is generated around all players. Using the default visibility collision will cause desync between the server and clients.
