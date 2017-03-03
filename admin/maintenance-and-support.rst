Maintainance & Support
======================
.. include:: /.shared/work-in-progress.rst

Logs
----
.. include:: /.shared/pre-release.rst

By default |BRISKHOME| stores logs in **MongoDB** only. You can override that setting by uncommenting ``file`` option in the `core.log` section of the main configuration file:

.. code-block:: ini
  :emphasize-lines: 11

  [core.log]
  # Minimum level of messages that will appear in main logfile.
  # 0 - Error - critical errors that mostly require intervention
  # 1 - Warn  - errors that do not affect normal workflow
  # 2 - Info  - informational messages about system processes
  # 3 - Debug - detailed information about important processes
  # 4 - Trace - detailed information about everything
  level = info

  # Name of the main Briskhome log file.
  file = briskhome.log


After setting this option |BRISKHOME| will write log entries both to **MongoDB** and to a file ``$BRISKHOME_DIR/log/briskhome.log`` (or other file you specified).

Crash Dumps
-----------
.. include:: /.shared/pre-release.rst

While |BRISKHOME| does store logs in the database, not all of the data after a crash may get logged. This is why after a crash |BRISKHOME| will generate a crash report in the ``log`` subdirectory of the |BRISKHOME| installation path.

If you believe that your |BRISKHOME| instance crashed because of a bug you can send us your core dump for inspection or you can create an issue on our `briskhome issue tracker`_ yourself. Please see `Contributing </reference/contributing.html>`_ section to learn what best to include in your issue to help us identify the cause.

Core Dumps
----------
If your |BRISKHOME| installation acts weird and you want to share the symptoms with us then you might want to make a core dump (or a couple of them) of a running process.
