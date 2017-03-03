.. sidebar:: Notice

  This documentation is a work in progress. Sections marked with a |wrench| are not completed yet.

  If you have any questions or suggestins feel free to open an issue and we will be more than happy to help you.

  * `Briskhome issue tracker`_
      *(questions about the application)*
  * `Documentation issue tracker`_
      *(questions about documentation)*

|BRISKHOME|
===========

|BRISKHOME| is a work-in-progress **house automation and monitoring system** for Node.js.

What can it do?
---------------
Currently |BRISKHOME| is on a stage of developing core functionality. It can't do much by itself as of right now, but after `v0.3.0` release it would be able to do the following:

* It manages your guests and shares your Wi-Fi connection with them.
* It provides a great interface for all yout IoT devices.
* It tracks your sensor readings and pushes notifications to you. *— planned in v.0.3.0!*
* It helps you control all your smart devices via a single web interface. *— planned in v.0.4.0!*

Documentation
-------------
Documentation is divided in three documents intended for different auditories:

`Administrator Guide <admin>`_
  This document provides an overview of steps required to install |BRISKHOME| and teaches how to maintain the system. Intended for system administrators or users wishing to install the system.

:html:`<i class="fa fa-wrench"></i>` `Developer Guide <developer>`_
  This document provides descriptions of system's REST API and APIs of core components. Intended for JavaScript developers wishing to create custom components for |BRISKHOME|.

:html:`<i class="fa fa-wrench"></i>` `User Manual <manual>`_
  This document provides an overview of the system web interface, ways of interacting with the system. Intended for aiding end users in their first catious steps with |BRISKHOME|.

Please note that the last two documents are in a work-in-progress state and will be completed some time around `1.0` release.

Development Status
------------------
The development of |BRISKHOME| is ongoing. System development progress, features, enhancements and bugfixes are tracked in a `Briskhome issue tracker`_. Issues connected to the documentation are tracked in a `Documentation issue tracker`_.

.. -------------------------------------------------------------------------------------------------

.. toctree::
  :caption: Administrator Guide
  :hidden:
  :maxdepth: 2
  :numbered: 2

  /admin/index
  /admin/system-requirements
  /admin/installing-dependencies
  /admin/configuring-dependencies
  /admin/installing-briskhome
  /admin/configuring-briskhome
  /admin/maintenance-and-support

.. toctree::
  :caption: Developer Guide
  :hidden:
  :maxdepth: 2
  :numbered: 2

  /developer/index
  /developer/key-concepts
  /developer/components
  /developer/api-reference

.. toctree::
  :caption: User Manual
  :hidden:
  :maxdepth: 2
  :numbered: 2

  /manual/index
  /manual/managing-users
  /manual/managing-devices
  /manual/managing-services

.. toctree::
  :caption: Reference
  :hidden:
  :maxdepth: 2

  Briskhome on GitHub <https://github.com/heuels/briskhome>
