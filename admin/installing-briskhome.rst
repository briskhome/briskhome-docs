Installing Briskhome
====================

.. include:: /.shared/work-in-progress.rst

Automatic installation
**********************
|BRISKHOME| is currently not published on **npm**. You can install it via the command line with either ``curl`` or ``wget``.

via ``curl``
------------


.. code-block:: console

  $ su -c "curl -o /tmp/install.sh https://.../install.sh && chmod +x /tmp/install.sh && /tmp/install.sh"

via ``wget``
------------

.. code-block:: console

  $ su -c "wget -O /tmp/install.sh https://.../install.sh && chmod +x /tmp/install.sh && /tmp/install.sh"

Manual installation
*******************

.. note:: This section is a stub. You can help us by describing the installation script step-by-step.

If you would like to manually install the application, please follow the instructions in the installation script.


Configuration
*************
|BRISKHOME| uses ``ini`` files for configuration (just like most unix packages). Main configuraion file is located in the ``etc`` subdirectory of the installation directory. Go ahead and open it with your favourite editor.

If you are unfamiliar with the structure and synthax used in the configuration file, please consult :ref:`core.config section of the Developer Guide <core.config>`.

.. literalinclude:: /.static/example.conf
  :language: ini
  :linenos:
  :lines: 1-6

Skip to the **[core.db]** section and replace the placeholders with actual values of `username` and `password` for your **MongoDB** installation. If your database is deployed on another machine you also need to change the `host` field value from ``localhost`` to IP-address (or DNS name) of that machine.

.. literalinclude:: /.static/example.conf
  :language: ini
  :linenos:
  :lines: 8-12

Next, fill in the information about you **OpenLDAP** server in the **[core.ldap]** section.

.. literalinclude:: /.static/example.conf
  :language: ini
  :linenos:
  :lines: 15-18

If on the previous step you have installed **Freeradius** then locate the **[core.pki]** section and list all Intermediate Certificate Authorities that you created in the corresponding section. Note that ``buca`` here is the identifier of the Certificate Authority that will be used throughout the system. Option `kind` may be either ``usersign``, if the CA is used for signing user certificates, or ``datasign`` if the CA is used for signing data payloads. Also note that there may be only one CA of each type. Root CA should not be mentioned in the configuration file.

.. literalinclude:: /.static/example.conf
  :language: ini
  :linenos:
  :lines: 21-26

.. attention:: Make sure to set proper (restrictive) permissions on the ``certs`` and ``private`` directories!

Congratulations! The setup is now complete.

Running Briskhome
*****************

.. note:: This section is a stub.

.. code-block:: console

  $ service briskhome start
