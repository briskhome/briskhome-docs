Introduction
============

  *This document provides an overview of system requirements and steps required in order to install and configure the application and is intended for system administrators or advanced users wishing to try Briskhome in action.*

.. include:: /.shared/work-in-progress.rst

|BRISKHOME| is not a typical **Node.js** application, meaning that it can't be installed and run by simply typing `npm i -g briskhome` in console. Though it will download, it will not launch, because in order to function it requires host machine to be properly set up and needs several unix packages to be installed on it and/or be available on the local area network.

This document is a result of many months spent trying to set up everything properly integrated with each other. So, it is composed in a special way — it contains all of the instructions required to set up a fully working instance of |BRISKHOME|, even the ones for optional unix packages. All instructions are kept up-to-date as of corresponding release date.

.. include:: /.shared/contributing.rst
