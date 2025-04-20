
.. _mysql-source-connector-config:

Configuration Reference for Debezium MySQL Source Connector for |cp|
====================================================================

The Debezium MySQL Source connector can be configured using a variety of
configuration properties.

.. note::

   These are properties for the self-managed connector. If you are using
   |ccloud|, see :cloud:`MySQL CDC Source (Debezium) Connector
   for Confluent Cloud|connectors/cc-mysql-source-cdc-debezium.html`.

Required parameters
-------------------

``name``
  A unique name for the connector. Trying to register again with the same name
  will fail. This property is required by all |kconnect-long| connectors.

  * Type: string
  * Default: No default

``connector.class``
  The name of the Java class for the connector. Always specify
  ``io.debezium.connector.mysql.MySqlConnector`` for the MySQL connector.

  * Type: string
  * Default: No default

``tasks.max``
  The maximum number of tasks that should be created for this connector. The
  MySQL connector always uses a single task and therefore does not use this
  value, so the default is always acceptable.

  * Type: int
  * Default: 1


``database.protocol``
  The JDBC protocol used by the driver connection string for connecting to the database.

  * Type: string
  * Default: ``jdbc:mysql``

``database.jdbc.driver``
  The driver class name to use. This can be useful when using an alternative
  driver to the one packaged with the connector.

  * Type: string
  * Default: ``com.mysql.cj.jdbc.Driver``

``database.hostname``
  IP address or hostname of the MySQL database server.

  * Type: string
  * Default: No default

``database.port``
  Integer port number of the MySQL database server.

  * Type: int
  * Importance: low
  * Default: ``3306``

``database.user``
  Name of the MySQL user to use when connecting to the MySQL database server.

  * Type: string
  * Importance: high
  * Default: No default

``database.password``
  Password to use when connecting to the MySQL database server.

  * Type: password
  * Importance: high
  * Default: No default

``topic.prefix``
  Topic prefix that provides a namespace for the particular MySQL database
  server/cluster in which Debezium is capturing changes. The topic prefix should
  be unique across all other connectors, since it is used as a prefix for all
  Kafka topic names that receive events emitted by this connector. Only
  alphanumeric characters, hyphens, dots and underscores must be used in the
  database server logical name.

  .. warning::

     Do not change the value of this property. If you change the name value,
     after a restart, instead of continuing to emit events to the original
     topics, the connector emits subsequent events to topics whose names are
     based on the new value. The connector is also unable to recover its
     database schema history topic.

  * Type: string
  * Default: No default

``database.server.id``
  A numeric ID of this database client, which must be unique across all
  currently-running database processes in the MySQL cluster. This connector
  joins the MySQL database cluster as another server (with this unique ID) so it
  can read the binlog.

  * Type: int
  * Default: No default

``database.include.list``
  An optional comma-separated list of regular expressions that match database
  names to be monitored. Any database name not included in the include list will
  be excluded from monitoring. By default all databases will be monitored. May
  not be used with  ``database.exclude.list``.

  * Type: list of strings
  * Importance: low
  * Default: empty string

``database.exclude.list``
  An optional comma-separated list of regular expressions that match database
  names to be excluded from monitoring. Any database name not included in the
  exclude list will be monitored. May not be used with
  ``database.include.list``.

  * Type: list of strings
  * Importance: low
  * Default: empty string

``table.include.list``
  An optional, comma-separated list of regular expressions that match schema
  names for which you want to capture changes for tables to be monitored. Any
  schema not included in the include list will not have its changes captured.
  Each identifier is of the form ``databaseName.tableName``. By default, the
  connector will monitor every non-system table in each monitored schema. May
  not be used with ``table.exclude.list``.

  * Type: list of strings
  * Importance: low
  * Default: No default

``table.exclude.list``
  An optional comma-separated list of regular expressions that match
  fully-qualified table identifiers for tables to be excluded from monitoring.
  Any table not included in the blacklist will be monitored. Each identifier is
  of the form ``databaseName.tableName``. May not be used with
  ``table.include.list``.

  * Type: list of strings
  * Importance: low
  * Default: empty string

