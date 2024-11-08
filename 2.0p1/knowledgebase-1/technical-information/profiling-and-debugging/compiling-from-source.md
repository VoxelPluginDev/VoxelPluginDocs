# Compiling From Source

Plugin version available on dev or preview branches on [GitHub](https://github.com/VoxelPlugin/VoxelPlugin) should always compile. The plugin gets built through an internal CI pipeline before changes are merged, which means it is very rare for build errors to slip through. &#x20;

Because of this, compiler errors when building the plugin are usually environment/configuration errors. If plugin compilation fails, make sure to go through _exactly_ the following steps:

{% hint style="info" %}
When trying to solve compiler errors according to this article, do **not** use any IDE other than Visual Studio 2022. Once the errors are resolved, you can use your usual IDE again. &#x20;
{% endhint %}

* Make sure your project is a C++ project - this will not work on a Blueprint project.
* Right-click the project's .uproject and click `Generate Visual Studio project files`
  * If this fails, make sure Visual Studio 2022 is installed and up-to-date.
* Open the project's .sln file in Visual Studio 2022
*   Make sure the Solution Explorer is visible in VS22, and ensure there are no warnings about out-of-date or missing packages.

    * If there are warnings, click them to install any missing components.

    <figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Warnings about missing packages in Visual Studio 2022. Sometimes missing packages can entirely break compilation.</p></figcaption></figure>
* If there are no (remaining) warnings, click the green play button with Local Windows Debugger in the toolbar.

At this point, the compilation process will start and the editor should launch after a while.

{% hint style="warning" %}
If this does not solve the issues you are experiencing, reach out on [Discord](https://discord.voxelplugin.com).
{% endhint %}
