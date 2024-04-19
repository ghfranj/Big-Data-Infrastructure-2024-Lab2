# PostgreSQL with extensions

## Debug

```shell
BUILDKIT_PROGRESS=plain \
docker build -t qpod/postgres-16-ext -f ./postgres-ext.Dockerfile --build-arg BASE_NAMESPACE=qpod .

( docker stop db-postgres && docker rm db-postgres || true )
docker run -d \
    --name db-postgres \
    -p 5432:5432 \
    -e POSTGRES_PASSWORD=postgres \
    qpod/postgres-16-ext

docker exec -it db-postgres bash

ls -alh /usr/share/postgresql/${PG_MAJOR}/extension/*.control
```

## Reference

- article: https://mp.weixin.qq.com/s/CduvvvuUDjqNtvKA1OblAQ
- code: https://github.com/digoal/postgresql_docker_builder/blob/main/pg14_amd64/1.sh

## List of Extensions

```sql
SELECT extname AS name, extversion AS ver FROM pg_extension ORDER BY extname;

SELECT name, default_version AS ver, comment FROM pg_available_extensions ORDER BY name;

SELECT name, default_version AS ver FROM pg_available_extensions
WHERE name NOT IN (SELECT extname AS name FROM pg_extension) ORDER BY name;
```

| name                             | installed | avaliable  | comment                                                                                                               |
|----------------------------------|-----------|------------|-----------------------------------------------------------------------------------------------------------------------|
|  address_standardizer            |  3.4.2    |  3.4.2     |  Used to parse an address into constituent elements. Generally used to support geocoding address normalization step.  |
|  address_standardizer-3          |           |  3.4.2     |  Used to parse an address into constituent elements. Generally used to support geocoding address normalization step.  |
|  address_standardizer_data_us    |  3.4.2    |  3.4.2     |  Address Standardizer US dataset example                                                                              |
|  address_standardizer_data_us-3  |  3.4.2    |            |  Address Standardizer US dataset example                                                                              |
|  adminpack                       |  2.1      |  2.1       |  administrative functions for PostgreSQL                                                                              |
|  age                             |  1.5.0    |  1.5.0     |  AGE database extension                                                                                               |
|  amcheck                         |  1.3      |  1.3       |  functions for verifying relation integrity                                                                           |
|  autoinc                         |  1.0      |  1.0       |  functions for autoincrementing fields                                                                                |
|  bloom                           |  1.0      |  1.0       |  bloom access method - signature file based index                                                                     |
|  btree_gin                       |  1.3      |  1.3       |  support for indexing common datatypes in GIN                                                                         |
|  btree_gist                      |  1.7      |  1.7       |  support for indexing common datatypes in GiST                                                                        |
|  citext                          |  1.6      |  1.6       |  data type for case-insensitive character strings                                                                     |
|  citus                           |  12.1-1   |  12.1-1    |  Citus distributed database                                                                                           |
|  citus_columnar                  |  11.3-1   |  11.3-1    |  Citus Columnar extension                                                                                             |
|  credcheck                       |  2.6.0    |  2.6.0     |  credcheck - postgresql plain text credential checker                                                                 |
|  cube                            |  1.5      |  1.5       |  data type for multidimensional cubes                                                                                 |
|  dblink                          |  1.2      |  1.2       |  connect to other PostgreSQL databases from within a database                                                         |
|  decoderbufs                     |           |  0.1.0     |  Logical decoding plugin that delivers WAL stream changes using a Protocol Buffer format                              |
|  dict_int                        |  1.0      |  1.0       |  text search dictionary template for integers                                                                         |
|  dict_xsyn                       |  1.0      |  1.0       |  text search dictionary template for extended synonym processing                                                      |
|  earthdistance                   |  1.1      |  1.1       |  calculate great-circle distances on the surface of the Earth                                                         |
|  extra_window_functions          |  1.0      |  1.0       |                                                                                                                       |
|  file_fdw                        |  1.0      |  1.0       |  foreign-data wrapper for flat file access                                                                            |
|  first_last_agg                  |  0.1.4    |  0.1.4     |  first() and last() aggregate functions                                                                               |
|  fuzzystrmatch                   |  1.2      |  1.2       |  determine similarities and distance between strings                                                                  |
|  hll                             |  2.18     |  2.18      |  type for storing hyperloglog data                                                                                    |
|  hstore                          |  1.8      |  1.8       |  data type for storing sets of (key, value) pairs                                                                     |
|  hstore_pllua                    |           |  1.0       |  Hstore transform for Lua                                                                                             |
|  hstore_plluau                   |           |  1.0       |  Hstore transform for untrusted Lua                                                                                   |
|  hstore_plpython3u               |           |  1.0       |  transform between hstore and plpython3u                                                                              |
|  hypopg                          |  1.4.0    |  1.4.0     |  Hypothetical indexes for PostgreSQL                                                                                  |
|  icu_ext                         |  1.8      |  1.8       |  Access ICU functions                                                                                                 |
|  insert_username                 |  1.0      |  1.0       |  functions for tracking who changed a table                                                                           |
|  intagg                          |  1.1      |  1.1       |  integer aggregator and enumerator (obsolete)                                                                         |
|  intarray                        |  1.5      |  1.5       |  functions, operators, and index support for 1-D arrays of integers                                                   |
|  ip4r                            |  2.4      |  2.4       |                                                                                                                       |
|  isn                             |  1.2      |  1.2       |  data types for international product numbering standards                                                             |
|  jsonb_plpython3u                |           |  1.0       |  transform between jsonb and plpython3u                                                                               |
|  jsquery                         |  1.1      |  1.1       |  data type for jsonb inspection                                                                                       |
|  lo                              |  1.1      |  1.1       |  Large Object maintenance                                                                                             |
|  ltree                           |  1.2      |  1.2       |  data type for hierarchical tree-like structures                                                                      |
|  ltree_plpython3u                |           |  1.0       |  transform between ltree and plpython3u                                                                               |
|  mimeo                           |  1.5.1    |  1.5.1     |  Extension for specialized, per-table replication between PostgreSQL instances                                        |
|  moddatetime                     |  1.0      |  1.0       |  functions for tracking last modification time                                                                        |
|  mysql_fdw                       |  1.2      |  1.2       |  Foreign data wrapper for querying a MySQL server                                                                     |
|  ogr_fdw                         |  1.1      |  1.1       |  foreign-data wrapper for GIS data access                                                                             |
|  old_snapshot                    |  1.0      |  1.0       |  utilities in support of old_snapshot_threshold                                                                       |
|  oracle_fdw                      |  1.2      |  1.2       |  foreign data wrapper for Oracle access                                                                               |
|  orafce                          |  4.9      |  4.9       |  Functions and operators that emulate a subset of functions and packages from the Oracle RDBMS                        |
|  pageinspect                     |  1.12     |  1.12      |  inspect the contents of database pages at a low level                                                                |
|  pg_buffercache                  |  1.4      |  1.4       |  examine the shared buffer cache                                                                                      |
|  pg_cron                         |           |  1.6       |  Job scheduler for PostgreSQL                                                                                         |
|  pg_dirtyread                    |  2        |  2         |  Read dead but unvacuumed rows from table                                                                             |
|  pg_freespacemap                 |  1.2      |  1.2       |  examine the free space map (FSM)                                                                                     |
|  pg_graphql                      |           |  1.4.0     |  pg_graphql: GraphQL support                                                                                          |
|  pg_net                          |  0.8.0    |  0.8.0     |  Async HTTP                                                                                                           |
|  pg_partman                      |           |  5.0.1     |  Extension to manage partitioned tables by time or ID                                                                 |
|  pg_prewarm                      |  1.2      |  1.2       |  prewarm relation data                                                                                                |
|  pg_qualstats                    |  2.1.0    |  2.1.0     |  An extension collecting statistics about quals                                                                       |
|  pg_rational                     |  0.0.1    |  0.0.1     |  bigint fractions                                                                                                     |
|  pg_repack                       |  1.5.0    |  1.5.0     |  Reorganize tables in PostgreSQL databases with minimal locks                                                         |
|  pg_show_plans                   |  2.0      |  2.0       |  show query plans of all currently running SQL statements                                                             |
|  pg_similarity                   |  1.0      |  1.0       |  support similarity queries                                                                                           |
|  pg_sphere                       |  1.4.2    |  1.4.2     |  spherical objects with useful functions, operators and index support                                                 |
|  pg_squeeze                      |  1.6      |  1.6       |  A tool to remove unused space from a relation.                                                                       |
|  pg_stat_kcache                  |           |  2.2.3     |  Kernel statistics gathering                                                                                          |
|  pg_stat_statements              |  1.10     |  1.10      |  track planning and execution statistics of all SQL statements executed                                               |
|  pg_surgery                      |  1.0      |  1.0       |  extension to perform surgery on a damaged relation                                                                   |
|  pg_trgm                         |  1.6      |  1.6       |  text similarity measurement and index searching based on trigrams                                                    |
|  pg_visibility                   |  1.2      |  1.2       |  examine the visibility map (VM) and page-level visibility info                                                       |
|  pg_wait_sampling                |  1.1      |  1.1       |  sampling based statistics of wait events                                                                             |
|  pg_walinspect                   |  1.1      |  1.1       |  functions to inspect contents of PostgreSQL Write-Ahead Log                                                          |
|  pgagent                         |  4.2      |  4.2       |  A PostgreSQL job scheduler                                                                                           |
|  pgaudit                         |  16.0     |  16.0      |  provides auditing functionality                                                                                      |
|  pgautofailover                  |  2.1      |  2.1       |  pg_auto_failover                                                                                                     |
|  pgcrypto                        |  1.3      |  1.3       |  cryptographic functions                                                                                              |
|  pgfincore                       |  1.3.1    |  1.3.1     |  examine and manage the os buffer cache                                                                               |
|  pgmemcache                      |  2.3.0    |  2.3.0     |  memcached interface                                                                                                  |
|  pgmp                            |  1.1      |  1.1       |  Multiple Precision Arithmetic extension                                                                              |
|  pgpool_adm                      |  1.4      |  1.4       |  Administrative functions for pgPool                                                                                  |
|  pgpool_recovery                 |  1.4      |  1.4       |  recovery functions for pgpool-II for V4.3                                                                            |
|  pgpool_regclass                 |  1.0      |  1.0       |  replacement for regclass                                                                                             |
|  pgroonga                        |  3.1.9    |  3.1.9     |  Super fast and all languages supported full text search index based on Groonga                                       |
|  pgroonga_database               |  3.1.9    |  3.1.9     |  PGroonga database management module                                                                                  |
|  pgrouting                       |           |  3.6.2     |  pgRouting Extension                                                                                                  |
|  pgrowlocks                      |  1.2      |  1.2       |  show row-level locking information                                                                                   |
|  pgstattuple                     |  1.5      |  1.5       |  show tuple-level statistics                                                                                          |
|  pldbgapi                        |  1.1      |  1.1       |  server-side support for debugging PL/pgSQL functions                                                                 |
|  pljava                          |  1.6.7    |  1.6.7     |  PL/Java procedural language (https://tada.github.io/pljava/)                                                         |
|  pllua                           |  2.0      |  2.0       |  Lua as a procedural language                                                                                         |
|  plluau                          |  2.0      |  2.0       |  Lua as an untrusted procedural language                                                                              |
|  plpgsql                         |  1.0      |  1.0       |  PL/pgSQL procedural language                                                                                         |
|  plpgsql_check                   |  2.7      |  2.7       |  extended check for plpgsql functions                                                                                 |
|  plprofiler                      |  4.2      |  4.2       |  server-side support for profiling PL/pgSQL functions                                                                 |
|  plproxy                         |  2.11.0   |  2.11.0    |  Database partitioning implemented as procedural language                                                             |
|  plpython3u                      |  1.0      |  1.0       |  PL/Python3U untrusted procedural language                                                                            |
|  plr                             |  8.4.6    |  8.4.6     |  load R interpreter and execute R script from within a database                                                       |
|  pointcloud                      |  1.2.5    |  1.2.5     |  data type for lidar point clouds                                                                                     |
|  pointcloud_postgis              |           |  1.2.5     |  integration for pointcloud LIDAR data and PostGIS geometry data                                                      |
|  postgis                         |  3.4.2    |  3.4.2     |  PostGIS geometry and geography spatial types and functions                                                           |
|  postgis-3                       |           |  3.4.2     |  PostGIS geometry and geography spatial types and functions                                                           |
|  postgis_raster                  |  3.4.2    |  3.4.2     |  PostGIS raster types and functions                                                                                   |
|  postgis_raster-3                |           |  3.4.2     |  PostGIS raster types and functions                                                                                   |
|  postgis_sfcgal                  |  3.4.2    |  3.4.2     |  PostGIS SFCGAL functions                                                                                             |
|  postgis_sfcgal-3                |           |  3.4.2     |  PostGIS SFCGAL functions                                                                                             |
|  postgis_tiger_geocoder          |  3.4.2    |  3.4.2     |  PostGIS tiger geocoder and reverse geocoder                                                                          |
|  postgis_tiger_geocoder-3        |           |  3.4.2     |  PostGIS tiger geocoder and reverse geocoder                                                                          |
|  postgis_topology                |  3.4.2    |  3.4.2     |  PostGIS topology spatial types and functions                                                                         |
|  postgis_topology-3              |           |  3.4.2     |  PostGIS topology spatial types and functions                                                                         |
|  postgres_fdw                    |  1.1      |  1.1       |  foreign-data wrapper for remote PostgreSQL servers                                                                   |
|  powa                            |  4.2.2    |  4.2.2     |  PostgreSQL Workload Analyser-core                                                                                    |
|  pre_prepare                     |  0.4      |  0.4       |  Pre Prepare your Statement server side                                                                               |
|  prefix                          |  1.2.0    |  1.2.0     |  Prefix Range module for PostgreSQL                                                                                   |
|  prioritize                      |  1.0      |  1.0       |  get and set the priority of PostgreSQL backends                                                                      |
|  q3c                             |  2.0.1    |  2.0.1     |  q3c sky indexing plugin                                                                                              |
|  rdkit                           |  4.3.0    |  4.3.0     |  Cheminformatics functionality for PostgreSQL.                                                                        |
|  refint                          |  1.0      |  1.0       |  functions for implementing referential integrity (obsolete)                                                          |
|  rum                             |  1.3      |  1.3       |  RUM index access method                                                                                              |
|  seg                             |  1.4      |  1.4       |  data type for representing line segments or floating-point intervals                                                 |
|  sslinfo                         |  1.2      |  1.2       |  information about SSL certificates                                                                                   |
|  table_log                       |  0.6.1    |  0.6.1     |  Module to log changes on tables                                                                                      |
|  tablefunc                       |  1.0      |  1.0       |  functions that manipulate whole tables, including crosstab                                                           |
|  tcn                             |  1.0      |  1.0       |  Triggered change notifications                                                                                       |
|  tdigest                         |  1.4.1    |  1.4.1     |  Provides tdigest aggregate function.                                                                                 |
|  tds_fdw                         |  2.0.3    |  2.0.3     |  Foreign data wrapper for querying a TDS database (Sybase or Microsoft SQL Server)                                    |
|  timescaledb                     |  2.14.2   |  2.14.2    |  Enables scalable inserts and complex queries for time-series data (Community Edition)                                |
|  toastinfo                       |  1        |  1         |  show details on toasted datums                                                                                       |
|  tsm_system_rows                 |  1.0      |  1.0       |  TABLESAMPLE method which accepts number of rows as a limit                                                           |
|  tsm_system_time                 |  1.0      |  1.0       |  TABLESAMPLE method which accepts time in milliseconds as a limit                                                     |
|  unaccent                        |  1.1      |  1.1       |  text search dictionary that removes accents                                                                          |
|  unit                            |  7        |  7         |  SI units extension                                                                                                   |
|  uuid-ossp                       |  1.1      |  1.1       |  generate universally unique identifiers (UUIDs)                                                                      |
|  vector                          |  0.6.2    |  0.6.2     |  vector data type and ivfflat and hnsw access methods                                                                 |
|  xml2                            |  1.1      |  1.1       |  XPath querying and XSLT                                                                                              |