``column.exclude.list``
  An optional, comma-separated list of regular expressions that match the
  fully-qualified names of columns to exclude from change event record values.
  Fully-qualified names for columns are of the form
  ``databaseName.tableName.columnName``.

  * Type: list of strings
  * Importance: low
  * Default: empty string

``column.include.list``
  An optional, comma-separated list of regular expressions that match the
  fully-qualified names of columns to include in change event record values.
  Fully-qualified names for columns are of the form
  ``databaseName.tableName.columnName``.

  * Type: list of strings
  * Importance: low
  * Default: empty string

``skip.messages.without.change``
  Specifies whether to skip publishing messages when there is no change in
  included columns. This would essentially filter messages if there is no change
  in columns included as per ``column.include.list`` or ``column.exclude.list``
  properties.

  * Type: boolean
  * Default: false


``column.truncate.to.length.chars``
  An optional comma-separated list of regular expressions that match the
  fully-qualified names of character-based columns. The column values are
  truncated in the change event message values if the field values are longer
  than the specified number of characters. Multiple properties with different
  lengths can be used in a single configuration, although in each the length
  must be a positive integer. Fully-qualified names for columns are in the form
  ``databaseName.tableName.columnName``.

  * Type: list of strings
  * Importance: low
  * Default: No default

``column.mask.with.length.chars``
  An optional comma-separated list of regular expressions that match the
  fully-qualified names of character-based columns. The column values are
  replaced in the change event message values with a field value consisting of
  the specified number of asterisk (*) characters. Multiple properties with
  different lengths can be used in a single configuration, although in each the
  length must be a positive integer. Fully-qualified names for columns are in
  the form ``databaseName.tableName.columnName``.

  * Type: list of strings
  * Importance: low
  * Default: No default


``column.mask.hash.hashAlgorithm.with.salt.salt``; ``column.mask.hash.v2.hashAlgorithm.with.salt.salt``
  An optional, comma-separated list of regular expressions that match the
  fully-qualified names of character-based columns. Fully qualified names for a
  column are in the following form: ``<databaseName>.<tableName>.<columnName>``.
  For more details about these properties, see the `Debezium documentation
  <https://debezium.io/documentation/reference/2.3/connectors/mysql.html#mysql-property-column-mask-hash>`__.

  * Type: list of strings
  * Default: No default

``column.propagate.source.type``
  An optional comma-separated list of regular expressions that match the
  fully-qualified names of columns whose original type and length should be
  added as a parameter to the corresponding field schemas in the emitted change
  messages. The schema parameters ``__debezium.source.column.type``,
  ``__debezium.source.column.length`` and ``_debezium.source.column.scale`` are
  used to propagate the original type name and length (for variable-width
  types), respectively. Useful to properly size corresponding columns in sink
  databases. Fully-qualified names for columns are in the form
  ``databaseName.tableName.columnName``.

  * Type: list of strings
  * Importance: low
  * Default: No default

``database.propagate.source.type``
  An optional, comma-separated list of regular expressions that match the fully
  qualified names of columns whose original type and length should be added as a
  parameter to the corresponding field schemas in the emitted change event
  records. The following schema parameters are used to propagate the original
  type name and length for variable-width types, respectively:
  ``__debezium.source.column.type``, ``__debezium.source.column.length``, and
  ``__debezium.source.column.scale``.

  This is a useful to properly size corresponding columns in sink databases.
  Fully-qualified data type names are of one of the following forms:
  ``databaseName.tableName.typeName``. For a list of MySQL-specific data type
  names, see `Data type mappings
  <https://debezium.io/documentation/reference/connectors/mysql.html#mysql-data-types>`__
  in the Debezium documentation.

  * Type: list of strings
  * Importance: low
  * Default: No default

