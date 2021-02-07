## acme_tiny

[![CI](https://github.com/ganto/ansible-acme_tiny/workflows/CI/badge.svg?event=push)](https://github.com/ganto/ansible-acme_tiny/actions?query=workflow%3ACI)
[![Travis CI](http://img.shields.io/travis/ganto/ansible-acme_tiny.svg?style=flat)](https://travis-ci.org/ganto/ansible-acme_tiny)
[![test-suite](http://img.shields.io/badge/test--suite-ansible--acme__tiny-blue.svg?style=flat)](https://github.com/ganto/ansible-acme_tiny-test/)
[![Ansible Galaxy](http://img.shields.io/badge/galaxy-ganto.acme_tiny-660198.svg?style=flat)](https://galaxy.ansible.com/ganto/acme_tiny/)
[![Platforms](http://img.shields.io/badge/platforms-gentoo-lightgrey.svg?style=flat)](#)

This [Ansible](https://ansible.com) role will setup and renew SSL certificates
from the [Let's Encrypt](https://letsencrypt.org) certificate authority with
help of the minimal [acme-tiny](https://github.com/diafygi/acme-tiny) Python
client. It can be used to generate and send the initial certificate request as
well as run by e.g. a cron job for regularly renewing the certificate and
restart the secured services after the certificate has been replaced.


### Getting started

The role requires at least Ansible `v2.7.0`. To install it, run:

```bash
ansible-galaxy install ganto.acme_tiny
```

### Documentation

More information about `ganto.acme_tiny` Ansible role can be found at the
[official documentation](https://gantoacme-tiny.readthedocs.io).


### Authors and license

`acme_tiny` role was written by:

- [Reto Gantenbein](https://linuxmonk.ch) | [e-mail](mailto:reto.gantenbein@linuxmonk.ch)

License: [GPLv3](https://tldrlegal.com/license/gnu-general-public-license-v3-%28gpl-3%29)

