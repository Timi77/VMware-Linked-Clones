#VMware Linked Clone manage script (PowerCLI)

This script provides a mass cloning and deleting functionalities for the Linked Clones VMs

This script consist of 2 parts:
Part 1 provides the ability to create a Linked Clones VMs
Part 2 provides the ability to delete all Linked Clones VMs from selected parent VM. It can only delete VMs that was created using this script

##How to use it

-Change your vCenter address and login credentials at the top of the script
(change variables: $Lserver, $Luser, $Lpass)

-The script will generate VMs names from 3 elements: prefix + NUMBER + suffix
The prefix and suffix parts are easy (you can leave them blank if you want)
The sequence of numbers is constructed from ranges x-y and items x,y,z. The script provides you a small menu where you can combine any numbers of ranges and items tohether. This way you can easily deploy a VMs with names that are not part of one continual sequence of numbers in one go.

-You can run this script (Part 1) multiple times against one parent VM and every time the script adds Linked Clones VMs from actual state of that parent.
But by running Part 2 of this script against a parent VM you will have no option but to delete all the "childs" of that parent VM (granularity of deleton I will intergate later, I hope...)

-Os customisation works only with OS customisation templates that don't ask for additional user inputs

-If you don't have trusted certificate on your vCenter server run this command:
Set-PowerCLIConfiguration -InvalidCertificateAction Ignore

-If you don't see any datastores in the menu, maybe it's because of the 'listOnlyMultiHostDS' option. Try to change it in the script in 'settings area'.

##How it works

Because there is no easy way how to tell if VM is a linked clone this script uses tags for storing this informations.
Every parent VM has assigned tag with name "Childs-{VMname}". In description of this tag there are all names of Linked Clones for this parent (separated by ;). This list of VMs is used by Part 2 during deletion. You can edit this tag description by hand in vCenter for manual control of links between Parent and "childs".


##Features

- fully interactive = no need to write all data by hand, you can choose from menu by numbers
- Option for Automatic host selection (by free RAM)

##To Do

-Os customisation (Make it work with OS customisation templates that have user inputs)
-Create groups (labels) during creation of VMs and then add ability to delete by groups