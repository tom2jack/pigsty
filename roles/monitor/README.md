# Monitor (ansible role)

This role will install monitor component on target hosts
* node_exporter
* pg_exporter
* pgbouncer_exporter

### Tasks

[tasks/main.yml](tasks/main.yml)

```yaml
tasks:

```

### Default variables

[defaults/main.yml](defaults/main.yml)

```yaml
---
#------------------------------------------------------------------------------
# MONITOR PROVISION
#------------------------------------------------------------------------------
# - common - #
exporter_metrics_path: /metrics               # default metric path for pg related exporter
exporter_binary_install: false                # install exporter via copy local binary (from files/{pg,node}_exporter)

# - node exporter - #
node_exporter_enabled: true                   # setup node_exporter on instance
node_exporter_port: 9100                      # default port for node exporter

# - pg/pgb exporter - #
pg_exporter_config: pg_exporter-demo.yaml     # default config files for pg_exporter
pg_exporter_enabled: true                     # setup pg_exporter on instance
pgbouncer_exporter_enabled: true              # setup pgbouncer_exporter on instance
pg_exporter_port: 9630                        # default port for pg exporter
pgbouncer_exporter_port: 9631                 # default port for pgbouncer exporter

# - postgres variables reference - #
pg_port: 5432                                 # postgres port (5432 by default)
pgbouncer_port: 6432                          # pgbouncer port (6432 by default)
pg_localhost: /var/run/postgresql             # localhost unix socket dir for connection
pg_default_database: postgres                 # default database will be used as primary monitor target
pg_monitor_username: dbuser_monitor           # system monitor username, for postgres and pgbouncer
pg_monitor_password: DBUser.Monitor           # system monitor user's password
service_registry: consul                      # none | consul | etcd | both
...
```