``time.precision.mode``
  Time, date, and timestamps can be represented with
  different kinds of precision.

  * Type: string
  * Importance: low
  * Default: ``adaptive_time_microseconds``

  Settings include the following:

  -  ``adaptive_time_microseconds``: (Default) which captures the date,
     datetime and timestamp values exactly as they are in the database. It uses
     either millisecond, microsecond, or nanosecond precision values that are are
     based on the database column’s type. An exception to this are TIME type
     fields, which are always captured as microseconds.

  -  ``adaptive``: (deprecated) Captures the time and timestamp values exactly as
     they are the database using either millisecond, microsecond, or nanosecond
     precision values. These values are based on the database column type.

  -  ``connect``: Represents time and timestamp values using |kconnect|'s
     built-in representations for Time, Date, and Timestamp. It uses millisecond
     precision regardless of database column precision.

``decimal.handling.mode``
  Specifies how the connector should handle values for ``DECIMAL`` and
  ``NUMERIC`` columns.

  * Type: string
  * Importance: low
  * Default: ``precise``

  Settings include the following:

  -  ``precise``: (the default) represents them precisely using
     ``java.math.BigDecimal`` values represented in change events in a binary
     form; or double represents them using double values, which may result in a
     loss of precision but will be far easier to use.
  -  ``double``: Represents them using ``double`` values, which may result in a
     loss of precisions but is easier to use.
  -  ``string``: encodes values as formatted string which is easy to consume but
     semantic information about the real type is lost.

``bigint.unsigned.handling.mode``
  Specifies how BIGINT UNSIGNED columns should be represented in change events.

  * Type: string
  * Importance: low
  * Default: ``long``

  Settings include the following:

  -  ``precise`` uses ``java.math.BigDecimal`` to represent values, which are
     encoded in the change events using a binary representation and
     |kconnect-long|’s ``org.apache.kafka.connect.data.Decimal`` type.

  -  ``long`` (the default) represents values using Java’s ``long``, which may
     not offer the precision but will be far easier to use in consumers. ``long``
     is usually the preferable setting. The ``precise`` setting should only be used
     when working with values larger than 2^63 (these values can not be conveyed
     using ``long``).


``include.schema.changes``
  Boolean value that specifies whether the connector should publish changes in
  the database schema to a |ak| topic with the same name as the database server
  ID. Each schema change will be recorded using a key that contains the database
  name and whose value includes the DDL statement(s). This is independent of how
  the connector internally records database history.

  * Type: boolean
  * Importance: low
  * Default: ``true``


``include.schema.comments``
  Boolean value that specifies whether the connector should parse and publish
  table and column comments on metadata objects. Enabling this option will bring
  the implications on memory usage. The number and size of logical schema
  objects is what largely impacts how much memory is consumed by the Debezium
  connectors, and adding potentially large string data to each of them can
  potentially be quite expensive.

  * Type: boolean
  * Default: ``false``

``include.query``
  Boolean value that specifies whether the connector should include the original
  SQL query that generated the change event. Note: This option requires MySQL be
  configured with the ``binlog_rows_query_log_events`` option set to ``ON``.
  Query will not be present for events generated from the snapshot process. Note
  that enabling this option may expose tables or fields explicitly excluded
  or masked by including the original SQL statement in the change event.

  * Type: boolean
  * Importance: low
  * Default: ``false``

``event.deserialization.failure.handling.mode``
  Specifies how the connector should react to exceptions during deserialization
  of binlog events. ``fail`` propagates the exception (indicating the
  problematic event and its binlog offset), causing the connector to stop.
  ``warn`` causes the problematic event to be skipped and the problematic event
  and its binlog offset to be logged (make sure that the logger is set to the
  ``WARN`` or ``ERROR`` level). ``ignore`` causes the problematic event to be
  skipped.

  * Type: string
  * Importance: low
  * Default: ``fail``

``inconsistent.schema.handling.mode``
  Specifies how the connector should react to binlog events that relate to tables not present in the internal schema representation (which is inconsistent with the database). ``fail`` throws an exception (indicating the problematic event and its binlog offset), causing the connector to stop. ``warn`` causes the problematic event to be skipped and the problematic event and its binlog offset to be logged (make sure that the logger is set to the ``WARN`` or ``ERROR`` level). ``ignore`` causes the problematic event to be skipped.

  * Type: string
  * Importance: low
  * Default: ``fail``

``connect.timeout.ms``
  A positive integer value that specifies the maximum time in milliseconds this
  connector should wait after trying to connect to the MySQL database server
  before timing out. Defaults to 30000 milliseconds (30 seconds).

  * Type: string
  * Importance: low
  * Default: 30000

