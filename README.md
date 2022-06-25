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

This role requires at least Ansible `v2.8.0`. To install it run:

```Shell
ansible-galaxy install ganto.acme_tiny
```


### Documentation

The role documentation is available online at [gantoacme-tiny.readthedocs.io](https://gantoacme-tiny.readthedocs.io).

It can be built locally from the [docs](docs/) directory by running:
```Shell
cd docs && make html
```


### Development

#### Testing

There is a [Molecule](https://molecule.readthedocs.io/) test profile that can
be used to verify the basic functionality of the role. The default scenario is
using the [podman](https://podman.io/) container driver. If you prefer
[docker](https://www.docker.com/) you can select the corresponding scenario
with the `-s docker` molecule arguments.

1. Ensure you have the necessary dependencies installed (e.g. in a Python
   [venv](https://docs.python.org/3/tutorial/venv.html)):
```
pip install -r molecule/default/requirements.txt        # for podman
# or
pip install -r molecule/docker/requirements.txt         # for docker
```
2. Run the test suite. The options in brackets are optional but useful if you
   need to troubleshoot issues:
```
molecule [-vvv] test [--destroy never][-s docker]
```
3. If you used `--destroy never` the container will remain after the test run
   and can be inspected interactively via:
```
podman exec -it <container-id> /bin/sh                  # for podman
# or
docker exec -it <container-id> /bin/sh                  # for docker
```
4. Once you're done with inspecting the instance it has to be deleted before
   a new test run can be started:
```
molecule destroy [-s docker]
```


### Author

The `acme_tiny` Ansible role was written by:

- [Reto Gantenbein](https://linuxmonk.ch/) | [e-mail](mailto:reto.gantenbein@linuxmonk.ch) | [GitHub](https://github.com/ganto)

License: [GPLv3](https://tldrlegal.com/license/gnu-general-public-license-v3-%28gpl-3%29)
