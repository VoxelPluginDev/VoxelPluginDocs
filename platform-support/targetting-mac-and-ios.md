# Getting started on Mac & iOS

Mac currently requires building the plugin from source.

In order to so, you will need to add this to your Project.Target.cs and ProjectEditor.Target.cs:

```
bCompileISPC = true;
```



For iOS, you'll need to do the same. If you're using 5.1 or earlier, you'll also need to backport this CL: [https://github.com/EpicGames/UnrealEngine/commit/7b4a384ba802074927456e30722a9788226680c0](https://github.com/EpicGames/UnrealEngine/commit/7b4a384ba802074927456e30722a9788226680c0)t