``gtid.source.includes``
  A comma-separated list of regular expressions that match source UUIDs in the GTID set used to find the binlog position in the MySQL server. Only the GTID ranges that have sources matching one of these include patterns will be used. May not be used with ``gtid.source.excludes``.

  * Type: list of strings
  * Importance: low
  * No default

``gtid.source.excludes``
  A comma-separated list of regular expressions that match source UUIDs in the GTID set used to find the binlog position in the MySQL server. Only the GTID ranges that have sources matching none of these exclude patterns will be used. May not be used with ``gtid.source.includes``.

  * Type: list of strings
  * Importance: low
  * No default

``tombstones.on.delete``
  Controls whether a tombstone event should be generated after a delete event. When set to ``true``, the delete operations are represented by a delete event and a subsequent tombstone event. When set to ``false``, only a delete event is sent. Emitting the tombstone event (the default behavior) allows |ak| to completely delete all events pertaining to the given key once the source record got deleted.

  * Type: string
  * Importance: low
  * Default: true

``message.key.columns``
  A list of expressions that specify the columns that the connector uses to form
  custom message keys for change event records that it publishes to the |ak|
  topics for specified tables.

  By default, Debezium uses the primary key column of a table as the message key
  for records that it emits. In place of the default, or to specify a key for
  tables that lack a primary key, you can configure custom message keys based on
  one or more columns.

  To establish a custom message key for a table, list the table, followed by the
  columns to use as the message key. Each list entry takes the following format:

  .. code-block:: text

     <fully-qualified_tableName>:_<keyColumn>_,<keyColumn>

  To base a table key on multiple column names, insert commas between the column names.

  Each fully-qualified table name is a regular expression in the following format:

  .. code-block:: text

     <databaseName>.<tableName>

  The property can include entries for multiple tables. Use a semicolon to
  separate table entries in the list. The following example sets the message key
  for the tables ``inventory.customers`` and ``purchase.orders``:

  .. code-block:: text

     inventory.customers:pk1,pk2;(.*).purchase.orders:pk3,pk4

  For the table inventory.customer, the columns pk1 and pk2 are specified as the
  message key. For the ``purchase.orders`` tables in any database, the columns
  ``pk3`` and ``pk4`` server as the message key.

  There is no limit to the number of columns that you use to create custom message
  keys. However, it’s best to use the minimum number that are required to specify
  a unique key.

  * Type: list
  * Default: No default

``binary.handling.mode``
  Specifies how binary columns (for example, blob, binary, varbinary) should be
  represented in change events.

  * Type: bytes or string
  * Importance: low
  * Default: Bytes

``schema.name.adjustment.mode``
  Specifies how schema names should be adjusted for compatibility with the
  message converter used by the connector.
  * Type: string
  * Default: ``none``
  * Valid values:

    - ``none``: Applies no adjustment.
    - ``avro``: Replaces the characters that cannot be used in the Avro type
      name with an underscore,
    - ``avro_unicode``: Replaces the underscore or characters that cannot be
      used in the Avro type name with corresponding unicode like ``_uxxxx``.
      Note that ``_`` is an escape sequence like backslash in Java.

``field.name.adjustment.mode``
  Specifies how schema names should be adjusted for compatibility with the
  message converter used by the connector. The following are possible settings:

  - ``avro``: Replaces the characters that cannot be used in the Avro type name with an underscore
  - ``none``: Does not apply any adjustment
  - ``avro_unicode``: Replaces the underscore or characters that cannot be used
    in the Avro type name with corresponding unicode like ``_uxxxx``. Note that ``_`` is
    an escape sequence like backslash in Java.

Advanced parameters
-------------------

``connect.keep.alive``
  A Boolean value that specifies whether a separate thread should be used to
  ensure that the connection to the MySQL server/cluster is kept alive.

  * Type: boolean
  * Default: true

``converters``
  Enumerates a comma-separated list of the symbolic names of the custom
  converter instances that the connector can use. For more details about this
  property, see the `Debezium documentation
  <https://debezium.io/documentation/reference/2.4/connectors/mysql.html#mysql-property-converters>`__

  * Default: No default

