---
description: Voxel Plugin 2 licensing details
---

# Licensing

With the release of Voxel Plugin 2 getting closer, we are now ready to announce the new licensing structure that will accompany it. We go into more details below, but the main changes are:

* Voxel Plugin 2 will be sold directly through our website, instead of being sold through the Unreal Marketplace. A license will still cost $349, excluding regional sales taxes such as VAT.
* Buying a license will give you perpetual access to the plugin, and entitles you to one year of feature updates, three years of bug fixes and three years of Unreal upgrades.
* Licenses will be shareable within reason, rather than being per-seat. Projects with a budget over $100k USD need to contact us for separate terms.

{% hint style="info" %}
This isn't subscription licensing. When you buy a license, you get access to the current Voxel Plugin version **forever**, with bug fixes & engine upgrades included.
{% endhint %}

We're making these changes for multiple reasons. The first one is sustainability: so far we have relied on userbase growth for revenue, and that's not sustainable long term. Our company being sustainable isn't just important for our own sake - it's critical for the plugin's viability. We need to support the plugin long term so all the games using it can rely on the plugin working as expected.

The second reason we're making these changes is so we can increase the plugin's stability. In the past, it's been hard to pick a stable Voxel Plugin version as we did not provide engine or stability fixes for old versions. This would force users to always upgrade to the latest, potentially breaking, version.

With Voxel Plugin 2, we will have a much cleaner versioning system with long-term supported versions. You will be able to pick a version that works for you, knowing it will be supported for the years to come.

While making these changes, we also want to make sure to thank all of our existing customers. We wouldn't be where we are now without your support, and we know waiting for Voxel Plugin 2 to release has been tough. In this spirit, **all existing Unreal Marketplace & Gumroad customers will be granted a Voxel Plugin 2 license - including a full year of updates - when it releases**.

