# Targeting Mac & iOS

{% hint style="danger" %}
Mac & iOS support are experimental. If issues appear, please reach out on [Discord](https://discord.voxelplugin.com).
{% endhint %}

Mac currently requires building the plugin from source.

In order to so, you will need to add this to your Project.Target.cs and ProjectEditor.Target.cs:

```
bCompileISPC = true;
bOverrideBuildEnvironment = true;
```

If you're using 5.2, you'll also need to integrate this PR: [https://github.com/EpicGames/UnrealEngine/pull/10562](https://github.com/EpicGames/UnrealEngine/pull/10562)

## Using a Launcher engine

Only Unreal Engine 5.1 is currently supported if you're not building the engine from source.

Extract the following files under `UE_5.1/Engine/Source/ThirdParty/Intel/ISPC/`: [https://drive.google.com/file/d/11WY797J\_olw6ptlbH0KKOYwdoWdFR56A/view?usp=share\_link](https://drive.google.com/file/d/11WY797J_olw6ptlbH0KKOYwdoWdFR56A/view?usp=share_link)

Make sure you have `.../Intel/ISPC/bin/Mac/ispc`

## IOS

For iOS, you'll need to do the same. If you're using 5.1 or earlier, you'll also need to backport this CL: [https://github.com/EpicGames/UnrealEngine/commit/7b4a384ba802074927456e30722a9788226680c0](https://github.com/EpicGames/UnrealEngine/commit/7b4a384ba802074927456e30722a9788226680c0)