``table.ignore.builtin``
  A Boolean value that specifies whether built-in system tables should be
  ignored. This applies regardless of the table include and exclude lists. By
  default, system tables are excluded from having their changes captured, and no
  events are generated when changes are made to any system tables.

  * Type: boolean
  * Default: true

``database.ssl.mode``
  Specifies whether to use an encrypted connection.

  * Type: string
  * Valid values:

    -  ``disabled``: Specifies the use of an unencrypted connection.
    -  ``preferred``: Establishes an encrypted connection if the server supports secure connections. If the server does not support secure connections, falls back to an unencrypted connection.
    -  ``required``: Establishes an encrypted connection or fails if one can't be made for any reason.
    -  ``verify_ca``: Behaves like required but additionally it verifies the server TLS certificate against the configured Certificate Authority (CA) certificates and fails if the server TLS certificate does not match any valid CA certificates.
    -  ``verify_identity``: Behaves like ``verify_ca`` but additionally verifies the server certificate matches the host of the remote connection.
  * Default: disabled

``binlog.buffer.size``
  The size of a look-ahead buffer used by the binlog reader. The default setting
  is 0,  which disables buffering.

  * Type: int
  * Default: 0

``max.queue.size``
  Positive integer value that specifies the maximum size of the blocking queue into which change events read from the database log are placed before they are written to |ak|. This queue can provide backpressure to the binlog reader when, for example, writes to |ak| are slower or if |ak| is not available. Events that appear in the queue are not included in the offsets periodically recorded by this connector. Defaults to ``8192``, and should always be larger than the maximum batch size specified in the ``max.batch.size`` property.

  * Type: int
  * Importance: low
  * Default: 8192

``max.batch.size``
  Positive integer value that specifies the maximum size of each batch of events that should be processed during each iteration of this connector. Defaults to ``2048``.

  * Type: int
  * Importance: low
  * Default: 2048

``max.queue.size.in.bytes``
  Long value for the maximum size in bytes of the blocking queue. The feature is
  disabled by default, it will be active if it’s set with a positive long value.

  * Type: long
  * Importance: low
  * Default: 0

``poll.interval.ms``
  Positive integer value that specifies the number of milliseconds the connector should wait during each iteration for new change events to appear. Defaults to ``500`` milliseconds.

  * Type: int
  * Importance: low
  * Default: 500

``snapshot.mode``
  Specifies the criteria for running a snapshot when the connector starts.

  * Type: string
  * Valid values:

    - ``initial``: Runs a snapshot only when no offsets have been recorded for the logical server name
    - ``initial_only``: Runs a snapshot only when no offsets have been recorded for the logical server name and then stop
    - ``when_needed``: Runs a snapshot only upon startup whenever the connector deems it necessary. Note that if the connector cannot find the binlog file mentioned in the offsets, it will take another snapshot and that may lead to duplicate data.
    - ``never``: Never uses snapshots
    - ``schema_only``: Runs a snapshot of schemas and not the data
    - ``schema_only_recovery``: For a connector that has already been capturing change, enables recovery of a corrupted or lost database history topic that has been growing unexpectedly

  * Default: initial

``snapshot.locking.mode``
  Controls whether and for how long the connector holds the global MySQL read
  lock, which prevents any updates to the database while the connector is
  performing a snapshot.

  * Type: string
  * Valid values:

    - ``minimal``: Hold the global read lock for only the initial portion of the snapshot during which the connector reads the database schemas and other metadata
    - ``minimal_percona``: Hold the global backup lock `<https://www.percona.com/doc/percona-server/5.7/management/backup_locks.html>`__ for only the initial portion of the snapshot during which the connector reads the database schemas and other metadata.
    - ``extended``: Block all writes for the duration of the snapshot. Use if there are clients that are submitting operations that MySQL excludes from REPEATABLE READ semantics
    - ``none``: Prevent the connector from acquiring any table locks during a snapshot. Best to use if and only if no schema changes are happening while the snapshot is running


  * Default: minimal

