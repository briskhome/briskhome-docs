Installing Dependencies
=======================
.. include:: /.shared/work-in-progress.rst

|BRISKHOME|, being a **Node.js** application, requires a set of software packages to be installed on a host system *and* sertain services to be available on a local network.

Node.js
*******

.. sidebar:: Information

  Supported Versions
    >=5.0.0
  Recommended Version
    6.9.5
  Homepage
    https://nodejs.org

This is the package that absolutely **must** be installed on a system that will run |BRISKHOME|. To install it we'll first need to get some software packages that will allow us to build source packages. The ``nvm`` command will leverage these tools to build the necessary components:

.. code-block:: console

  $ sudo apt-get update
  $ sudo apt-get install build-essential libssl-dev

Once the prerequisite packages are installed, you can pull down the ``nvm`` installation script from the `project's GitHub page <https://github.com/creationix/nvm>`_. The version number may be different, but in general, you can download it with either ``curl`` or ``wget``:

.. code-block:: console

  $ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash

.. code-block:: console

  $ wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash

This command will install the software into a subdirectory of your home directory at ``~/.nvm``. It will also add the necessary lines to your ``~/.profile`` file to make the nvm command available.

To gain access to the ``nvm`` command and its functionality, you'll need to log out and log back in again, or you can source the ``~/.profile`` file so that your current session knows about the changes:

.. code-block:: console

  $ source ~/.profile

Now that you have ``nvm`` installed, you can install isolated Node.js versions. To find out the versions of Node.js that are available for installation, you can type:

.. code-block:: console

  $ nvm ls-remote

The newest version at the time of this writing is **v7.5.0**, but **v6.9.5** is the latest long-term support release. While |BRISKHOME| supports any Node.js version greater than 5.0.0, it is generally better to use a latest LTS release. You can install that by typing:

.. code-block:: console

  $ nvm install v6.9.5

Usually, ``nvm`` will switch to use the most recently installed version. You can explicitly tell ``nvm`` to use the version we just downloaded by typing:

.. code-block:: console

  $ nvm use v6.9.5

You can see the version currently being used by the shell by typing:

.. code-block:: console

  $ node -v

If you have multiple Node.js versions, you can see which ones are installed by typing:

.. code-block:: console

  $ nvm ls

If you wish to make one of the versions the default, you can type:

.. code-block:: console

  $ nvm alias default v6.9.5

This version will be automatically selected when you open a new terminal session. You can also reference it by the alias like this:

.. code-block:: console

  $ nvm use default


MongoDB
*******
**MongoDB** is a required package, but it should not neccessarily be installed on the same system that will run |BRISKHOME| — for this you can use other machines on your network or even remote installations.

To install it import the public key used by the package management system.

The Debian package management tools (i.e. ``dpkg`` and ``apt``) ensure package consistency and authenticity by requiring that distributors sign packages with GPG keys. Issue the following command to import the MongoDB public GPG Key:

.. code-block:: console

  $ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6

Create a ``/etc/apt/sources.list.d/mongodb-org-3.4.list`` file for MongoDB.

.. code-block:: console

  $ echo "deb http://repo.mongodb.org/apt/debian jessie/mongodb-org/3.4 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

Reload local package database. Issue the following command to reload the local package database:

.. code-block:: console

  $ sudo apt-get update

Install the latest stable version of MongoDB. Issue the following command:

.. code-block:: console

  $ sudo apt-get install -y mongodb-org

OpenLDAP
********
**OpenLDAP** helps |BRISKHOME| manage users and permissions. The integration with OpenLDAP was included in |BRISKHOME| in version ``0.3.0-alpha``. Same as with **MongoDB**, the installation of this package is not required to be local — you may use other machines or remote installations.

The **OpenLDAP** server is in Debian's default repositories under the package ``slapd``, so we can install it easily with ``apt-get``. We will also install some additional utilities:

.. code-block:: console

  $ sudo apt-get install slapd ldap-utils

You will be asked to enter and confirm an administrator password for the administrator LDAP account. When the installation is complete, we actually need to reconfigure the LDAP package. Type the following to bring up the package configuration tool:

.. code-block:: console

  $ sudo dpkg-reconfigure slapd

You will be asked a series of questions about how you'd like to configure the software.

Omit OpenLDAP server configuration?
  No

DNS domain name?
  This will create the base structure of your directory path. Read the message to understand how it works. The default domain name for a typical |BRISKHOME| installation should be ``briskhome.local``

Organization name?
  BRISKHOME

Administrator password?
  Use the password you configured during installation, or choose another one

Database backend to use?
  HDB

Remove the database when slapd is purged?
  No

Move old database?
  Yes

Allow LDAPv2 protocol?
  No




OpenSSL
*******
The OpenSSL package is in Debian's default repositories, so we can install it easily with ``apt-get``.

.. code-block:: console

  $ sudo apt-get install openssl


Optional dependencies
*********************
Some software packages, while not required explicitly by |BRISKHOME|, provide functionality that may be useful. These package may be installed on a different machine and do not connect or integrate with |BRISKHOME| directly.

Each of the software packages listed below has been provided with a description of its effect on an overall |BRISKHOME| experience. Consider it carefully and decide whether to install them or not yourself.

DNS Server
~~~~~~~~~~
**Description**

Putting a **DNS server** on a network allows for the replacement of IP addresses of individual machines by a name. As a result, it's possible to access |BRISKHOME| web insterface by typing ``www.briskhome.local`` in the address bar instead of a server IP address. It also allows associating multiple names to the same machine to update the different available services. For example, ``www.briskhome.local`` and ``ldap.briskhome.local``, could both point to the primary server where the LDAP server and the business intranet reside, and the domain could be ``briskhome.local``. It's easy to remember that these two services are running on the same machine whose IP address is ``10.29.0.10``.

Now imagine that for some reason or another it is required to move the mail server to the machine ``10.29.0.11``. The only thing that has to be changed is the DNS server configuration file. You could always go and modify the host configuration for all the users, but that would be time consuming and inconvenient.

**Installation**

The package ``bind9`` will be used for installation.

.. code-block:: console

  $ sudo apt-get install bind

**Configuration**

Freeradius
~~~~~~~~~~
**Description**

The FreeRADIUS Server is a daemon for unix and unix like operating systems which allows one to set up a radius protocol server, which can be used for Authentication and Accounting various types of network access. In |BRISKHOME| it's often used to secure wireless networks with WPA2 Enterprise and VPN clients.

**Installation**

**Configuration**
