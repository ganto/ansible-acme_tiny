Changelog
=========

**ganto.acme_tiny**

This project adheres to `Semantic Versioning <https://semver.org/spec/v2.0.0.html>`_
and `human-readable changelog <https://keepachangelog.com/en/0.3.0/>`_.

The current role maintainer is `ganto <ganto@linuxmonk.ch>`_.


`ganto.acme_tiny master`_ - unreleased
--------------------------------------

.. _ganto.acme_tiny master: https://github.com/ganto/ansible-acme_tiny/compare/v0.1.4...master

Added
~~~~~

- ``Makefile`` for building documentation

Changed
~~~~~~~

- Update intermediate CA certificate to 'Let’s Encrypt R3'
- Don't add Root CA certificate to certificate chain
- Fix user check condition to be compatible with Ansible 2.10
- Fix ignored variable :envvar:`acme_tiny__cert_symlink` when checking if
  certificate symlinks should be made
- Fix bug where the RSA private key file system group was updated in case the
  certificate was generated for a service defined in :envvar:`acme_tiny__service`
  which resulted in the :envvar:`acme_tiny__user_name` not being able to read
  the key anymore.
- Update full path of :program:`systemctl` path used to restart services after
  certificate updates from ``/usr/bin`` to ``/bin`` to be compatible with more
  Linux distributions such as Debian.
- Reload instead of restart services after certificate update
- Use Ansible ``openssl_privatekey`` module instead of :program:`openssl` to
  generate RSA private key
- Use fully qualified collection name (FQCN) for Ansible modules
- Require Ansible version 2.8


`ganto.acme_tiny v0.1.4`_ - 2020-06-23
--------------------------------------

.. _ganto.acme_tiny v0.1.4: https://github.com/ganto/ansible-acme_tiny/compare/v0.1.3...v0.1.4

Changed
~~~~~~~

- Fix ``search`` filter converted to test in Ansible 2.9


`ganto.acme_tiny v0.1.3`_ - 2019-11-30
--------------------------------------

.. _ganto.acme_tiny v0.1.3: https://github.com/ganto/ansible-acme_tiny/compare/v0.1.2...v0.1.3

Changed
~~~~~~~

- Fix ``changed`` filter removed in Ansible 2.9


`ganto.acme_tiny v0.1.2`_ - 2019-09-07
--------------------------------------

.. _ganto.acme_tiny v0.1.2: https://github.com/ganto/ansible-acme_tiny/compare/v0.1.1...v0.1.2

Added
~~~~~

- New variable :envvar:`acme_tiny__cert_backup` allows to disable backup of
  existing certificates. Defaults to ``True``.

Changed
~~~~~~~

- Don't overwrite existing certificate when running ``acme-tiny``. First create a
  temporary file and only copy certificate in place after validation.


`ganto.acme_tiny v0.1.1`_ - 2018-09-23
--------------------------------------

.. _ganto.acme_tiny v0.1.1: https://github.com/ganto/ansible-acme_tiny/compare/v0.1.0...v0.1.1

Added
~~~~~

- New variable :envvar:`acme_tiny__ca_directory_url` which allows customization
  of certificate authority directory URL. Defaults to "Let's Encrypt".

Changed
~~~~~~~

- Switch "Let's Encrypt Authority X3" intermediate certificate from IdenTrust
  cross-signed to ISRG Root X1 signed, as this CA is now accepted by all major
  browsers.

- Enable customization of certificate chain via :envvar:`acme_tiny__ca_chain`
  configuration variable.


ganto.acme_tiny v0.1.0 - 2016-10-04
-----------------------------------

Added
~~~~~

- Initial release [ganto]