``snapshot.include.collection.list``
  An optional, comma-separated list of regular expression that match the
  fully-qualified names (``<databaseName>.<tableName>``) of the tables to
  include in a snapshot. The specified items must be named in the connector's
  ``table.include.list`` property.

  * Type: list
  * Default: All tables specified in ``table.include.list``

``snapshot.select.statement.overrides``
  Specifies the table rows to include in a snapshot.  Use this property if you
  want a snapshot to include only a subset of rows in a table. This property
  affects snapshots only. It doesn't apply to events that the connector reads
  from the log. This property contains a comma-separated list of fully-qualified
  table names in the form ``<databaseName>.<tableName>``. For an example and
  more details, see the `Debezium
  <https://debezium.io/documentation/reference/2.3/connectors/mysql.html#mysql-property-snapshot-select-statement-overrides>`__
  documentation.

  * Type: list
  * Default: No default

``min.row.count.to.stream.results``
  Specifies the minimum number of rows a table must contain before the connector
  streams results. To skip all table-size checks and always stream all results
  during a snapshot, set this property to 0.

  * Type: int
  * Default: 1000

``heartbeat.interval.ms``
  Controls how frequently the connector sends heartbeat messages to a |ak|
  topic. The default behavior is the connector does not send heartbeat messages.

  * Type: int
  * Default: 0

``heartbeat.action.query``
  Specifies a query that the connector executes on the source database when the
  connector sends a heartbeat message.

``database.initial.statements``
  A semicolon-separated list of SQL statements to be executed when a JDBC
  connection, not the connection that is reading the transaction log, to the
  database is established. To specify a semicolon as a character in a SQL
  statement and not as a delimiter, use two semicolons, ``;;``.

  * Type: list
  * Default: No default

``snapshot.delay.ms``
  An interval in milliseconds that the connector should wait before performing a
  snapshot when the connector starts. If starting several connectors in a
  cluster, this property is useful for avoiding snapshot interruptions, which
  might cause re-balancing of connectors.

  * Type: int
  * Default: No default

``snapshot.fetch.size``
  During a snapshot, the connector reads table content in batches of rows. This
  property specifies the maximum number of rows in a batch.

  * Type: int
  * Default: No default

``snapshot.lock.timeout.ms``
  Specifies the maximum amount of time (in milliseconds) to wait to obtain table
  locks when performing a snapshot. If the connector does not obtain table locks
  in the specified time interval, the snapshot fails. For more details, see `how
  MySQL connectors perform database snapshots
  <https://debezium.io/documentation/reference/2.0/connectors/mysql.html#mysql-snapshots>`__.

  * Type: int
  * Default: 10000

``enable.time.adjuster``
  Specify whether or not the connector converts a 2-digit year specification to
  four digits. Set this to ``false`` if you want the conversion to be fully
  delegated to the database.

  With MySQL, you can insert year values with either 2 or 4 digits. For
  2-digit values, the value gets mapped to a year in the range 1970 - 2069. The
  default behavior is that the connector does the conversion.

  * Type: boolean
  * Default: ``true``

``source.struct.version``
  Schema version for the ``source`` block in Debezium events. By setting this
  parameter to ``v1``, the structures used in earlier versions can be produced. This setting is not recommended.

  * Type: string
  * Default: ``v2``

``skipped.operations``
  A comma-separated list of operation types that will be skipped during
  streaming.  Possible values include: ``c`` for inserts/create, ``u`` for
  updates, ``d`` for deletes. By default, no operations are skipped.

  * Type: list
  * Default: No default

``signal.data.collection``
  Fully-qualified name of the data collection that is used to send signals to
  the connector. Use the following format to specify the collection name:
  ``<databaseName>.<tableName>``.

  * Type: string
  * Default: No default

``signal.enabled.channels``
  A list of the signaling channel names that are enabled for the connector. By
  default, the following channels are available: ``source``, ``kafka``,
  ``file``, and ``jmx`` (optionally, you can also implement a custom signal
  channel).

  * Type: list of strings
  * Default: ``source``


``notification.enabled.channels``
  A list of the notification channel names that are enabled for the connector.
  By default, the following channels are available: ``sink``, ``log``, and
  ``jmx`` (optionally, you can also implement a custom signal channel).

  * Type: string
  * Default: No default

