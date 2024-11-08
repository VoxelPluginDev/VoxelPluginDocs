---
description: Installing the Voxel Plugin sample content shipped with each release.
---

# Installing Example Content

Each Voxel Plugin release ships with a set of content examples designed to showcase plugin systems and how to utilize them. By default, none of these content examples are present on the plugin installation. Instead, they are downloaded from a server on demand.

&#x20;The Voxel Content can be accessed by right-clicking in the content browser, and selecting the `Add Voxel Content` option near the top.

<figure><img src="../.gitbook/assets/image (165).png" alt=""><figcaption><p>Right-clicking in the content browser will bring up the option to add Voxel Content.</p></figcaption></figure>

Once clicked, a dialog showing the various examples currently available on the server will pop up. Selecting one of these examples will show a description of the contents of the example, after which clicking `Add to Project` will start the installation process.&#x20;

<figure><img src="../.gitbook/assets/image (74).png" alt=""><figcaption><p>Selecting an example will show a description of its contents.</p></figcaption></figure>

If the content you are trying to add to your project is already installed, you will be asked to overwrite the current copy in your project. If you made any changes to your current copy, make sure to back them up before click `Yes`. Clicking `No` will still add any content that doesn't already exist in the project, but it will not override files that already exist.

Some content examples depend on each other. If a content example depends on another, it will automatically download and add its dependencies to the project as well. Bear in mind that this will re-download and overwrite existing installations of any dependencies in the project, again meaning any local changes made to those copies may be overwritten.
