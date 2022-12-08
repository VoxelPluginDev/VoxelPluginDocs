---
description: >-
  This article explores basic graph UX and provides pointers on how to get the
  most out of the graph editor's quality of life features.
---

# Using MetaGraphs

{% hint style="info" %}
This page is a stub. This means that the information on this topic will be significantly expanded in the future. What is currently on this page is an incomplete picture.
{% endhint %}

### 1. Working with nodes

When working with MetaGraphs, there may well be times where you are unsure what node to use. If you are unsure what to plug into a pin, drag out from it onto the graph and let go. The search menu will only show compatible nodes, making it easier to find what you're looking for.

### 2. Variables and parameters

`Promote to Local Variable` will create a local variable on the left side of the graph. The details panel right above the variable will let you configure the variable's default value.&#x20;

If this value needs to be editable per Voxel Actor in your world, promote it to a `Parameter` instead; these function like variables, but they will show up in your Voxel Actor's details panel rather than being set within the graph.
