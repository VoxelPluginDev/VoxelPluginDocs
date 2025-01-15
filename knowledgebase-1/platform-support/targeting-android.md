# Targeting Android

{% hint style="danger" %}
Android support is experimental. If issues appear, please reach out on [Discord](https://discord.voxelplugin.com).
{% endhint %}

Android currently requires building the plugin from source.

In order to so, you will need to add this to your Project.Target.cs:

```
if (Target.Platform == UnrealTargetPlatform.Android)
{
	bCompileISPC = true;
	bOverrideBuildEnvironment = true;
}
```

You'll also need to integrate this PR (only tested on 5.2): [https://github.com/EpicGames/UnrealEngine/pull/10746](https://github.com/EpicGames/UnrealEngine/pull/10746)
