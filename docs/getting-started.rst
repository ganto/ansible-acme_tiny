Getting started
===============

.. contents::
   :local:

There are two "modes" how this role can be run:

- *Scheduler mode*: Role will request new certificate based on an existing
  certificate request, replace the old certificate and restart the affected
  service. Ansible should be run with dedicated minimally privileged user
  account (by default ``certbot``).

- *Setup mode*: Role will run the initial setup for a new domain certificate
  such as create required directories, generate RSA key and certificate
  request. Further it will make sure that a dedicated user acount for the
  scheduler mode is created and install the necessary :program:`sudo` rules
  for the service restart. Role has to be run with ``root`` privileges. 


Prerequisites
-------------

Let's Encrypt Account Key
^^^^^^^^^^^^^^^^^^^^^^^^^

Before the role can be run to send certificate requests an account key has
to be generated. This can be done with the official `Certbot
<https://certbot.eff.org/>`_ client. Make sure the key is converted into the
correct format for :program:`acme-tiny` as described in `Use existing Let's
Encrypt key <https://github.com/diafygi/acme-tiny#use-existing-lets-encrypt-key>`_.

Eventually store the account key in :file:`/etc/ssl/acme-tiny/account.key`.


Web Server Configuration
^^^^^^^^^^^^^^^^^^^^^^^^

When requesting the certificate :program:`acme-tiny` will place a challenge
file in :file:`/var/www/acme-challenges` which has to be accessible through
``http://<fqdn>/.well-known/acme-challenge`` for every domain requested in
the certificate. Make sure you add a corresponding definition in your Web
server configuration.

**Apache 2**

.. code-block:: apacheconf

    Alias /.well-known/acme-challenge/ /var/www/acme-challenges/
    <LocationMatch "/.well-known/acme-challenge/*">
        Header set Content-Type "text/plain"
    </LocationMatch>

**Nginx**

.. code-block:: nginxconf

    location /.well-known/acme-challenge {
        alias /var/www/acme-challenges;

        location ~ /.well-known/acme-challenge/(.*) {
            default_type text/plain;
        }
    }

**Lighttpd**

.. code-block:: lighttpdconf

    alias.url += (
        "/.well-known/acme-challenge/" => "/var/www/acme-challenges/",
    )


Example playbook
----------------

A minimal playbook which would run the ``ganto.acme_tiny`` role to request a
SSL certificate would looke like this:

.. literalinclude:: playbooks/acme_tiny.yml
   :language: yaml


Example inventory
-----------------

When using the `example playbook`_ the host to run the role has to be added
to the ``[acme_tiny]`` host group in the Ansible inventory::

    [acme_tiny]
    hostname

Obviously, the :doc:`defaults` might not be suitable for everybody. Especially
the :envvar:`acme_tiny__domain` variable is likely to be defined individually.
This can be done via host variables in
:file:`/etc/ansible/host_vars/<hostname>/acme_tiny.yml`.

If there are multiple certificates that should be managed with this Ansible
role the individual configurations would be defined in separate "domain"
files (e.g. :file:`/etc/ansible/vars/<domain>.yml`) and then passed with the
Ansible ``--extra-vars`` argument to the playbook run.

..
 Local Variables:
 mode: rst
 ispell-local-dictionary: "american"
 End:
