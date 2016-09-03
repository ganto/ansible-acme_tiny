System configuration
====================

.. _acme_tiny_ref_fs_layout:

File system layout
------------------

The role will setup a configurable directory layout to store the certificates
and make them accessible by the services.

Example layout of the default configuration:

======================================================= ==================================
Server path                                             Ansible role variable
======================================================= ==================================
:file:`/etc/ssl/acme-tiny`                              :envvar:`acme_tiny__config_dir`
:file:`/etc/ssl/acme-tiny/example.com`                  :envvar:`acme_tiny__cert_dir`
:file:`/etc/ssl/acme-tiny/example.com/example.com.key`  :envvar:`acme_tiny__private_key`
:file:`/etc/ssl/acme-tiny/example.com/example.com.crq`  :envvar:`acme_tiny__cert_request`
:file:`/etc/ssl/acme-tiny/example.com/example.com.crt`  :envvar:`acme_tiny__certificate`
======================================================= ==================================

When setting a :envvar:`acme_tiny__cert_type` other than ``plain`` or ``chain``
an additional layer of indirection through symlinks which will make the
certificates accessible in a transparent way. For each service the role will
create a :file:`ssl/` subdirectory from where the actual certificates and keys
are symlinked. Like this the CA could be changed easily without reconfiguration
of the secured services.

E.g. For for Apache :program:`httpd` this would look like this::

    /etc/apache2/ssl/example.com.crt -> /etc/ssl/acme-tiny/example.com/example.com_chain.crt
    /etc/apache2/ssl/example.com.key -> /etc/ssl/acme-tiny/example.com/example.com.key

For :program:`lighttpd`::

    /etc/lighttpd/ssl/example.com.pem -> /etc/ssl/acme-tiny/example.com/example.com_lighttpd.pem
    /etc/lighttpd/ssl/ca.crt          -> /etc/ssl/acme-tiny/intermediate.crt

For :program:`Dovecot`::

    /etc/dovecot/ssl/example.com.crt -> /etc/ssl/acme-tiny/example.com/example.com.crt
    /etc/dovecot/ssl/example.com.key -> /etc/ssl/acme-tiny/example.com/example.com.key


.. _acme_tiny_ref_service_cfg:

Service configuration
---------------------

To secure a service the key and certificate have to be referenced in the
individual service configurations. When using the symlinks created by the role
this only has to be done once. Any certificate changes and even the change of
a certificate authority can be easily handled by pointing the symlinks to a
new target.

.. note:: The configuration of the certificates in the service configuration
          files has to be done manually.


.. _acme_tiny_ref_apache_cfg:

Apache httpd
~~~~~~~~~~~~

.. code-block:: apacheconf

    SSLCertificateFile    /etc/apache2/ssl/example.com.crt
    SSLCertificateKeyFile /etc/apache2/ssl/example.com.key

- Upstream documentation:
  `Apache Module mod_ssl <https://httpd.apache.org/docs/2.4/mod/mod_ssl.html>`_


.. _acme_tiny_ref_dovecot_cfg:

Dovecot
~~~~~~~

.. code-block:: text

    ssl_cert = </etc/dovecot/ssl/example.com.crt
    ssl_key  = </etc/dovecot/ssl/example.com.key

- Upstream documentation:
  `Dovecot Wiki: SSL <http://wiki.dovecot.org/SSL>`_


.. _acme_tiny_ref_lighttpd:

Lighttpd
~~~~~~~~

.. code-block:: lighty

    ssl.pemfile /etc/lighttpd/ssl/example.com.pem
    ssl.cafile  /etc/lighttpd/ssl/ca.crt

- Upstream documentation:
  `Lighttpd Wiki: Secure HTTP <http://redmine.lighttpd.net/projects/lighttpd/wiki/Docs_SSL>`_


.. _acme_tiny_ref_nginx:

Nginx
~~~~~

.. code-block:: nginx

    ssl_certificate     /etc/nginx/ssl/example.com.crt
    ssl_certificate_key /etc/nginx/ssl/example.com.key

- Upstream documentation:
  `Module ngx_http_ssl_module <http://nginx.org/en/docs/http/ngx_http_ssl_module.html>`_


.. _acme_tiny_ref_postfix:

Postfix
~~~~~~~

.. code-block:: text

    smtpd_tls_cert_file = /etc/nginx/ssl/example.com.crt
    smtpd_tls_key_file  = /etc/nginx/ssl/example.com.key

- Upstream documentation:
  `Postfix TLS Support <http://www.postfix.org/TLS_README.html>`_


.. _acme_tiny_ref_cert_renewal:

Certificate renewal
-------------------

After adding a new domain the role has to be run once with ``root``
privileges. Among other things this will create a separate user account
``acmetiny`` which can be used to schedule unattended certificate renewals.

.. note:: See :ref:`acme_tiny_ref_example_inventory` for an example how to
          create a role configuration.

Here an example of a :program:`cron` job (:file:`/etc/cron.d/acme-tiny`)
which whould renew the certificate every month:

.. code-block:: shell

    PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

    @monthly acmetiny /usr/bin/ansible-playbook -e @/etc/ansible/vars/mydomain.com.yml /etc/ansible/playbooks/acme_tiny.yml >/dev/null

..
 Local Variables:
 mode: rst
 ispell-local-dictionary: "american"
 End:
