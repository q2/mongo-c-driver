:man_page: mongoc_client_read_write_command_with_opts

mongoc_client_read_write_command_with_opts()
============================================

Synopsis
--------

.. code-block:: c

  bool
  mongoc_client_read_write_command_with_opts (
     mongoc_client_t *client,
     const char *db_name,
     const bson_t *command,
     const mongoc_read_prefs_t *read_prefs,
     const bson_t *opts,
     bson_t *reply,
     bson_error_t *error);

Execute a command on the server, applying logic for commands that both read and write, and taking the MongoDB server version into account. To send a raw command to the server without any of this logic, use :symbol:`mongoc_client_command_simple`.

Use this function for commands that both read and write, such as "mapReduce" with an output collection.

Read concern is applied from ``opts`` or else from ``client``. Collation is applied from ``opts`` (:ref:`see example for  <mongoc_client_read_command_with_opts_example>`). Read concern and collation both require MongoDB 3.2 or later, otherwise an error is returned. Read preferences are applied from ``read_prefs`` or else from ``client``. Write concern is applied from ``opts``, or else from ``client``. The write concern is omitted for MongoDB before 3.2.

To target a specific server, include an integer "serverId" field in ``opts`` with an id obtained first by calling :symbol:`mongoc_client_select_server`, then :symbol:`mongoc_server_description_id` on its return value.

``reply`` is always initialized, and must be freed with :symbol:`bson:bson_destroy()`.

Parameters
----------

* ``client``: A :symbol:`mongoc_client_t`.
* ``db_name``: The name of the database to run the command on.
* ``command``: A :symbol:`bson:bson_t` containing the command specification.
* ``read_prefs``: An optional :symbol:`mongoc_read_prefs_t`.
* ``opts``: A :symbol:`bson:bson_t` containing additional options.
* ``reply``: A location for the resulting document.
* ``error``: An optional location for a :symbol:`bson_error_t <errors>` or ``NULL``.

Errors
------

Errors are propagated via the ``error`` parameter.

Returns
-------

``true`` if successful; otherwise ``false`` and ``error`` is set.

A write concern timeout or write concern error is considered a failure.

Example
-------

See the example code for :symbol:`mongoc_client_read_command_with_opts`.

