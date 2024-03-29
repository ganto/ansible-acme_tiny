Introduction
============

This `Ansible <https://www.ansible.com>`_ role will setup `acme-tiny
<https://github.com/diafygi/acme-tiny>`_, a minimal ACME client for the
`Let's Encrypt <https://letsencrypt.org>`_ certificate authority. It also
automates the creation of certificate requests and renewal of certificates,
including service restart after the certificates have been replaced.


.. _acme_tiny_installation:

Installation
~~~~~~~~~~~~


.. _acme_tiny_ansible_role:

Ansible Role
^^^^^^^^^^^^

This role requires at least Ansible ``v2.8.0``. To install it, run:

.. code-block:: console

    ansible-galaxy install ganto.acme_tiny


.. _acme_tiny_upstream:

acme-tiny
^^^^^^^^^

As there are different ways to setup :program:`acme-tiny` on the various
distributions this task is not covered by the Ansible role. It has to be done
manually prior to running the role.

If there is no package provided by the distribution of your choice, it can
also be installed from `PyPi <https://pypi.org/project/acme-tiny/>`_.

.. code-block:: console

    pip install acme-tiny

..
 Local Variables:
 mode: rst
 ispell-local-dictionary: "american"
 End:
