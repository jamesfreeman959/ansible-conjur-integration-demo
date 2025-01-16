# Introduction
The details provided below assume that you have completed the setup in the root of this repository. If you have not, please return and complete this now.

# Obtaining the Host Factory Token
The instructions provided below will need to be run on the Ansible Control node. However, before this, you will need to run the following command on the host on which you installed and authenticated the Conjur CLI:
```
$ conjur hostfactory tokens create --hostfactory-id ansible
[
  {
    "expiration": "2025-01-10T21:55:09Z",
    "cidr": [],
    "token": "xxxxxxxxxxxxxx"
  }
]
```

You will see that this gives you an invite token which is valid for (by default) 5 minutes. If you do not use it within this time, you will need to create a new token by running the command given above again.

# Granting the Conjur ID
Once you have generated the token above, run the following commands on the Ansible Control node.

Install the Ansible Collection for Conjur:
```
$ ansible-galaxy collection install cyberark.conjur
```

Now download the Conjur TLS certificates (assuming you are using TLS) - use a command like this:
```
$ openssl s_client -showcerts -connect conjur.example.com:443 < /dev/null 2> /dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > /tmp/conjur.pem
```

Now set the `HFTOKEN` environment variable with the token you received from the `conjur hostfactor` command above:
```
$ export HFTOKEN="xxxxxxxxxxxxxx"
```

Now you can run the included playbook. Be sure to edit the `inventory` file and put the FQDN of your Ansible control node into it. You will also need to set up suitable SSH authentication for this host (e.g. SSH key).
```
$ ansible-playbook -i inventory grant_conjur_id.yml --become
```
Add:
* `--ask-pass` if you are using password authentication for the control node
* `--ask-become-pass` if your control node requires a password for `sudo` access

When the playbook runs successfully, the Ansible control node is set up as a host within the Conjur server, with the access and roles provided to it through the policies you uploaded. This host status does **not** expire when the host factor token expires - that is required purely for adding the host.

# Revoking the Conjur ID
In the event that you want to revoke the host status and remove the configuration, run the following playbook:
```
$ ansible-playbook -i inventory revoke_conjur_id.yml --become
```