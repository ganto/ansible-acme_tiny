## Ansible Role: ganto.acme_tiny

[![CI](https://github.com/ganto/ansible-acme_tiny/workflows/CI/badge.svg?event=push)](https://github.com/ganto/ansible-acme_tiny/actions?query=workflow%3ACI)
[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-ganto.acme__tiny-blue.svg?style=flat&logo=ansible)](https://galaxy.ansible.com/ganto/acme_tiny)
[![Read the Docs](https://img.shields.io/badge/docs-gantoacme--tiny-darkblue.svg?style=flat&logo=read-the-docs)](https://gantoacme-tiny.readthedocs.io/)

This [Ansible](https://ansible.com) role will setup and renew SSL certificates
from the [Let's Encrypt](https://letsencrypt.org) certificate authority with
help of the minimal [acme-tiny](https://github.com/diafygi/acme-tiny) Python
client. It can be used to generate and send the initial certificate request as
well as run by e.g. a cron job for regularly renewing the certificate and
restart the secured services after the certificate has been replaced.


### Installation

The role requires at least Ansible `v2.7.0`. To install it run:

```bash
ansible-galaxy install ganto.acme_tiny
```


### Documentation

More information about `ganto.acme_tiny` Ansible role can be found at the
[official documentation](https://gantoacme-tiny.readthedocs.io).


### Author

The `acme_tiny` Ansible role was written by:

- [Reto Gantenbein](https://linuxmonk.ch) | [e-mail](mailto:reto.gantenbein@linuxmonk.ch) | [GitHub](https://github.com/ganto)

License: [GPLv3](https://tldrlegal.com/license/gnu-general-public-license-v3-%28gpl-3%29)