``incremental.snapshot.allow.schema.changes``
  Set to ``true`` to allow schema changes during an incremental snapshot.

  * Type: string
  * Default: false

``incremental.snapshot.chunk.size``
  The maximum number of rows the connector fetches and reads into memory during
  an incremental snapshot chunk. Note that increasing the chunk size provides
  greater efficiency as the snapshot runs fewer snapshot queries of a greater
  size. On the other hand, a larger chunk size requires more memory to buffer the
  snapshot data. You should adjust the chunk size to a value that achieves
  optimal performance in your environment.

  * Type: int
  * Default: 1024

``read.only``
  Specifies whether to switch to alternative incremental snapshot watermarks
  implementation to avoid writes to signal data collection.

  * Type: boolean
  * Default: false

``provide.transaction.metadata``
  When set to ``true``, the connector generates events with transaction
  boundaries and enriches change event envelopes with transaction metadata. For
  more details, see `Transaction metadata
  <https://debezium.io/documentation/reference/1.9/connectors/mysql.html#mysql-transaction-metadata>`__.

  * Type: boolean
  * Default: false

``event.processing.failure.handling.mode``
  Specifies how failures should be handled during the processing of events–that is,
  when encountering a corrupted event.

  * Type: string
  * Valid values:

    - ``fail``: Raises an exception indicating the corrupted event and its
      position, causing the connector to be stopped.
    - ``warn``: Does not raise an exception, instead the corrupted event and its
      position will be logged and the event will be skipped.
    - ``ignore``: Ignores the corrupted event completely with no logging.
  * Default: fail

``topic.naming.strategy``
  The name of the TopicNamingStrategy class that should be used to determine the
  topic name for data change, schema change, transaction, heartbeat event, etc.

  * Type: string
  * Default: ``io.debezium.schema.DefaultTopicNamingStrategy``

``topic.delimiter``
  Specifies the delimiter for topic name.  Defaults to ``.``.

``topic.cache.size``
  The size used for holding the topic names in bounded concurrent hash map. This
  cache will help to determine the topic name corresponding to a given data
  collection.

  * Type: int
  * Default: 10000

``topic.heartbeat.prefix``
  Controls the name of the topic to which the connector sends heartbeat
  messages. The topic name has the following pattern:
  ``topic.heartbeat.prefix.topic.prefix``.

  * Type: string
  * Default: ``__debezium-heartbeart``


``topic.transaction``
  Controls the name of the topic to which the connector sends transaction
  metadata messages. The topic name has the following pattern:
  ``topic.prefix.topic.transaction``.

  * Type: string
  * Default: ``transaction``

``snapshot.max.threads``
  Specifies the number of threads the connector uses when performing an initial
  snapshot. You can enable parallel initial snapshots—the connector will
  process multiple tables at the same time—by setting this property to a value
  greater than 1.

  * Type: int
  * Default: 1

``snapshot.tables.order.by.row.count``
  Specifies the order in which the connector processes tables when it performs an
  initial snapshot.

  * Type: int
  * Valid values:

    - ``descending``: The connector snapshots tables in order, based on the number of rows from highest to lowest.
    - ``ascending``: The connector snapshots tables in order, based on the number of rows from lowest to highest.
    - ``disabled``: The connector disregards row count when performing an initial snapshot.
  * Default: ``disabled``


``custom.metric.tags``
  Accepts key-value pairs to customize the MBean object name. For more details
  about this property, see the `Debezium documentation
  <https://debezium.io/documentation/reference/2.4/connectors/mysql.html#mysql-property-custom-metric-tags>`__.

  * Default: No default

``errors.max.retries``
  The maximum number of retries on retriable errors (for example, connection
  errors) before failing.

  * Type: int
  * Default: -1

Database schema history parameters
----------------------------------

``schema.history.internal.kafka.topic``
  The full name of the |ak| topic where the connector will store the database
  schema history.

  .. important::

     Because Debezium uses multiple topics–of which certain limitations may
     apply–for storing data, Confluent recommends you view `Configuring Debezium
     Topics
     <https://debezium.io/documentation/reference/stable/install.html#configuring-debezium-topics>`__
     before you create a database schema history topic.

  * Type: string
  * Importance: high
  * Default: No default

