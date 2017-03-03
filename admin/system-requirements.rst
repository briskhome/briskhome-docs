System Requirements
===================
.. include:: /.shared/work-in-progress.rst

Application requirements
------------------------
|BRISKHOME| is mostly hardware-agnostic, meaning that generally it can be installed and run on almost any hardware. During `alpha stage` it has even been reported to launch on a **Raspberry Pi**. In short, your machine should be able to run **Node.js** and **OpenSSL**. If it does then you're good to go.

Currently our production server that runs |BRISKHOME| 24/7 has the following specifications:

CPU
  ...
RAM
  ...
Graphics Card
  ...
Network Card
  ...
Motherboard
  ...

Please `send us <mailto:hello@briskhome.com>`_ your configurations that were or were not able to successfully run |BRISKHOME|, it will help us greatly in figuring out the exact minimum system requirements for the application.

Dependency requirements
-----------------------
Another point to take into account when figuring out the minimum system specifications of a machine that will run |BRISKHOME| is whether or not required packages and/or services will be installed on the same machine.

Apart from **Node.js** and **OpenSSL** there are two more required services: **MongoDB** and **OpenLDAP**. These can be installed on a local machine or on another machine on the same network. Please refer to the corresponding documentations of these packages to learn their system requirements.

There are also two optional dependencies that help forming a unified |BRISKHOME| experience but do not integrate directly with |BRISKHOME| — **bind9** (a DNS server package) and **Freeradius** (a radius protocol server package). If you decide to install them alongside |BRISKHOME| please take them into account when figuring out the system requirements.
