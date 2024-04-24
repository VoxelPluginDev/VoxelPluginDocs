---
description: >-
  Going through the process of how to install Voxel Plugin to an existing
  project.
---

# Installing Voxel Plugin

{% hint style="info" %}
This release is targeted at Unreal Engine 5.2 and 5.3.
{% endhint %}

{% hint style="danger" %}
In order to install the 2.0 preview, ownership of Voxel Plugin Pro Legacy is required.
{% endhint %}

### 1. Downloading Voxel Plugin Installer from the Unreal Engine Marketplace

To start off, open the Epic Games Launcher. In the Unreal Engine section, navigate to the Marketplace and use the search-bar in the top-right to search for “Voxel Plugin Installer”. Find the “Voxel Plugin Installer” result, with “Voxel Plugin” as creator, and click it.

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

* With the store page open, click the “Free” button to get access to the plugin
* You will be prompted to log in if you aren’t already, and then to accept the Epic Content License Agreement
* After doing this, the "Free" button will have changed to an "Install to Engine" button
* Press that, select the engine version you are working in the slot dropdown and press "Install"
* The plugin will now download and install.&#x20;

<figure><img src="../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

### 2. Setting up Voxel Plugin Installer

Once this has finished, start the project you want to use Voxel Plugin in. In this project, go to "Edit -> Plugins" to open the Plugin browser.

<figure><img src="../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

### 3. Installing Voxel Plugin

With the plugin window open, use the search bar to look for "Voxel Plugin Installer". If it does not appear, try restarting the engine and looking again.

Tick the checkbox to the left of the Voxel Plugin Installer icon, and press the "Restart Now" button in the bottom right.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

After restarting, a Voxel Plugin icon will have appeared on the right-hand side of your editor's toolbar, next to the "Settings" dropdown.&#x20;

<figure><img src="../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

Click the Voxel Plugin icon and a dropdown menu will appear. In this menu, click the "Login" button. This will open a login page on the Voxel Plugin site in your browser. Create an account if you do not have one yet, verify your email address and log into your account.

Once logged in, you will be taken to the account dashboard. Click the "Connect" button in the Epic Games Account section.&#x20;

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

On the page that opens, log into the Epic account on which you own Voxel Plugin Pro. Click "Allow" on the Epic Games Account pop-up; these permissions allow the plugin to remember your Epic ID, which is needed to verify ownership.&#x20;

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Once finished, the page will close and you will be taken back to the account dashboard, where you will now see your Epic Games Account being successfully linked.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Click the Verify button. This will take you to the Vault section of the dashboard.&#x20;

Open the [Voxel Plugin Pro page on the Marketplace](https://pro.voxelplugin.com), go to the Questions tab, and press "Ask a Question". Enter "Verification" as both summary and description and press the "Post Your Question" button. This page can now be closed.&#x20;

Back in the Dashboard's Vault section, click the Verify button again. The "Voxel Plugin Pro Legacy" section should update with a green banner, saying "Purchase verified".

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
After verifying plugin ownership, you can connect accounts for other services to your Voxel Plugin account from the Profile section of the Account Dashboard.

Aside from your Epic account, you can connect the following:

* &#x20;a Github account to access source code for both Legacy and VP2.
* a Discord account to access additional channels on [the Voxel Plugin Discord](https://discord.voxelplugin.com).&#x20;
{% endhint %}

With this done, go back to the Unreal Engine editor and click the Voxel Plugin icon again. The menu should now be different, allowing you to download Voxel Plugin 2. Select a version to install, whether you want it to be installed to the engine, and press the Install button.

{% hint style="warning" %}
If you have Voxel Plugin Pro Legacy installed on the engine version you're using:

* Voxel Plugin 2 **must** be installed to the project. Untick the "Install in engine" box.&#x20;
* Voxel Plugin Pro Legacy **must** be disabled in this project's plugin list. Only one version of Voxel Plugin can be active at once.
{% endhint %}

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Once the download is finished, you will be prompted to restart the engine. After restarting the engine, the plugin will be installed.

{% hint style="info" %}
If you originally purchased the plugin on Gumroad, reach out to Phyronnaz on [Discord](https://discord.voxelplugin.com).
{% endhint %}

And with that, you are finished - time to start creating!
