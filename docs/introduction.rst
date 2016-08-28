Introduction
============

This `Ansible <https://ansible.com>`_ role will setup `acme-tiny
<https://github.com/diafygi/acme-tiny>`_, a minimal ACME client for the
`Let's Encrypt <https://letsencrypt.org>`_ certificate authority. It also
automates the creation of certificate requests and renewal of certificates,
including service restart after the certificates have been replaced.


Installation
~~~~~~~~~~~~

This role requires at least Ansible ``v2.0.0``. To install it, run:

.. code-block:: console

    ansible-galaxy install ganto.acme_tiny

..
 Local Variables:
 mode: rst
 ispell-local-dictionary: "american"
 End:
