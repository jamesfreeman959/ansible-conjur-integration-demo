# Introduction
The details provided below assume that you have completed the setup in the root of this repository. If you have not, please return and complete this now.

# CLI Setup
Install the [Conjur Go CLI](https://github.com/cyberark/conjur-cli-go) on a node of your choosing before proceeding with the rest of this guide.

# Policy installation
All Conjur policies are contained in YAML files, and loaded using the CLI. To load the policies in this directory into your server, you must first authenticate the CLI with your server if you haven't already:
```
$ ./conjur init -u https://conjur.example.com -a demo
The server's certificate fingerprint is XXXXXXXXXXXXXXXXXXXXXXXXX.
Please verify this certificate on the appliance using command:
openssl x509 -fingerprint -noout -in ~conjur/etc/ssl/conjur.pem

? Trust this certificate? Yes
? File /Users/demo/conjur-server.pem exists. Overwrite? Yes
Wrote certificate to /Users/demo/conjur-server.pem
? File /Users/demo/.conjurrc exists. Overwrite? Yes
Wrote configuration to /Users/demo/.conjurrc

$ conjur login -i admin
```

The second command will log into the account you created (assumed here to be `demo`) as the built in `admin` user that was created when you created the account. You will need the API Key provided when you set up the account to complete the login command. You can check if you are successfully authenticated using:
```
$ conjur whoami
{
  "client_ip": "x.x.x.x",
  "user_agent": "Go-http-client/1.1",
  "account": "demo",
  "username": "admin",
  "token_issued_at": "2025-01-16T15:55:35.000+00:00"
}
```

Once you have completed this, you can now load the included policy files with the following commands:
```
$ conjur policy load -b root -f conjur.yml
$ conjur policy load -b ansible -f ansible.yml
$ conjur policy load -b ansible -f server.yml
```

If you update your policy files, note that use of `conjur policy load` is additive - if you remove a policy element, it will **not** be deleted from the server policy. To do this, use the `conjur policy replace` command instead.
