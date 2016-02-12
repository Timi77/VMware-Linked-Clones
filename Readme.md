#VMware Linked Clone manage script (PowerCLI)

This script provides a mass cloning and deleting functionalities for the Linked Clones VMs

This script consist of 2 parts:
Part 1 provides the ability to create a Linked Clones VMs
Part 2 provides the ability to delete all Linked Clones VMs from selected parent VM. It can only delete VMs that was created using this script

##How to use it

- Change your vCenter address and login credentials at the top of the script
(change variables: $Lserver, $Luser, $Lpass)

- If you don't want to load the VM names from a csv file there is a second option. The script will generate VMs names from 3 elements: prefix + NUMBER + suffix. The prefix and suffix parts are easy (you can leave them blank if you want). The sequence of numbers is constructed from ranges x-y and items x,y,z... The script provides you a small menu where you can combine any numbers of ranges and items tohether. This way you can easily deploy a VMs with names that are not part of one continual sequence of numbers in one go.

- You can run this script (Part 1) multiple times against one parent VM and every time the script adds Linked Clones VMs from actual state of that parent.
But by running Part 2 of this script against a parent VM you will have no option but to delete all the "childs" of that parent VM (granularity of deleton I will integrate later, I hope...)

- Os customisation works only with OS customisation templates that don't ask for additional user inputs

- For static IP's on guestOS you need to have defined OS customization template in vCenter. During run of this script select an OS customization template and than you can enter first IP address and other IP details. All datails in network section of that OS customization template will be ignored and replaced by the static IP values. The IP address will be incremented by +1 for each VM.
Please note that this function is in early state (currently does not support: more than one network adapter in VM, the IP incrementation is "dummy", it does not care about proper IP ranges (it will increment the last digits over 255), user inputs are not validated)

- If you don't have trusted certificate on your vCenter server run this command:
Set-PowerCLIConfiguration -InvalidCertificateAction Ignore

- If you don't see any datastores in the menu, maybe it's because of the 'listOnlyMultiHostDS' option. Try to change it in the script in 'settings area'.

##How it works

Because there is no easy way how to tell if VM is a linked clone this script uses tags for storing this informations.
Every parent VM has assigned tag with name "Childs-{VMname}". In description of this tag there are all names of Linked Clones for this parent (separated by ;). This list of VMs is used by Part 2 during deletion. You can edit this tag description by hand in vCenter for manual control of links between Parent and "childs".


##Features

- fully interactive = no need to write all data by hand, you can choose from menu by numbers
- Option for Automatic host selection (by free RAM)

##To Do

- Os customisation (Make it work with OS customisation templates that have user inputs)
- Create groups (labels) during creation of VMs and then add ability to delete by groups