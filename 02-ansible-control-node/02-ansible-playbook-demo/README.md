# Introduction
The details provided below assume that you have completed the setup in the root of this repository. If you have not, please return and complete this now.

# Running the playbook
The playbook provided in this directory performs a very simple operation - it obtains the values for the `ansible_host`, `ansible_user` and `ansible_ssh_pass` from the Conjur server via the policy we uploaded previously, removing the need to save such sensitive data in a separate file.

Although we uploaded the policy, we must set the variable values before we run the playbook. Below are some examples for you - be sure to set them to the correct values for your environment. You will need to run these on the node where you installed the Conjur CLI:
```
$ conjur variable set --id server/conjur-ansible-node01.example.com/host -v "192.168.1.10"
$ conjur variable set --id server/conjur-ansible-node01.example.com/user -v "ansiblectl"
$ conjur variable set --id server/conjur-ansible-node01.example.com/password -v "Sup3rS3cr3t!"
```

If you want to retrieve your values, you can check then with a command such as:
```
$ conjur variable get --id server/conjur-ansible-node01.example.com/password 
```

Once you have stored your variable values, simply run the playbook as follows:
```
$ ansible-playbook -i inventory playbook.yml 
```

# LetsEncrypt/ACME TLS Certificates
One gotcha exists with the Ansible collection for Conjur and the use of LetsEncrypt/ACME certificates if you are using them on the Conjur server. These will not work with the `lookup` plugin out of the box (at the time of writing). If you are using these, then you will need to set the following environment variable before running the playbook. The exact path of the CA chain will vary depending on your OS, but as long as you set the `CONJUR_CERT_FILE` to the location of your central CA Trust Store, the `lookup` plugin will work.
```
$ export CONJUR_CERT_FILE=/etc/ssl/certs/ca-certificates.crt 
```