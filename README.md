# ansible

This is written to by run on the remote machine itself. Either by the `ansible-playbook` command or by utilizing the [EC2 Systems Manager Run Command and State Manager](https://aws.amazon.com/blogs/mt/running-ansible-playbooks-using-ec2-systems-manager-run-command-and-state-manager/) to automate it all.

```bash
$ ansible -i production webservers -m setup
$ ansible-playbook site.yml -i production --become
```

There are a number of workarounds specifically aimed at `Amazon Linux 2` AMIs, so be sure to use that rather than any other distro.

## Deploying

Here are all the environmental variables required (with some example values).

```bash
SECRET_KEY="12345678901234567890123456789012345678901234567890"

DB_NAME="postgres"
DB_USER="postgres"
DB_PASSWORD=""
DB_HOST="localhost"
DB_PORT="5432"

REDIS_CLIENTID=""
REDIS_PASSWORD=""
REDIS_HOST="localhost"
REDIS_PORT="6379"
REDIS_DB="0"

MEDIA_ROOT="/elite-dodgeball/media"
STATICFILES_DIRS="/elite-dodgeball/static"

MAILGUN_API_KEY="key-12345678901234567890123456789012"

STRIPE_API_KEY="sk_test_123456789012345678901234"
STRIPE_PUBLIC_KEY="pk_test_123456789012345678901234"
```

Make sure they are in `/etc/environment` and loaded into the session or whatever you decide before running `ansible` or it'll probably bomb out at `codebase : Make media directory`.

This could be automated by using EC2's [`user data`](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html) feature.

```bash
#!/bin/bash

yum update -y
yum install -y git

amazon-linux-extras install -y ansible2

cat > /etc/environment << 'EOF'
...
EOF
```
