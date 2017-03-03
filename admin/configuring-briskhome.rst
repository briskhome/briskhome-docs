Configuring |BRISKHOME|
=======================
.. include:: /.shared/work-in-progress.rst

|BRISKHOME| uses ``ini`` files for configuration (just like most unix packages). Main configuraion file is located in the ``etc`` subdirectory of the installation directory. Go ahead and open it with your favourite editor.

If you are unfamiliar with the structure and synthax used in the configuration file, please consult :ref:`the core.config section of the Developer Guide <core.config>`.

.. literalinclude:: /.static/example.conf
  :language: ini
  :linenos:
  :lines: 1-6

Skip to the **[core.db]** section and replace the placeholders with actual values of `username` and `password` for your **MongoDB** installation. If your database is deployed on another machine you also need to change the `host` field value from ``localhost`` to IP-address (or DNS name) of that machine.

.. literalinclude:: /.static/example.conf
  :language: ini
  :lineno-start: 8
  :lines: 8-12

Next, fill in the information about your **OpenLDAP** server in the **[core.ldap]** section.

.. literalinclude:: /.static/example.conf
  :language: ini
  :lineno-start: 15
  :lines: 15-18

If on the previous step you have chosen to install **Freeradius** then locate the **[core.pki]** section and list all Intermediate Certificate Authorities that you created in the corresponding section.

.. warning:: **Do not include your local Root Certificate Authority private key in the configuration file!**

  | Instead, set up a couple of intermediate certificate authorities. One would sign data and create certificates for services and devices, and the other would manage user certificates.
  | Keep your local Root Certificate Authority private key as safe as possible – preferrably, store it on a thumb drive in a safe that is in turn buried on the ocean floor. In the best-case scenario you're gonna need it twice only — to sign certificates of your Intermediate CAs and to renew them, which should be many years from now.

.. literalinclude:: /.static/example.conf
  :language: ini
  :lineno-start: 21
  :lines: 21-26

.. attention:: Make sure to set proper (restrictive) permissions on the ``certs`` and ``private`` directories!

Congratulations! The setup is now complete.
