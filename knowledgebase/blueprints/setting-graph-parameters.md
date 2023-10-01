# Setting graph parameters

You can set/get graph parameters on any voxel actor or component. Get the voxel parameter container from the voxel actor/component, and use it as input for the **Get/Set Voxel Parameter** parameter collection pins.

<figure><img src="../../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>

The Asset pin on these nodes does not need to be used for the node to function. When the Asset pin is used, the Parameter field will become a dropdown, showing a list of the parameters, and the node will automatically update its Value pin to match the parameter's type.&#x20;

When it is unused, the Parameter field will simply show a name field, and the user must ensure that a parameter with that name exists in the parameter collection, and that said parameter's type is the same as the node's Value pin. If any of these are not the case, the node's operation will fail and an error will be thrown.&#x20;
