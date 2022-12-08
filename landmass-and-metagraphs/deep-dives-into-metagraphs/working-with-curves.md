# Working with Curves

{% hint style="info" %}
This page is a stub. This means that the information on this topic will be significantly expanded in the future. What is currently on this page is an incomplete picture.
{% endhint %}

Curves operate on a non-normalized data source. If your noise has a range from -1 to 1 (so a a single octave with an amplitude of 1 - amplitude is range at first octave), make sure your curve also goes from -1 to 1.&#x20;

{% hint style="info" %}
An example on noise range, assuming gain - the multiplier for each octave's amplitude compared to the previous octave) is set to 0.5.&#x20;

Octave 0's range will be -10k to 10k, octave 1 will add -5k to 5k on top of that, coming to a total range of -15k to 15k, octave 2 will add -2.5k to 2.5k on top again, coming to -17.5k to 17.5k, and so on for each octave.
{% endhint %}

If your noise has an amplitude of 10,000, and several octaves, you will want to make sure your curve ranges from something more along the lines of -20,000 to 20,000.&#x20;
