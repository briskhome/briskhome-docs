Components
==========

.. include:: /.shared/work-in-progress.rst

|BRISKHOME| is a modular application. It means that the application is composed of several modules that provide separate pieces of functionality. Modules that are created with support of |BRISKHOME| in mind are called **Briskhome components**, or **components** for short.

Core components
***************
|BRISKHOME| comes with **12 core components** preinstalled. Versions of these components are hard linked to the version of |BRISKHOME| itself, e.g. if |BRISKHOME| is bumped to a next version, all core components are bumped along with it.

core.api
--------
*For REST API Reference see Section 4 of Developer Guide.*

---------------------------------------------------------------------------------------------------

.. _core.bus:

core.bus+core.mqtt
------------------
**core.bus** component provides a common bus interface for core and custom components intercommunication and an interface to publish MQTT messages and subscribe to MQTT topics.

API
~~~
Bus.emit(name, data)
  Sends a message over the bus interface.

  * **name** *(String)* – Message identifier;
  * **data** *(Object)* – Data payload.

Specification
~~~~~~~~~~~~~
Starting from version **0.1.4** message format is defined in accordance to this specification.
Using any message format that differs from this specification is discouraged.

Message header
^^^^^^^^^^^^^^
Message header consists of `message namespace` and `message title`, delimited dy a colon, like this: ``namespace:title``.

* **namespace** *(String)* – If sender knows the name of the receiver, then it is used as a namespace, otherwise namespace is ``broadcast``.
* **title** *(String)* – Defined by sender's specification for a broadcast message, otherwise defined by reciever's specification.

Message body
^^^^^^^^^^^^

.. attention::

  Version 0.3.0 introduced the following breaking changes to this component:
    - In message body key ``module`` was renamed to ``component``.

  Please make sure to update your custom components to support the new specifications.

Message body or `payload` is transmitted in a form of JavaScript object. On the top level this object must have the information about the sender and the data object with the information.

* **component** *(String)* - Sender name.
* **data** *(Object)* - Data object.

Example usage
^^^^^^^^^^^^^

.. code-block:: javascript

  const bus = imports.bus;
  bus.emit('broadcast:mqtt-message-received', {
    component: "core.mqtt",
    data: {
      topic: '/irrigation/circuits/garden',
      payload: '{"status": 0}',
    },
  });

Version History
~~~~~~~~~~~~~~~
v0.3.1
  **Proposed change**: merging core.bus and core.mqtt components.
v0.3.0
  Component specification changed: in message body key ``module`` renamed to ``component``.
v0.1.4
  Component specification released.
v0.1.2
  Component added.

---------------------------------------------------------------------------------------------------

.. _core.config:

core.config
-----------
...

---------------------------------------------------------------------------------------------------

core.db
-------
...

---------------------------------------------------------------------------------------------------

core.ldap
---------
...

---------------------------------------------------------------------------------------------------

core.loader
-----------
...

---------------------------------------------------------------------------------------------------

core.log
--------
...

---------------------------------------------------------------------------------------------------

core.notifications
------------------
...

---------------------------------------------------------------------------------------------------

core.pki
--------
...

---------------------------------------------------------------------------------------------------

core.planner
------------
...

---------------------------------------------------------------------------------------------------

core.users
----------
...

---------------------------------------------------------------------------------------------------

core.utils
----------
...

Custom components
*****************
Components that are not included in the default distribution of |BRISKHOME| are called **custom**. Some of the examples are **briskhome-irrigation**, **briskhome-ups** and **briskhome-sysinfo**.

Naming convention
-----------------
As you can see, the naming scheme is simple: put ``briskhome-`` prefix and you're good to go.

package.json
------------
Inside the ``package.json`` file you also need to specify a peer dependency of |BRISKHOME|:

.. literalinclude:: /.static/package.json
  :lines: 2-4
