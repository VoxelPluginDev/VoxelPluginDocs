# Extending MetaGraphs through C++

{% hint style="info" %}
This page is a stub. This means that the information on this topic will be significantly expanded in the future. What is currently on this page is an incomplete picture.
{% endhint %}

We don't currently have any documentation diving into low-level MetaGraph API, other than [a-technical-exploration-of-metagraphs.md](design-philosophy/a-technical-exploration-of-metagraphs.md "mention"). We intend to write documentation on implementing basic new nodes, both pure C++ CPU node as well as ISPC and GPU-compatible nodes.

We also intend to cover when you should extend the graph with your own nodes, as using multiple  stock nodes will often be dramatically faster than using a single pure C++ function to do the same.
