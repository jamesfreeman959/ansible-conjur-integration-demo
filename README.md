# ansible-conjur-integration-demo
This repository contains simple example code to demonstrate the integration of CyberArk Conjur and Ansible.

It is based on the code provided in this example: https://www.conjur.org/get-started/interactive/secure-ansible-automation/ - however it contains a number of fixes and notes on pitfalls.

# Architecture Requirements
The example code here assumes that you have:
* a Conjur server set up and running - this is a great example to get going quickly: https://github.com/cyberark/conjur-quickstart 
* A host to run the [Conjur CLI](https://github.com/cyberark/conjur-cli-go 
) on - this should be separate from the other hosts listed here to preven tissues.
* An Ansible Control node, with Ansible >= 2.9 installed
* At least one node to manage with Ansible

# Conjur Server configuration
The code and commands provided here assume you have set up a Conjur account called `demo`. You will need to set this up on the Conjur server - if you are running it on Docker, open a shell into the container with a command like:
```
$ docker exec -it <ID of Conjur container> /bin/bash
```

Then issue this command to create the account:
```
conjurctl account create demo
```

**Be sure to save the output of this command - you will need it later on!**

# Policy setup
Proceed to [01-conjur-policy](01-conjur-policy/)

# Ansible integration
Proceed to [02-ansible-control-node](02-ansible-control-node/)
