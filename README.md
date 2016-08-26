## acme_tiny

<!-- This file was generated by Ansigenome. Do not edit this file directly but
     instead have a look at the files in the ./meta/ directory. -->

[![Platforms](http://img.shields.io/badge/platforms-gentoo-lightgrey.svg?style=flat)](#)

This [Ansible](https://ansible.com) role will setup and renew SSL certificates
from the [Let's Encrypt](https://letsencrypt.org) certificate authority with
help of the minimal [acme-tiny](https://github.com/diafygi/acme-tiny) Python
client. It can be used to generate and send the initial certificate request as
well as run by e.g. a cron job for regularly renewing the certificate and
restart the secured services after the certificate has been replaced.


### Getting started

#### Role installation

Either clone the role into your `roles/` directory:

```bash
git clone https://github.com/ganto/ansible-acme_tiny /etc/ansible/roles/ganto.acme_tiny
```

Or install it via `ansible-galaxy`:

```bash
ansible-galaxy install ganto.acme_tiny
```

#### acme-tiny installation

As there are different ways to setup `acme-tiny` on the various distributions
this task is not covered by this Ansible role and you have to do it manually.
If there is no package already provided by your distribution, you can also
install it via
[pip](https://pypi.python.org/pypi/acme-tiny). If by chance you are running
Gentoo, checkout the related
[ebuild](https://github.com/ganto/linuxmonk-overlay/tree/master/app-crypt/acme-tiny)
in my [linuxmonk-overlay](https://github.com/ganto/linuxmonk-overlay).


#### Account key

Before you can run the role and send certificate requests, you need to generate
an account key. This can be done with the official
[Certbot](https://certbot.eff.org/) client. Make sure the key is in the correct
format for `acme-tiny` as described in
[Use existing Let's Encrypt key](https://github.com/diafygi/acme-tiny#use-existing-lets-encrypt-key).

Store the account key at `/etc/ssl/acme-tiny/account.key`.


#### Web server configuration

When requesting the certificate, `acme-tiny` will place a challenge file in
a directory which has to be served on `http://<fqdn>/.well-known/acme-challenge`
for every domain requested in the certificate. Make sure you add a corresponding
definition in your Web server configuration.

* **Apache 2**:

```apacheconf
Alias /.well-known/acme-challenge/ /var/www/acme-challenges/
<LocationMatch "/.well-known/acme-challenge/*">
  Header set Content-Type "text/plain"
</LocationMatch>
```

* **Nginx**:

```
location /.well-known/acme-challenge {
    alias /var/www/acme-challenges;

    location ~ /.well-known/acme-challenge/(.*) {
        default_type text/plain;
    }
}
```

* **Lighttpd**:

```
alias.url += (
    "/.well-known/acme-challenge/" => "/var/www/acme-challenges/",
)
```


#### Example playbook

A minimal playbook which would run the `ganto.acme_tiny` role to request a
SSL certificate would looke like this:

```YAML
---

- hosts: localhost
  gather_facts: False

  vars:
    acme_tiny__domain: [ 'example.com', 'www.example.com' ]
    acme_tiny__cert_type: 'lighttpd'

  roles:
    - role: ganto.acme_tiny
```

The first time you run the role for a new domain, `ansible` must be run with
the `root` user to setup the necessary environment:

* create configuration, challenge, domain, certificate directories

* create unprivileged Linux account `certbot` for later certificate renewal

* create `~certbot/.ansible.cfg` so that Ansible runs are logged to individual
  log file `/var/log/acme-tiny/certbot.log`.

* add sudo rule service restart by `certbot` account

* create 4096 bit key and certificate request of the defined domains

* creates a PEM certificate file and depending on the service an individual
  service certificate file.

* restart the service


#### Certificate renewal

If you have different domains, you might want to configure the individual
certificate properties in a separate `vars/` file. After the initial setup,
the following command which can be run with the dedicated `certbot` account
will get you a new certificate. E.g. you can define a cronjob
`/etc/cron.d/acme-tiny`:

```bash
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

@monthly certbot /usr/bin/ansible-playbook -e @/etc/ansible/vars/example.com.yml /etc/ansible/playbooks/acme_tiny.yml >/dev/null
```

### Configuration

#### Directory layout

The role will setup a configurable directory layout to store the certificates
and make them accessible.

```txt
/etc/ssl/acme-tiny                                   # acme_tiny__config_dir
/etc/ssl/acme-tiny/example.com                       # acme_tiny__cert_dir
/etc/ssl/acme-tiny/example.com/example.com.key       # acme_tiny__private_key
/etc/ssl/acme-tiny/example.com/example.com.crq       # acme_tiny__cert_request
/etc/ssl/acme-tiny/example.com/example.com.crt       # acme_tiny__certificate
```

There is a layer of indirection through symlinks which will make the
certificates accessible in a unified way. For each service the role will
create a `ssl/` subdirectory from where the actual certificates and keys are
symlinked. Like this the CA could be changed easily without reconfiguration
of the secured services.

E.g. for Lighttpd, this would look like this:

```txt
/etc/lighttpd/ssl/example.com.crt -> /etc/ssl/acme-tiny/example.com/example.com_lighttpd.crt
```

Or for Dovecot:

```txt
/etc/postfix/ssl/example.com.crt -> /etc/ssl/acme-tiny/example.com/example.com.crt
/etc/postfix/ssl/example.com.key -> /etc/ssl/acme-tiny/example.com/example.com.key
```

#### Role variables

List of default variables available in the inventory:

```YAML
# Basic configuration
# -------------------

# Configuration base directory
acme_tiny__config_dir: '/etc/ssl/acme-tiny'


# Directory accessible through a HTTP server used for temporary challenge
# storage.
acme_tiny__challenge_dir: '/var/www/acme-challenges'


# Log directory.
acme_tiny__log_dir: '/var/log/acme-tiny'


# Log file for renewal process.
acme_tiny__log_file: '{{ acme_tiny__log_dir }}/{{ acme_tiny__user_name }}.log'


# Length of the private key, in case new key is generated
acme_tiny__key_length: 4096


# Domain configuration
# --------------------

# Domain for which certificate is requested. Can be string or list.
acme_tiny__domain: 'example.com'


# File name of key, certificate request and certificate (without ending).
acme_tiny__cert_name: '{{ acme_tiny__domain[0]
                          if (acme_tiny__domain is iterable and not acme_tiny__domain is string)
                          else acme_tiny__domain }}'


# Directory name where key, certificate requtest and certificate are stored
# for this domain.
acme_tiny__cert_dir: '{{ acme_tiny__config_dir }}/{{ acme_tiny__cert_name }}'


# Private key used for certificate request. Will be generated if not existant.
acme_tiny__private_key: '{{ acme_tiny__cert_dir }}/{{ acme_tiny__cert_name }}.key'


# Certificate request. Will be generated if not existant.
acme_tiny__cert_request: '{{ acme_tiny__cert_dir }}/{{ acme_tiny__cert_name }}.csr'


# Certificate which will be generated.
acme_tiny__certificate: '{{ acme_tiny__cert_dir }}/{{ acme_tiny__cert_name }}.crt'


# List of output certificate type(s). String or list. Can be one of ``plain``,
# ``apache2``, ``dovecot``, ``httpd``, ``lighttpd``, ``postfix``.
acme_tiny__cert_type: 'plain'


# User configuration
# ------------------

# User account used for running acme-tiny

# User name.
acme_tiny__user_name: 'certbot'


# Primary group of functional user.
acme_tiny__user_group: '{{ acme_tiny__user_name }}'


# Home directory.
acme_tiny__user_home: '/var/lib/acme-tiny'


# Renewal setup
# -------------

# Account key.
acme_tiny__account_key: 'account.key'

```


### Authors and license

`acme_tiny` role was written by:

- [Reto Gantenbein](https://linuxmonk.ch) | [e-mail](mailto:reto.gantenbein@linuxmonk.ch)

License: [GPLv3](https://tldrlegal.com/license/gnu-general-public-license-v3-%28gpl-3%29)