We detail these changes more in-depth below. As always, feel free to reach out on [Discord](https://discord.voxelplugin.com) or by [email](mailto:contact@voxelplugin.com) if you have any questions!

## What buying a license gives you

> Buying a license will give you perpetual access to the plugin, and entitles you to one year of feature updates, three years of bug fixes and three years of Unreal upgrades

### What's a feature update?

Voxel Plugin 2 will use a versioning system similar to Unreal's: `2.Major.Minor`. A change of the major version, e.g. switching from 2.3 to 2.4, is referred to as a feature update.

Just like with Unreal, a new major release means new features, and potential breaking changes when upgrading.

A change of the minor version, e.g. switching from 2.3.2 to 2.3.3, is a minor update. A minor update might fix bugs or add compatibility for a new Unreal release. A minor update will never have breaking changes and won't add new features.

### What does three years of bug fixes mean?

As long as a major release came out while you had an active license, you will get access to all its minor releases without having to buy a new license.

For at least three years after a release, we will do our best to backport all bug fixes to it. It is possible that in some cases it won't be possible to do so, in which case we'll try to provide an alternative fix or workaround.

{% hint style="info" %}
For example:&#x20;

You buy a license on February 1st 2024. Voxel Plugin 2.5 releases on January 30th 2025.&#x20;

Since it's within one year from your purchase, you get access to it and all of its minor releases:

* If 2.5.1 releases in March 2025, you will get access to it for free.
* If we find a critical bug in 2028 affecting 2.5, we will release a new minor version fixing it and you'll have access to it at no additional cost.
{% endhint %}

### What does three years of Unreal upgrades mean?

We commit to upgrading all of Voxel Plugin 2 major releases to the latest Unreal release for at least three years. Support for new Unreal releases will be distributed in minor releases and will be included at no additional cost.

{% hint style="info" %}
For example:

We release Voxel Plugin 2.1 in December 2023, supporting only Unreal 5.2 and 5.3.

If Epic releases 5.7 in December 2026, we commit to upgrading Voxel Plugin 2.1 to also support 5.7 as it's within three years from its release.

That upgrade will be part of a minor release (eg, 2.1.5) so if you have access to 2.1, you will have access to it without additional purchases.
{% endhint %}

We believe this to be important for stability as well: if you're happy with the feature set of a specific Voxel Plugin release, we don't want to force you to upgrade or to buy a new license.

{% hint style="info" %}
For example:

You integrate Voxel Plugin 2.1 in your game in December 2023, using Unreal 5.2.

You can release your game in 2026, still using Voxel Plugin 2.1 and using the latest Unreal release at the time (eg, 5.8).

You can do so without ever renewing your license, for a fixed cost of $349.
{% endhint %}

### Is this just another subscription?

We've tried hard to find a middle ground between single sale and subscription plans. From the vendor's side, subscriptions are an excellent way to hook customers and generate continuous revenue, allowing for sustainable development.

The unfortunate part of that equation from the user side is, of course, that subscriptions are expensive. In our case, it especially punishes those that are developing their games for a long time - or just for themselves as a hobby - as they'll continuously need to keep subscribing. It also means there is no incentive for the vendor (in this case, us) to keep improving their software, as users are forced to pay to keep using the software regardless of improvements.

As we outlined above, the sustainability part of this equation is very important for us, as our current model won't keep us going forever. At the same time, we don't want to provide a poor value proposition by forcing users into an ongoing cost.

The model we outline in the rest of this article allows us to have more recurring revenue from our users, as long as those users are happy with the work we are doing. If a user is happy with what they already have, they don't have to pay for the plugin ever again, and if a user is unimpressed with or uninterested in what we're releasing in our updates, they can simply hold off until we do release things they're interested in.

## Moving off the Unreal Marketplace

We have been selling the plugin on the Unreal Marketplace since April 2020. The Marketplace has been an incredible platform for us to grow as a company, but its limitations are constricting our options when it comes to sustainable growth.

Most notably, the new licensing scheme would be impossible to implement on top of the Marketplace. We'd also like exploring more complex licensing options like having Voxel Plugin Pro trials down the road, which would also be impossible to do through the Marketplace.

Moving forward, you'll be able to purchase Voxel Plugin 2 directly on our website. We'll have more info about this soon, but we can already tell you we'll be using [Paddle](https://www.paddle.com/) as payment processor. If you have any feedback on this, please reach out!

## License sharing

Technically speaking, plugins sold on the Unreal Marketplace are per-seat. We've never enforced that rule, and moving forward we are making it explicit that you're allowed to share the plugin when working on a project.

We do ask that you keep such sharing reasonable: don't share your Voxel Plugin 2 license with a dozen projects at once, and don't join a project just to give it your Voxel Plugin license.

{% hint style="info" %}
For example:

* I'm working on a project with a few friends, and I already own the plugin. I'm allowed to share it with them as long as I actively contribute to the project.
* Some people on discord are asking me to join their project so I can give them my Voxel Plugin license. I'm only allowed to do so if I plan to actively contribute to the project afterwards.
{% endhint %}

## Budget cap

If your game has a budget over $100k USD, you won't be able to use the $349 version of the plugin. Please reach out to us at contact@voxelplugin.com instead, and we'll setup a custom license matching your specific use case.

We can also offer dedicated support through Slack along with custom licenses.

### What if my game ends up earning more than $100k?

First off, congratulations! As long as your initial budget before the first release of your game using the plugin was under $100k, you can still use the $349 license and don't owe us anything. If you are unsure whether you can keep using your license, just reach out to contact@voxelplugin.com.

If you'd like to setup a custom license to get additional support afterwards, that's also totally possible.

{% hint style="info" %}
For example:

* You make a game with a budget of $45k. The plugin is a core part of your game, so you buy it early in development. After release, you earn $250k in game sales. Since that money is earned post-release and you were already using the plugin before, you don't owe us anything. Nonetheless, we'd love to hear from you and see what you've made with the plugin!
* You make a game with a budget of $45k. The plugin isn't part of your game at first, and you release the game as early access. Following that, you earn $250k in sales and decide to push a new update to your game that adds the plugin. Since your budget is now $295k, you are not allowed to use the $349 version and must reach out to contact@voxelplugin.com.
{% endhint %}



