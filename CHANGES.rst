Changelog
=========

**ganto.acme_tiny**

This project adheres to `Semantic Versioning <http://semver.org/spec/v2.0.0.html>`_
and `human-readable changelog <http://keepachangelog.com/en/0.3.0/>`_.

The current role maintainer is `ganto <ganto@linuxmonk.ch>`_.


`ganto.acme_tiny master`_ - unreleased
--------------------------------------

.. _ganto.acme_tiny master: https://github.com/ganto/ansible-acme_tiny/compare/v0.1.2...master


`ganto.acme_tiny v0.1.2`_ - 2019-09-07
--------------------------------------

.. _ganto.acme_tiny v0.1.2: https://github.com/ganto/ansible-acme_tiny/compare/v0.1.0...v0.1.2

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
