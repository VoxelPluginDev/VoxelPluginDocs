# Targeting Mac & iOS

{% hint style="danger" %}
Mac & iOS support are experimental. If issues appear, please reach out on [Discord](https://discord.voxelplugin.com).
{% endhint %}

Voxel Plugin makes heavy use of ISPC ([https://ispc.github.io/](https://ispc.github.io/)). Unfortunately, ISPC is disabled by default on Mac and iOS.&#x20;

## Using a launcher engine

### Common errors

If you see something like `XXXX.ispc cancelled` in your log, it means the ispc binary isn't getting launched. Make sure you copied it over to the right location and that it is executable (`chmod +x ispc`).

### 5.4.3

To enable ISPC on a launcher engine, you'll need to add the following files:

* **UnrealBuildTool.dll**: Replace the file in `/Users/Shared/Epic Games/UE_5.4/Engine/Binaries/DotNET/UnrealBuildTool/UnrealBuildTool.dll`
* **ispc**: Unzip `ispc.zip` and add the `ispc` binary in `/Users/Shared/Epic Games/UE_5.4/Engine/Source/ThirdParty/Intel/ISPC/bin/Mac/` (the folder hierarchy will not exist, you will need to create it)

You can download them from here: [https://drive.google.com/drive/folders/174FDD9\_DM470weG8xGuk-WwJo97Wd6sL?usp=sharing](https://drive.google.com/drive/folders/174FDD9\_DM470weG8xGuk-WwJo97Wd6sL?usp=sharing)

### Other releases

Other releases are currently not supported - reach out on [Discord](https://discord.voxelplugin.com).

## Using a source engine

If you're building your engine from source, check our fork for the latest changes needed to enable ISPC on Mac & iOS: [https://github.com/Phyronnaz/UnrealEngine/tree/5.4-Mac](https://github.com/Phyronnaz/UnrealEngine/tree/5.4-Mac)