``schema.history.internal.kafka.bootstrap.servers``
  A list of host/port pairs that the connector will use for establishing an
  initial connection to the |ak| cluster. This connection will be used for
  retrieving database schema history previously stored by the connector, and for
  writing each DDL statement read from the source database. This should point to
  the same |ak| cluster used by the |kconnect-long| process.

  * Type: list of strings
  * Importance: high
  * Default: No Default

``schema.history.internal.kafka.recovery.poll.interval.ms``
  The maximum time duration, in milliseconds, the connector waits during startup or recovery while polling for persisted data.

  * Type: int
  * Default: 100

``schema.history.internal.kafka.query.timeout.ms``
  The maximum time duration, in milliseconds, the connector waits for cluster information to be fetched using the |ak| admin client.

  * Type: int
  * Default: 3000

``schema.history.internal.kafka.create.timeout.ms``
  An integer value that specifies the maximum number of milliseconds the
  connector should wait while creating the |ak| history topic using the |ak|
  admin client.

  * Type: int
  * Default: 30000

``schema.history.internal.kafka.recovery.attempts``
  The maximum number of times that the connector should try to read persisted
  history data before the connector recovery fails with an error. The maximum
  amount of time to wait after receiving no data is ``recovery.attempts x
  recovery.poll.interval.ms``.

  * Type: int
  * Default: 100

``schema.history.internal.skip.unparseable.ddl``
  A Boolean value that specifies whether the connector should ignore malformed
  or unknown database statements or stop processing so a human can fix the
  issue. The safe default is ``false``. Skipping should be used only with care
  as it can lead to data loss or mangling when the binlog is being processed.

  * Type: boolean
  * Default: false

``schema.history.internal.store.only.captured.tables.ddl``
  A Boolean value that specifies whether the connector records schema structures
  from all tables in a schema or database, or only from tables that are
  designated for capture.

  * Type: boolean
  * Default: false


``schema.history.internal.store.only.captured.databases.ddl``
  A Boolean value that specifies whether the connector should record schema
  structures from all logical databases instance. When set to ``true`` the
  connector records schema structures only for tables in the logical database
  and schema from which Debezium captures change events. When set to ``false``
  the connector records schema structures for all logical databases.

  * Type: boolean
  * Default: false

Signal Parameters
-----------------

``signal.kafka.topic``
  The name of the |ak| topic that connector monitors for ad-hoc signals.

  * Type: string
  * Default: No default

``signal.kafka.groupId``
  The name of the group ID that is used by |ak| consumers.

  * Type: string
  * Default: ``kafka-signal``

``signal.kafka.bootstrap.servers``
  A list of host/port pairs that the connector uses for establishing an initial
  connection to the |ak| cluster. Each pair should point to the same |ak|
  cluster used by the |kconnect-long| process.

  * Type: list
  * Default: No default

``signal.kafka.poll.timeout.ms``
  An integer value that specifies the maximum number of milliseconds the
  connector should wait when polling signals. The default is 100 ms.

  * Type: int
  * Default: 100

Auto topic creation
-------------------

For more information about Auto topic creation, see :connect-common:`Configuring
Auto Topic Creation for Source
Connectors|userguide.html#connect-source-auto-topic-creation`.

.. include:: .hidden/docs-common/kafka-connectors/self-managed/includes/auto-topic-connector-configs.rst

You can find more advanced configuration properties and details in the `Debezium
connector for MySQL
<https://debezium.io/docs/connectors/mysql/#connector-properties>`__
documentation.

.. note::

   Portions of the information provided here derives from documentation
   originally produced by the `Debezium Community <https://debezium.io/>`__. Work
   produced by Debezium is licensed under `Creative Commons 3.0
   <https://creativecommons.org/licenses/by/3.0/>`__.

CSFLE configuration
--------------------

.. include:: .hidden/docs-common/kafka-connectors/self-managed/includes/csfle_configurations_sm.rst